+++
title = 'Migrating Application to K8S'
date = 2024-03-13T13:40:58+01:00
draft = false
summary = 'Dockerize all the things!'
+++

Now that I have the K3S cluster up and running, the time has come to adjust the Anex project to be deployable to it. You can find the code for this change [here](https://github.com/goblinhero/Anex/pull/33).

## Docker

I'm not going to explain the idea behind Docker - plenty of information about that online, already. Instead, I'm going to focus on the dockerfile that I ended up with after much tinkering - there are a lot of different ways to set it all up, and this is only one viable approach.

#### The entire dockerfile

```
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

COPY ["Anex.Api/Anex.Api.csproj", "Anex.Api/"]
RUN dotnet restore "Anex.Api/Anex.Api.csproj"

COPY ["Anex.Domain.Tests/Anex.Domain.Tests.csproj", "Anex.Domain.Tests/"]
RUN dotnet restore "Anex.Domain.Tests/Anex.Domain.Tests.csproj"

COPY . .

WORKDIR "/src/Anex.Domain.Tests"
RUN dotnet test "Anex.Domain.Tests.csproj"

WORKDIR "/src/Anex.Api"
RUN dotnet build "Anex.Api.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "Anex.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Anex.Api.dll"]
```

Quite a bit going on here, I'll try to explain my thoughts for each section, below.

#### Why two dotnet images?

```
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
```

So - there are basically two phases to creating the final image: Building the sourcecode into binaries and running these binaries in another image. Each has it's own prereqs and constraints. The build phase only happens once (at some point, in the CI/CD pipeline) while the final image will be used whenever a new container is spun up. While we are building the final image, we need access to the dotnet SDK (the build/sdk image above) - however, this is not needed once the application has been built and packaged. At that point we only need the base/aspnet image with the dotnet Core runtime. The result is that the docker image that will be used to have the app running in K3S will be much smaller and leaner.

#### Why three COPY commands?

```
WORKDIR /src

COPY ["Anex.Api/Anex.Api.csproj", "Anex.Api/"]
RUN dotnet restore "Anex.Api/Anex.Api.csproj"

COPY ["Anex.Domain.Tests/Anex.Domain.Tests.csproj", "Anex.Domain.Tests/"]
RUN dotnet restore "Anex.Domain.Tests/Anex.Domain.Tests.csproj"

COPY . .
```

So, this is solely a performance/storage optimization - Docker has multiple cached layers and detection of non-changed files. Basically, by only copying the .csproj files before doing `dotnet restore`, we can exploit the caching of NuGet packages.

The end-result is that at this point in the docker file, all the source code and referenced NuGet packages will be in the `/src` folder inside the build container.

#### Building and testing

```
WORKDIR "/src/Anex.Domain.Tests"
RUN dotnet test "Anex.Domain.Tests.csproj"

WORKDIR "/src/Anex.Api"
RUN dotnet build "Anex.Api.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "Anex.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Anex.Api.dll"]
```

So, the rest is fairly self-explanatory. First we run tests - we want it to fail as fast as possible if there are any broken tests. Next we build the API and then publish the resulting binaries into the `/app/publish` folder. Finally, we take the runtime image and create a new image including the binaries and set the entry point (which will be executed everytime a new container is created from the iamge).

## Conclusion

This dockerfile took a lot of tinkering to get right - and also to get it working in my local IDE for debugging - there are a gazzillion ways to get it to work, but I'm fairly satisfied with the result.