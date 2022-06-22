---
title: "🦊 Install a dependabot on GitLab for Free"
date: 2022-06-22T20:54:17+02:00
draft: false
---

# How to setup a dependabot

The goal is to set up a dependabot for free that automaticly create MR/PR for you

Github have that free feature for everyone with dependabot, that can send you alert and create for you Pull Request to correct your security issues  
Unfortunaly Gitlab doesn't provide this feature for free and the pricing to get it even with a self-managed application are high 💸

Let's see a free alternative then

Luckilly the Github dependabot is open source !  
https://github.com/dependabot

##  Create your own dependabot

We must first pull the depandabot-script project on our Gitlab space, go on :  
`Gitlab -> New Project -> Import project -> Repository by URL`  

And import that repository : https://github.com/dependabot/dependabot-script

Then rename `.gitlab-ci.example.yml` to `.gitlab-ci.yml`

You won't need to update any other file in that project

So now we have our script engine that will be used to check dependecies and will automaticly creates MR on Gitlab

## Setup API tokens

### Github token

You may be wondering why would we need a Github API token if we're doing a setup on Gitlab ?  
The script you just imported before are using Github API and especially the advisory database to check for vulnerabilities (https://github.com/advisories)

This token is not a requirement but Gitub API have really low quotas if you don't use any token, so I higly recommend you to use this token if you don't want your dependabot to break.

Go on Github and create an account if you don't have any, then you should go in your profile setting and at the bottom left you'll see a developper settings tab (https://github.com/settings/apps)

Then create a personal access token with this params :

![Gitub API token creation](/gitlab-dependabot/githubAPI.PNG)

Keep your Github API token for now as you won't be able to see it again

### Gitlab token

Gitlab token will be used to create MR to your projects

Go to your profile settings and go to "Access Tokens" tab.

Then create a token with the `api` scope

![Gitlab API token creation](/gitlab-dependabot/gitlabAPI.PNG)

As same at Github keep this token as you won't be able to access it after.

### Setup CI/CD variables

Go to your dependabot-script project again then in `Settings -> CI/CD` and expends `Variables`.

Here we're going to put the 2 tokens we just generated, create 2 variables called :

`GITHUB_ACCESS_TOKEN` and `GITLAB_ACCESS_TOKEN` with your respective tokens.  
I higly recommend you to mask this variables and do not protect them.

![Gitlab CI/CD variables](/gitlab-dependabot/gitlabVars.PNG)

If you're using Gitlab as Self-managed you should add one more var to define the url you're using :  
`GITLAB_HOSTNAME` and as value put you're domain name for example `gitlab.thomasdl.fr`

### Create your schedules by project

We're going to create scheduled pipelines to run it every weeks and also have the possibility to run this dependabot on multiple project

Go to `CI/CD -> Schedules` and add a new schedule.

I higly recommend you to setup it like this :
+ Start it every week
+ On your main branch
+ Keep it active

Before ending the schedule creation we have to set 2 variables:

`PROJECT_PATH` : that will be `<namespace>/<project>`  
`PACKAGE_MANAGER_SET` : that will be your package manager name (npm_and_yarn, maven, composer, ...)

![Gitlab schedule variables](/gitlab-dependabot/scheduleVar.PNG)

### (Optional) Create your own Gitlab runner

This step is optionnal but **I higly recommend you** to create and use your own runner than using shared runners.  
First, because this script may take some times and it will burn you usage quota and secondly because you'll be sure that your runner will be working as you configured it yourself

When register a new runner use docker to run it and set the default image to `docker:latest`

Then edit your `config.toml` located at `/etc/gitlab-runner` on Linux distro.  
Find your runner in this file and under `[runners.docker]` edit it like this : 
+ Set `privileged` to `true` instead of false
+ Set `volumes` to `volumes = ["/cache","/var/run/docker.sock:/var/run/docker.sock"]`
+ Add new option `pull_policy = "if-not-present"`

This will allow docker to work correctly, and use builded image in that docker instance instead of trying to find it remotly.


And then you should finally be ready to go 😉 !