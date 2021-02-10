Now that you've gotten Ruby into a container, we're going to start adding some common language tools into your container for use.

# Basic Ruby Tooling

Modern languages all have dependency managers. Ruby uses [Bundler](https://bundler.io/). For most languages and their ecosystems, dependency managers have a few important roles:

1. fetch libraries that your code depends on (e.g.: Ruby `gems`).
2. _lock_ your dependencies to specific versions so that building your app is reproduceable (usually through a file + lockfile).
3. in some cases, _execute_ runnable programs/executables/binaries that belong to your installed dependencies.

So for example you have two Ruby codebases. `CodeBaseA` & `CodeBaseB`. Both rely on [`rubocop`](https://rubocop.org/).

1. CodeBaseA relies on `rubocop` at version 1.0. 
2. CodeBaseB _also_ relies on `rubocop` but at version 2.0.

Now installing `rubocop` as a tool on your machine is definitely an option. This way you could go into any directory on your machine and just run `rubocop <file>`.

But let's say that the configuration for `rubocop` at version 1.0 is not compatible with version 2.0 and running `rubocop` in one directory works but it throws an error in the other.

This is where `bundler` comes in. In each directory for the codebases, there should be a `Gemfile` and `Gemfile.lock` that describes which version of `rubocop` each respective project is compatible with.

After you run `bundle` in each of the directories to pull down any versioned gems, you'll be able to run:

```sh
bundle exec rubocop
```

This will execute `rubocop` at version 1.0 when you're in the directory for `CodeBaseA`, vs version 2.0 in `CodeBaseB`

# Where Things Fall Apart

Ruby is a language that can have [native extentions](https://guides.rubygems.org/gems-with-extensions/). This means that you can actually write Ruby code that ends up calling executable binaries created from C code! Developers choose to do this from time-to-time to give their libraries performance boosts or utilize equivalent OS-specific calls across multiple platforms (e.g.: a kernel call in Linux may have an equivalent method in Windows, but under a different name, or would require different C libraries/headers).

This is where things can start falling apart. What if you're on a team that is spread out in terms of preferred operating systems? What if some of your team mates like to develop on Windows? Linux? Bundler should handle the installation correctly when you run `bundle install` in your project, but it can't handle the installation of Visual Studio std libs, or the equivalent headers on a Linux distro.

This is a problem that people run into often. [Just a glance on issues installing a Ruby XML parsing library Nokogiri](https://stackoverflow.com/questions/27496498/failing-to-install-nokogiri-gem) and you'll get a taste of the kinds of headaches that you can run into.

# Docker To the Rescue!

Docker can remove the problem above entirely. If you _do_ rely on libraries with native extentions all you have to do is ensure that they can be compiled and installed _ONCE_ in your Docker container. Your teammates can then download and use the container immediately for development, instead of having to worry about incompatibility issues!

<h3 align="center">Let's add some dependencies into your container!</h3>
<hr>

You'll see in this pull request that we've already prepped some files for you. Specifically the `Dockerfile` and `Gemfile`.

## Breakdown of Building Containers

Then [`Gemfile`]({{ repoUrl }}/blob/adding-gemfile/Gemfile) in this pull request should be straightfoward enough. It just specifies that we're going to install a `gem` called `minitest` (used for the next step).

The `Dockerfile` is a bit more interesting though.

For more information on how to build containers via Dockerfiles, check out [the documentation](https://docs.docker.com/engine/reference/builder/).

[A breakdown of the Dockerfile in this branch though]({{ repoUrl }}/blob/adding-gemfile/Dockerfile):

```sh
# this is from the last step! this provides ruby & bundler in your container
FROM ...

# this runs the command. Anything that the command does is SAVED into the image
RUN apk add make

# take all the code in this directory (that's the ".") and add it into the
# "/workspace" directory in the container 
ADD . /workspace

# All commands AFTER this line will take place in /workdir, the directory that
# was created above
WORKDIR /workspace

# run Bundler in /workspace. This will tell bundler to look at your Gemfile and
# install anything specified in there
RUN bundle install
```

## Steps

If you take a closer look at the [GitHub Actions config]({{ repoUrl }}/blob/adding-gemfile/.github/workflows/main.yml) you'll see that we've updated the CI test to now look for `rubocop`. It was already building and testing a container based on your `Dockerfile` so we'll just continue there. Don't worry, interacting and working _inside_ your container will be in the next step :D

So now let's try adding another dependency into the `Gemfile`!

All you have to do is add the line:

```ruby
gem 'rubocop'
```

Into your `Gemfile`. You can do so via [this link]({{ repoUrl }}/edit/adding-gemfile/Gemfile).
Like before, I'll respond after I detect a commit and see that it passes the CI checks :)

<hr>

> _Sometimes I respond too fast for the page to update! If you perform an expected action and don't see a response from me, wait a few seconds and refresh the page for your next steps._
