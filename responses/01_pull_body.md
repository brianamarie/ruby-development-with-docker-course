# Welcome!

## Pre-Requisites

For this lab you will need to have docker installed. We're not going to be doing anything crazy so just as long as you can run the following command you should be ok:

```sh
ruby-development-with-docker-course octocat$ docker images
REPOSITORY       TAG       IMAGE ID       CREATED       SIZE
```

If you need docker, you can install it [here](https://www.docker.com/products/docker-desktop).

Finally, this lab uses GitHub Actions to validate that you're completing the tasks! So make sure that you turn them on or you won't be able to progress. You can enable Actions:

- via the `Actions` tab at the top of this repo
- click `Enable Actions ...`

![enable actions](https://github.com/kran-learn-something-pls/ruby-development-with-docker-course/blob/main/assets/enable-actions.gif?raw=true)

## Installing Ruby...?

On some systems like macOS, Ruby comes installed. However, what if you're on a machine that does not have Ruby? Or if you want to use a different language instead? Crystal? Golang? Rust? Installation of language tools are trivial. However, if you're on a team that needs to move fast, sharing installation scripts, instructions or whatnot more-often-than-not requires _some_ overhead.

This is where Docker can come in handy. You can create and version containers with all dependencies built in. Almost as important is that you also spare your fellow team mates the task of going through local-machine setups, either through a wiki, doc or some other time-consuming provisioning scripts.

_this isn't a knock on machine provisioning tools btw, more just a comment on the number of things such tools would need to install; e.g.: `my ansible scripts install rails, postgres, redis, etc etc etc` vs `my ansible scripts install a specific version of docker - scripts for running things locally are defined in my repo`_

## Ok, Not Installing Ruby.

So now that you have docker you should be able to get started!

Most languages support official images for their languages. Ruby is one such example.

[Here's the list of Ruby images that Docker supports](https://hub.docker.com/_/ruby/?tab=tags&page=1&ordering=last_updated).

So instead of installing Ruby locally on your machine, we can simply pull down a Ruby image to use! Try this:

```sh
ruby-development-with-docker-course octocat$  docker run --rm ruby:alpine ruby --version
ruby 3.0.0p0 (2020-12-25 revision 95aff21468) [x86_64-linux-musl]
```

This command:
1. downloaded the `ruby:alpine` image from Docker Hub
2. ran the command `ruby --version` in that container, outputting the version
3. exited and removed the container

_NOTE: a `container` is a running **instance** of the `image` - so after this command, the `ruby:alpine` image still exists on your machine_

The nice thing about this image is that it also includes `gem` and `bundle` - commandline tools for Ruby that help you manage libraries (gems)!

<h3 align="center">Now let's build your Docker container!</h3>
<hr>

`Wait, but why build a container when I already have one?`

Good question! You can build Docker containers that are _based_ on other containers (kind of like "parent" containers). When you build a Docker container, you not only get all of the installed tools/commands that are in your "parent" container, but you can also _add_ code and other commands. We're going to start setting up a container for this exact purpose.

In this pull-request there is already a `Dockerfile` for you to edit. Based on your requirements for Ruby, update the `FROM` command in the Docker. If you don't know what to do though, feel free to copy this bit:

```Dockerfile
FROM ruby:alpine3.13
```

## Steps
1. [Edit the Dockerfile in this branch]({{ repoUrl }}/edit/add-ruby-to-dockerfile/Dockerfile) and commit. Doing that will update this pull-request
2. I'll respond after I detect a commit and see that it passes the CI checks :)


<hr>

> _Sometimes I respond too fast for the page to update! If you perform an expected action and don't see a response from me, wait a few seconds and refresh the page for your next steps._
