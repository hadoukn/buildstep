# Buildstep

Heroku-style application builds using Docker and Buildpacks. Used by [Dokku](https://github.com/progrium/dokku) to make a mini-Heroku.

## Requirements

 * Docker
 * Git

## Supported Buildpacks

Buildpacks should generally just work, but many of them make assumptions about their environment. So Buildstep has a list of officially supported buildpacks that are built-in and ready to be used.

 * [Ruby](https://github.com/heroku/heroku-buildpack-ruby)
 * [Node.js](https://github.com/heroku/heroku-buildpack-nodejs)
 * [Java](https://github.com/heroku/heroku-buildpack-java)
 * [Play!](https://github.com/heroku/heroku-buildpack-play)
 * [Python](https://github.com/heroku/heroku-buildpack-python)
 * [PHP](https://github.com/heroku/heroku-buildpack-php.git)
 * [Go](https://github.com/kr/heroku-buildpack-go.git)

## Building Buildstep

The buildstep script uses a buildstep base container that needs to be built. It must be created before
you can use the buildstep script. To create it, run:

    $ docker build -t progrium/buildstep .

This will create a container called `progrium/buildstep` that contains all supported buildpacks and the
builder script that will actually perform the build using the buildpacks.

## Building an App

Running the buildstep script will take an application tar via STDIN and an application container name as
an argument. It will put the application in a new container based on `progrium/buildstep` with the specified name. 
Then it runs the builder script inside the container. 

    $ cat myapp.tar | ./buildstep myapp

The resulting container has a built app ready to go. The builder script also parses the Procfile and produces
a starter script that takes a process type. Run your app with:

    $ docker run -d myapp /bin/bash -c "/start web"

## Adding Buildpacks

Buildstep needs to support a buildpack by installing packages needed to run the build and to run the application
it builds. For example, the Python buildpack would need Python to be installed.

To add a new buildpack to buildstep, add commands to install the necessary packages that the buildpack and built
application environment will need to Dockerfile.

You'll then have to re-build.

## License

MIT
