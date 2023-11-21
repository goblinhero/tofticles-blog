+++
title = 'Databases in Azure'
date = 2023-11-21T08:16:59+01:00
draft = false
summary = 'Moving to PostGres in the cloud (with diamonds).'
+++

Only a few changes needed to get this running - you can see them [here](https://github.com/goblinhero/Anex/pull/11) and [here](https://github.com/goblinhero/Anex/pull/12).

It might be a few changes, but it took quite a bit of tinkering to get working. I have limited experience with PostGres and really did not want to have a local installation. So, I went with the more annoying Docker solution to be able to test locally.

In case, you want to try and get it up and running, locally - here are the steps, I took:

### Get Docker Desktop

Fairly straight-forward - go to their [download page](https://www.docker.com/products/docker-desktop/) - I did a next-next-next-finish install to get up and running.

### Get the PostGres image and run it

In your favorite command line, run this command:

    docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -d -p 5432:5432 postgres

    --name  is the name of the container in Docker
    -e      is the environment variables - for PostGres options, POSTGRES_PASSWORD is the variable holding the root password
    -d      run in detached mode. Basically means that the container keeps running without you having to keep the command prompt alive
    -p      is the port that is mapped from inside the docker container to the local host and enables connections from outside Docker

### Connect to the PostGres instance and create the database 

Choose whichever editor you prefer. In there simply create a blank database called `Anex_DB`. 

### Run the Anex application locally

This will trigger the `.Initialize()` call which will drop the tables and recreate them. 

Now, this is obviously not how we are going to run in production - users tend to be miffed, if they have to recreate the data everytime you restart the application. At this state of development, this is fine, however, and probably even beneficial.