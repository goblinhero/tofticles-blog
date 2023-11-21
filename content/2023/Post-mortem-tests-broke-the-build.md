+++
title = 'Post Mortem: The tests broke the Build'
date = 2023-11-21T14:50:42+01:00
draft = false
summary = 'The intentions were good... Execution, not so much'
+++

The code for this post can be found [here](https://github.com/goblinhero/Anex/pull/16).

I added tests to the project (you can find the introduction [here](https://tofticles.dk/2023/adding-tests-to-pipeline/)) and managed to completely miss that I broke the build. I first noticed when I was trying to test the real database in production and suddenly my api was no where to be found.

### First rule of panic club - blame the host

A few commits had been added to the main branch since I added the test, and the tests weren't really on my radar as the culprit. I had added Application Insights to the project in the mean time, so I was -sure- that must have been the breaking change. So, after cleaning up after that (and that is quite a lot of crap it adds in the environment) - still no dice. Honestly, I'm still not entirely sure if I've removed everything relating to Application Insights.

### Rolling back. What do you mean 'no'?!

Next step was trying to roll back until I found a working version. And this is when I found out that such is not supported. I mean .. wat?! You have to have deployment slots or run on the Kubernetes platform to have such cutting-edge tech. And with deployment slots, you get one shot. Tough shit if you find out two versions or later down the line. So, yeah - don't use Web Apps for Production if you do releases by CI/CD. It's simply too risky.

### Searching for needles in the haystack

So. Back to square one we go. Looking at log files - every developer's favourite past-time. I knew it had to have something to do with the deployment - but I had no errors, anywhere. Github Actions seemed to be a good place to start and after combing through them, I stumbled over this little gem:

    The "--output" option isn't supported when building a solution. Specifying a solution-level output path results in all projects copying outputs to the same directory, which can lead to inconsistent builds.

I had a smoking gun. Or at least some smoke. Basically, what was happening was that the build-output of all my (three) assemblies was jumbled into one folder, which isn't exactly ideal.

### The (temporary) solution

In the next week that contains two Thursdays, I'll create the build script from scratch to fully understand how to tailor it to my needs, but for now, I'll go with the band-aid solution: Excluding the test project from the published output, by inserting the following line in the `.csproj` file:

    <IsPublishable>false</IsPublishable>

And we're back online. It all just goes to show: Might be a good idea to test once in a while if your production site is still working after every deploy. You might have broken it in ways that do not show up as blinding, rotating lights.