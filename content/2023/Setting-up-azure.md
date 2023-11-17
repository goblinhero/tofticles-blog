+++
title = 'Setting Up Azure'
date = 2023-11-17T12:11:02+01:00
draft = false
summary = 'Goodbye Docker!'
+++

So, this took more fiddling, than I would have liked, but hey-ho. I started by creating a new web app on Azure (I used the Web App + Database resource). This took quite a bit, but worked in the end. Next up was linking the Github repo to Azure. I tried different approaches:

### Dockerized, setting it up with a .yml Github Action file, tries 1-8

This requires a personal access token to the Github repo to be stored in Azure along with a few extra parameters. My structure did not work out of the box (I had to prefix the path to my docker file - Visual Studio places it in the project path, not the solution path), and once I got the action to find the docker file, I got an error that it could not find my csproj file (which does not make sense since the dockerfile works locally). After some fiddling, I decided not to pursue it further as the error message was very unhelpful and searching online for it pulled a lot of duds.

### Creating the Github action from Azure, still dockerized

Next try was setting up the pipeline from Azure. This was fairly painless setting up, and I could see a new Github Action. Great. Next was pushing a commit to trigger it - since I needed to delete the old Github action, that was a prime target for the trigger-commit. Action failed: "No .sln files found". Okay, this was not what I expected since there was no hint when creating it - so back to the drawing board.

### Adjusting the solution back to using the .sln file for building

That simply took moving everything to the root for the action to find the .sln file (I had gone with the default structure of having a /src path). Pushed the commit and there was much rejoycing - the app finally shows the Swagger inferface once again, but now in Azure. So, nothing much to show for the code - it was mostly cleaning up after docker and removing all traces of it.

I'm sure there are guides how to do this, but trust me, I've found a number of guides that are a) out-of-date, b) point to no-longer viable(possible) solutions or c) were written using Visual Studio Publish - which I refuse to use. I need my CI/CD. Next part will focus on adjusting the connection string to the new and shiny Azure database which will run MySql. Today's history lesson is that it is not pronounced My SQL like if you had ownership of it. My is the name of founder Michael Widenius' daughter which pronounced in Swedish sounds more like the scientific unit my (also known as a micron) which is 1000th of a millimetre. His second daughter is called Maria - hence MariaDB.