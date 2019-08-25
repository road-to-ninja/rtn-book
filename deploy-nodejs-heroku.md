Deploy NodeJS to Heroku
===

This doc is based on MacOS

## Prerequisites

- Heroku account (free)
- Homebrew (mac package manager)
- Node/Npm

## Install CLI Heroku

First install heroku CLI heroku with brew

```console
 $ brew install heroku/brew/heroku
```

## Login

Enter your heroku account credentials

```console
 $ heroku login
```

## Create Heroku project

```console
 $ heroku create [your-app-name]
```
After this heroku will create a remote link server to your heroku server

## Push to heroku

```console
$ git push heroku master
```

Maybe you will need to specify buildpack. Do it like this:

```console
$ heroku buildpacks:set heroku/nodejs
```

## Test

The URL to your app is `https://[your-name].herokuapp.com` 


## Configs

Most of the time you have different environnement variables values. Your Database URL is not the same for the example when you are in production or in local.

To setup environnement keys:

```console
$ heroku config:set [Y_ENV_VALUES]=[value]
example
$ heroku config:set DB_URI= mongodb+srv://server:password@cluster.net
```

to see configs:
```console
$ heroku config
```