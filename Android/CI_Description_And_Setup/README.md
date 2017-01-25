# Continuous Integration

## What is CI?

Please refer to [this Wikipedia article](https://en.wikipedia.org/wiki/Continuous_integration) and related sources. In general, it's "this thing that automatically builds your code, runs tests and lints for you". Here I'll focus on explaining how to set it up for our Android GitLab projects. 

You should only ever be reading this if:

* You're curious as to how to set it up, OR
* You're a manager and have to set it up. 

(if you're in both of these categories, you're good too :-)).

## What are we using?

We are using the CI integrated with GitLab. To read about GitLab CI in general, refer to [this page](https://about.gitlab.com/gitlab-ci/). Here we will focus on the setup for android projects. 

## How does CI work and why do I care?

Every CI must run the code you provide within a [virtual machine](https://en.wikipedia.org/wiki/Virtual_machine) or [container](https://en.wikipedia.org/wiki/Operating-system-level_virtualization). What does that mean?

First, let's establish the basics. From the user perspective (your perspective) this is what happens:

1. You push your code to the repo, 
2. The GitLab CI runner will detect this fact
3. The Gitlab CI runner will initiate a series of events according to a special file we will be considering later on (called .gitlab-ci.yml).

Pretty cool, huh? Everything happens automatically and it's magic. But... not so fast.

Step 3 is actually much more complex than it sounds. The series of events you are initiating is being run on some server which is shared across many users (who have no clue that they aren't the only user!). This means that there must be some program which controls what is being run, when it is being run and who should get access to which resources (such as CPUs, RAM etc.) Such a program is called a [hypervisor](https://en.wikipedia.org/wiki/Hypervisor) or container manager (the difference depends on whether VMs or containers are used). To be more brief, I'll go on using the VM and hypervisor terminology as everything that follows translates easily to containers.

Hence, a better approximation of what actually happens for step 3 is the following:

1. The hypervisor running on the server launches a virtual machine with given resource constraints (usually based on how much you pay for the ci)
2. The hypervisor then executes the series of commands you have requested within .gitlab-ci.yml within the virtual machine

Hence, the user has no clue that he or she doesn't own the machine that is being used to execute what he or she requests. Moreover, the user has no clue what operating system is actually being run on that machine since he or she operates on the virtual machine (which does the job of interfacing with the real operating system for you!). 

The bottom line here is that we must specify what virtual machine the hypervisor should launch for us. This is why we care about this whole issue and it also happens to be the reason for which we need Docker. 

## What is Docker?

First, see some of [this wiki page](https://en.wikipedia.org/wiki/Docker_(software)) remembering that for the purpose of this discussion containers and virtual machines are the same (in reality [they are not](http://electronicdesign.com/dev-tools/what-s-difference-between-containers-and-virtual-machines)).

In short, for our purpose Docker is what we will use to describe and create virtual machine images. In other words, it will be the software we use to describe and create images of the virtual machines in which we will be running tests.

## You will need docker...

Please instal docker refering to these instructions for [Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04) or [macOS](https://docs.docker.com/docker-for-mac/)).

## Setting up CI

Setting up CI will require the following steps:

1. Modify the `Dockerfile` file
2. Log in to the repo using `docker login yale.githost.io:4678`
3. Build the docker image using `docker build -t yale.githost.io:4678/sdmp/yalemobile-android .`
4. Push the docker image using `docker push yale.githost.io:4678/sdmp/yalemobile-android`
5. Update the .gitlab-ci.yml file to reflect the behavior of your CI

Steps 2 - 4 are self-explanatory, so I won't spend time on them. Let's talk about Steps 1 and 5.

## Create the image you need...

In general, this is rather involved and [this tutorial](https://docs.docker.com/engine/getstarted/) should help you get the grasp of it.

You may also just use the current dockerfile as a template to modify. Please refer to the dockerfile [here](https://yale.githost.io/SDMP/YaleMobile-Android/blob/master/Dockerfile).

In general, when you write docker files for Android projects you will need to base them on unix systems with installed java. Some base images are widely available (such as opejdk:8-jdk), which will do the job. Then, on top of that you need to install all of the necessary android stuff (SDK, tooling, images...). 

There are few more tricks that you might find handy. 

1. If you want to run the image you have built locally to test something, use `docker run -it yale.githost.io:4678/sdmp/yalemobile-android`. Exiting the interactive run may be done using `CTRL + D` or the `exit` command.
2. If you want to delete the image from your local machine to free up some space, use:
    - `docker rm $(docker ps -a -q)` -- this will stop all of docker jobs
    - `docker images` -- this will list your images
    - `docker rmi <hashsum>` -- this will delete the image from your local workspace.

## Updating .gitlab-ci.yml

There is a relatively good guide for creating and modifying this file [here](https://about.gitlab.com/2016/11/30/setting-up-gitlab-ci-for-android-projects/). Keep in mind that the tutorial uses the raw `openjdk-8:jdk` docker image so they download a bunch of things you don't need to! Just make sure to use the docker image you have uploaded before (see the top image directive?). Otherwise it is relatively similar. 

After you went through that tutorial, take a look at the [current `.gitlab-ci.yml` file](https://yale.githost.io/SDMP/YaleMobile-Android/blob/master/.gitlab-ci.yml), which is much better commented. That should give you the idea of what instructions it specifies. Modify whatever you need to!
