# Heroku Buildpack: Quarantyne [![Build Status](https://travis-ci.org/quarantyne/heroku-buildpack-quarantyne.svg?branch=master)](https://travis-ci.org/quarantyne/heroku-buildpack-quarantyne)
A [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) to install [**Quarantyne**](https://github.com/quarantyne/quarantyne) on a heroku dyno. **Quarantyne** is an open-source layer 7 firewall that protects websites and APIs from bots, fraudulent behavior, application misuse, and cyber-attacks.

## Requirements
Quarantyne requires a JVM to function. If your app is not a JVM app (java, scala, clojure), you must install the official [JVM](https://elements.heroku.com/buildpacks/heroku/heroku-buildpack-jvm-common) buildpack from Heroku.

## Installation
Add this buildpack to any heroku app with

    $ heroku buildpacks:set https://github.com/quarantyne/heroku-buildpack-quarantyne

## Usage
Quarantyne works either as a solo dyno process or in-front of an existing app. Edit your `Procfile` according to the following scenarios.

### Quick demo
    # 1) default egress is httpbin.org.
    # 2) Use the sample traffic blocking config we provide
    web: java -jar quarantyne.jar --ingress 0.0.0.0:$PORT --config-file https://s3-us-west-2.amazonaws.com/releases.quarantyne.com/quarantyne.json 

### With app on same dyno
    # 1) Run your app in a different process
    web: java -jar quarantyne.jar --ingress 0.0.0.0:$PORT --egress 127.0.0.1:3000 --config-file https://s3-us-west-2.amazonaws.com/releases.quarantyne.com/quarantyne.json 
    app: bundle exec unicorn -p 3000 -b127.0.0.1 -c ./config/unicorn.rb
    
### With app on different dyno/host
    # beware of extra latency incurred between quarantyne on this dyno and the remote host
    web: java -jar quarantyne.jar --ingress 0.0.0.0:$PORT --egress app.example.com --config-file https://s3-us-west-2.amazonaws.com/releases.quarantyne.com/quarantyne.json 

## Support
Please file an issue in this project if related to the buildpack only. Otherwise please file a [Quarantyne issue](https://github.com/quarantyne/quarantyne/issues)
