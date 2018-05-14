# Heroku buildpack: `l2met-shuttle`

This is a [Heroku buildpack][buildpack] that allows an app to use [l2met-shuttle][] to extract metrics and deliver them to a configured URL.

[buildpack]: https://devcenter.heroku.com/articles/buildpacks
[l2met-shuttle]: https://github.com/heroku/l2met-shuttle

## Usage

First you need to set this buildpack as your initial buildpack with:

    $ heroku buildpacks:add -i 1 https://github.com/heroku/heroku-buildpack-l2met-shuttle.git

You also need to define the URL of your [L2met][l2met] compatible endpoint:

    $ heroku config:set L2MET_SHUTTLE_URL=<url>

Next, for each process that emit metrics, you will need to preface the command in your Procfile with `start-l2met-shuttle`. In this example, we want only the web process to send metrics through `l2met-shuttle`.

    $ cat Procfile
    web:    ./bin/start-l2met-shuttle bundle exec unicorn -p $PORT -c ./config/unicorn.rb -E $RACK_ENV
    worker: bundle exec rake worker

Rebuild your app for the changes to take effect.

[l2met]: https://github.com/ryandotsmith/l2met
