+++
title = 'Adding Tests to Pipeline'
date = 2023-11-20T14:43:42+01:00
draft = false
summary = 'Small work -> Big impact'
+++

I'm still running with the default build-script for a Github action, so basically haven't touched it yet. Adding a testing phase turned out to be really easy:

For the section here (see the complete file [here](https://github.com/goblinhero/Anex/blob/main/.github/workflows/main_anexerp.yml)):

        <snip>
          include-prerelease: true

      - name: Build with dotnet
        run: dotnet build --configuration Release

      - name: dotnet publish
        <snip>

We simply add a step to have testing:

        <snip>
          include-prerelease: true

      - name: Build with dotnet
        run: dotnet build --configuration Release

      - name: Test with dotnet
        run: dotnet test --configuration Release

      - name: dotnet publish
        <snip>

And since altering the script is considered a pull request - we get sorta instant feedback from the logs (again, full log [here](https://github.com/goblinhero/Anex/actions/runs/6930930691/job/18851558694)): 


    <snip>
    Run dotnet test --configuration Release
    Determining projects to restore...
    All projects are up-to-date for restore.
    Anex.Domain -> /home/runner/work/Anex/Anex/Anex.Domain/bin/Release/net8.0/Anex.Domain.dll
    Anex.Domain.Tests -> /home/runner/work/Anex/Anex/Anex.Domain.Tests/bin/Release/net8.0/Anex.Domain.Tests.dll

    <snip>

    Starting test execution, please wait...
    A total of 1 test files matched the specified pattern.

    Passed!  - Failed:     0, Passed:     4, Skipped:     0, Total:     4, Duration: 9 ms - Anex.Domain.Tests.dll (net8.0)
    <snip>

In the eternal words of Borat: "Great success!" - we now have CI/CD. Next up is adding a lot of database stuff and testing the API.