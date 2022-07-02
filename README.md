# Heroku Multi heroku.yml buildpack

_tl;dr_ -- The idea here is that you have a single git repository, but multiple Heroku apps. In other words, you want to share a single git repository to power multiple Heroku apps. So, for each app you need this buildpack, and for each app, you need to set a config variable named `HEROKUYML` to the location where the `heroku.yml` is for that app. As an example:

```
$ heroku create -a example-1
$ heroku create -a example-2
$ heroku buildpacks:add -a example-1 jamesarobbo/multi-herokuyml
$ heroku buildpacks:add -a example-2 jamesarobbo/multi-herokuyml
$ heroku config:set -a example-1 HEROKUYML=heroku.yml
$ heroku config:set -a example-2 HEROKUYML=backend/heroku.yml
$ git push https://git.heroku.com/example-1.git HEAD:main
$ git push https://git.heroku.com/example-2.git HEAD:main
```

When example-1 builds, it'll copy `heroku.yml` into `/app/heroku.yml`, and when example-2 builds, it'll copy `backend/heroku.yml` to `/app/heroku.yml`. For example-2, the process types available for you to scale up will be the ones referenced (originally) in `backend/heroku.yml`.
