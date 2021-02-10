Sorry, but when inspecting the built container I could not run `bundle exec rubocop` successfully.
Make sure that you've:

1. updated your Gemfile with `gem 'rubocop'`
2. and that your Docker image can be built via `docker build --tag test .`
