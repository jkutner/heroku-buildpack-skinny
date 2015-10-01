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

## Customizing

By default, the buildpack runs your app with a command like:

```
java $JAVA_OPTS -Dskinny.port=$PORT -Dskinny.env=production -Ddb.default.url=$JDBC_DATABASE_URL -Ddb.default.user=$JDBC_DATABASE_USER -Ddb.default.password=$JDBC_DATABASE_PASSWORD -jar standalone-build/target/scala-2.11/*.jar
```

You can customize this by setting `JAVA_OPTS` like this:

```
$ heroku config:set JAVA_OPTS=""
```

Or you can create a [Procfile] in the root directory with contents like this:

```
web: java \$JAVA_OPTS -Dskinny.port=\$PORT -Dskinny.env=production -Ddb.default.url=\$JDBC_DATABASE_URL -Ddb.default.user=\$JDBC_DATABASE_USER -Ddb.default.password=\$JDBC_DATABASE_PASSWORD -jar standalone-build/target/scala-2.11/*.jar
```

Then add the Procfile to Git and redeploy with `git push heroku master`.

## License

MIT