# Heroku Skinny Buildpack

This is a [Heroku buildpack](https://devcenter.heroku.com/articles/buildpacks) for the [Skinny framework](http://skinny-framework.org/).

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

The first deploy will take a long time because it must download all dependencies. But they will be cached, so the next deploy will be faster.

## Database

The buildpack will automatically set up the connection URL for you, but you may need to include the PostgreSQL (or other vendor's) driver
in your dependencies. Addd something like this to `project/Build.scala`:

```
libraryDependencies ++= Seq(
  "org.postgresql" % "postgresql" % "9.4-1201-jdbc41"
)
```

To run migrations, configure your `src/main/resources/application.conf` with something like this:

```groovy
production {
  db {
    default {
      driver="org.postgresql.Driver"
      url=${?JDBC_DATABASE_URL}
    }
  }
}
```

You can get the value for `$JDBC_DATABASE_URL` by running `heroku run echo \$JDBC_DATABASE_URL`. Set that value locally, but add
the arguments `ssl=true&sslfactory=org.postgresql.ssl.NonValidatingFactory`, and then run:

```
./skinny db:migrate production default
```

## Customizing

By default, the buildpack runs your app with a command like:

```
java $JAVA_OPTS -Dskinny.port=$PORT -Dskinny.env=production -Ddb.default.url=$JDBC_DATABASE_URL -Ddb.default.user=$JDBC_DATABASE_USER -Ddb.default.password=$JDBC_DATABASE_PASSWORD -jar standalone-build/target/scala-2.11/*.jar
```

You can customize this by setting `JAVA_OPTS` like this:

```
$ heroku config:set JAVA_OPTS="-XX:+UseCompressedOops"
```

Or you can create a [Procfile](https://devcenter.heroku.com/articles/procfile) in the root directory with contents like this:

```
web: java \$JAVA_OPTS -Dskinny.port=\$PORT -Dskinny.env=production -Ddb.default.url=\$JDBC_DATABASE_URL -Ddb.default.user=\$JDBC_DATABASE_USER -Ddb.default.password=\$JDBC_DATABASE_PASSWORD -jar standalone-build/target/scala-2.11/*.jar
```

Then add the Procfile to Git and redeploy with `git push heroku master`.

## License

MIT