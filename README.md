# Scripts To Rule Them All

Inspired by [github/scripts-to-rule-them-all](https://github.com/github/scripts-to-rule-them-all).
This is a set of boilerplate scripts based on the [normalized script pattern
that GitHub uses in its projects](http://githubengineering.com/scripts-to-rule-them-all/). While
these patterns can work for projects based on any framework or language, these particular examples
are for a simple ***Python*** application.

## The Idea

If your scripts are normalized by name across all of your projects, your contributors only need to
know the pattern, not a deep knowledge of the application. This means they can jump into a project
and make contributions without first learning how to bootstrap the project or how to get its tests
to run.

The intricacies of things like test commands and bootstrapping can be managed by maintainers, who
have a rich understanding of the project's domain. Individual contributors need only to know the
patterns and can simply run the commands and get what they expect.

## The Scripts

Each of these scripts is responsible for a unit of work. This way they can be called from other
scripts.

This not only cleans up a lot of duplicated effort, it means contributors can do the things they
need to do, without having an extensive fundamental knowledge of how the project works. Lowering
friction like this is key to faster and happier contributions.

The following is a list of scripts and their primary responsibilities.

### script/bootstrap

[`script/bootstrap`][bootstrap] is used solely for fulfilling dependencies of the project.

This can mean RubyGems, npm packages, Homebrew packages, Ruby versions, Git submodules, etc. For
this Python-based repository, this means installing Python packages into a virtual environment.

The goal is to make sure all required dependencies are installed.

### script/clean

[`script/clean`][clean] is used to clean the project directory and other services the project
interacts with.

The script may implement command line options that clean specific services and/or "deep clean" the
system removing all artifacts created by the project.

### script/setup

[`script/setup`][setup] is used to set up a project in an initial state. This is typically run
after an initial clone, or, to reset the project back to its initial state.

This is also useful for ensuring that your bootstrapping actually works well.

### script/update

[`script/update`][update] is used to update the project after a fresh pull.

If you have not worked on the project for a while, running [`script/update`][update] after a pull
will ensure that everything inside the project is up to date and ready to work.

The script may implement a `--dependencies` command line option that locks and installs updated
project dependencies.

Typically, [`script/clean`][clean] and [`script/bootstrap`][bootstrap] are run inside this script.
This is also a good opportunity to run database migrations or any other things required to get the
state of the app into shape for the current version that is checked out.

### script/build

[`script/build`][build] is used to build the application and its artifacts.

This can mean building a Python package, Docker image, etc.

If the project has multiple build products, for example a project that builds a Python package and
generates documentation, the script may implement command line options to control which products
are built.

### script/server

[`script/server`][server] is used to start the application.

For a web application, this might start up any extra processes that the application requires to run
in addition to itself.

[`script/update`][update] should be called ahead of any application booting to ensure that the
application is up to date and can run appropriately.

### script/test

[`script/test`][test] is used to run the test suite of the application.

A good pattern to support is having an optional argument that is a file path. This allows you to
support running single tests.

Linting (i.e. pylint, flake8) can also be considered a form of testing. These tend to run faster
than tests, so put them towards the beginning of a [`script/test`][test] so it fails faster if
there's a linting problem.

[`script/test`][test] should be called from [`script/cibuild`][cibuild], so it should handle
setting up the application appropriately based on the environment. For example, if called in a
development environment, it should probably call [`script/update`][update] to always ensure that
the application is up to date. If called from [`script/cibuild`][cibuild], it should probably reset
the application to a clean state.

### script/ci

[`script/ci`][ci] is used for your continuous integration server. This script is typically only
called from your CI server.

You should set up any specific things for your environment here before your tests are run. Your
test are run simply by calling [`script/test`][test].

### script/console

[`script/console`][console] is used to open a console for your application.

A good pattern to support is having an optional argument that is an environment name, so you can
connect to that environment's console.

You should configure and run anything that needs to happen to open a console for the requested
environment.

[bootstrap]: script/bootstrap
[clean]: script/clean
[setup]: script/setup
[update]: script/update
[build]: script/build
[server]: script/server
[test]: script/test
[ci]: script/cibuild
[console]: script/console
