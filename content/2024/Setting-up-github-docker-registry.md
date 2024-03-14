+++
title = 'Setting up Github Docker Registry'
date = 2024-03-14T07:27:27+01:00
draft = false
summary = 'Wonder what this button does...'
+++

Now that I have a dockerfile, I need to use this for something. The end goal is to be able to pull the images from a registry, generate a container from it and then deploy it to the K3S cluster. 

## The outline of the solution

I want to have the pipeline build the image from the dockerfile and push it to a registry somewhere, where my 'deployment centre' (Okay, for now it is just me pushing a button) can pull the image and deploy a new version.

#### Why not automatic deployment?

Well, for a development/testing environment, sure - I will do that later. But to get there, I either need to have a local registry and git repository (since my deployment centre is not on the public internet, so triggering it from a Github Action is an issue) or some kind of public access to my setup, which I am not terribly keen on.

Having things deploy directly to production, I believe to be counter to the needs of everyone but developers.

#### Choosing a registry

So, it used to be easy to choose [Docker Hub](https://hub.docker.com/). However, I have both technical, economic and moral issues with Docker as a company. I loathe the VC-backed approach to the world. It produces all the wrong results and motivations. I am not terribly up to date with their shenanigans, but it seems Docker has decided to delete several free organizations as well as using trackers, extensively. No, thank you.

Then there's Azure, AWS and Google. Yeah, no. I'm actively trying to avoid the big three cloud things.

So, that leaves Gitlab and Github for public registries that I am familiar with - while I'm by no means fond of Github, I will for now use their registry while I plan my escape completely from the clutches of them. For one, since I already have the code hosted on Github, pushing to the registry is seamless from a Github Action. 

#### Setting up a Github Action

As is probably evident from the [repository](https://github.com/goblinhero/Anex/blob/main/.github/workflows/docker-publish.yml) - this is basically just an applied template - I did very little. Mostly, I just had to move the dockerfile into the root of the project to avoid having to adjust the paths in the docker file. 

Very seamless experience and quite a lot easier to get up and running than anticipated. And now you can see the very [first](https://github.com/goblinhero/Anex/pkgs/container/anex/190450843?tag=main) image of Anex (it currently does not have a connection to a valid database and that will be the next hurdle).

Still a lot to do with semantic versioning and general fleshing out of description and such, but for now, I just needed to get a registry that I can pull from.