# Heroku Skinny Buildpack

This is a [Heroku buildpack]() for the [Skinny framework](http://skinny-framework.org/).

## Usage

Install the [Heroku Toolbelt](https://toolbelt.heroku.com/) and create a free Heroku account. Then
login in with that account by running:

```
$ heroku login
```

Make sure you have a Git repo for you app. From the root of your project run:

```
$ git init
$ git add .
$ git commit -am "deploy to heroku"
```

Then create a Heroku app:

```
$ heroku create
```

Set this buildpack as the default buildpack:

```
$ heroku buildpacks:set https://github.com/jkutner/heroku-buildpack-skinny
```

And push your app to Heroku:

```
$ git push heroku master
```

## License

MIT