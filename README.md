Homebase
========

<img src="http://i.imgur.com/Pa5lhV2.gif" />

Homebase is a tool to help San Antonio homeowners fix up their homes. The current version is focused on streamlining the permit selection and application process for basic renovation projects, like building additions or fixing up doors.

This software is under development by the Code for America San Antonio fellowship team in collaboration with the City of San Antonio, TX.

### This is early-stage software

Homebase is a very young application still under rigorous development. It regularly breaks. We frequently wipe out and rewrite chunks of the code. If you want to tinker with the application, please keep in mind it is not ready for prime time.

(But if you'd like to poke around, fix some things and make a pull request, take a look at Pull Requests Welcome section below.)

# Install and deploy

Homebase has been tested and deployed locally and on the Heroku platform. We'll cover local installation first, then Heroku deployment.

### Pre-requisites

You'll need the following set up to run Homebase:

* [Ruby](https://github.com/codeforamerica/howto/blob/master/Ruby.md)
* [Rails](https://github.com/codeforamerica/howto/blob/master/Rails.md)
* [PostgreSQL](https://github.com/codeforamerica/howto/blob/master/PostgreSQL.md)

### Local Installation

All of these commands should be entered into your command line tool of choice.

#### 1. Clone the app to your computer and open the folder

    $ git clone https://github.com/codeforamerica/homebase.git
    $ cd homebase

#### 2. Install the dependencies

    $ bundle install

#### 3. Create and migrate your database

Homebase has a simple database that stores information users enter into the forms. You'll just need to create the databases on your local PostgreSQL installation.

    $ bundle exec rake db:create
    $ bundle exec rake db:migrate

Homebase checks if a user's address is in San Antonio (that's the only city we're supporting now). We have a pre-setup database (cosa_boundaries) that contains the geometry to allow us to check addresses. You'll need to import (or another boundary database you substitute) to get the app working.

    $ bundle exec rake cosa_boundaries:load

#### 4. Start your web service

Homebase uses the [Foreman](http://theforeman.org/) web server. It's easy to start up.

    $ bundle exec foreman start

#### You're good to go locally.

Check out your Homebase instance at [0.0.0.0:5000](http://0.0.0.0:5000) and have some fun.

### Deploy to Heroku

Things get a little tricky here. You'll need a [Heroku](https://heroku.com) to go forward. You'll also need to install the [Heroku Toolbelt](https://toolbelt.heroku.com/) so you can easily interact with Heroku from the command line.

#### 1. Create a new Heroku app, or connect to an existing one

Make sure you're in the folder for your app before you get started. Now, let's make our app.

    $ heroku create

Alternatively, you can connect to an existing app:

    $ heroku git:remote -a project-name-here

#### 2. Add the heroku-postgresql addon

You'll also need to upgrade to the Standard Yanari package (this costs $50/month).

    $ heroku addons:add heroku-postgresql:standard-yanari

#### 3. Configure your buildpack

    $ heroku config:set BUILDPACK_URL=https://github.com/ddollar/heroku-buildpack-multi.git

#### 4. Change your database config

You'll need to set up your Heroku PostgreSQL addon to use PostGIS. First, check out your config.

    $ heroku config

You'll see something like this:

    === project-name-here Config Vars
    DATABASE_URL:               postgres://xxxxxxxxxxxxxxxxxxx@xxx-xxx-xxxx-xxxxxx-xxxxxxxxx.xxxxxxxxxx-x.amazonaws.com:XXXX/xxxxxxxxx
    HEROKU_POSTGRESQL_COLOR_URL: postgres://xxxxxxxxxxxxxxxxxxx@xxx-xxx-xxxx-xxxxxx-xxxxxxxxx.xxxxxxxxxx-x.amazonaws.com:XXXX/xxxxxxxxx
    LANG:                       en_US.UTF-8
    RACK_ENV:                   production
    RAILS_ENV:                  production

We'll need to change those `postgres://` referers to `postgis://`.

    $ heroku config:set DATABASE_URL=postgis://xxxxxxxxxxxxxxxxxxx@xxx-xxx-xxxx-xxxxxx-xxxxxxxxx.xxxxxxxxxx-x.amazonaws.com:XXXX/xxxxxxxxx
    $ heroku config:set HEROKU_POSTGRESQL_COLOR_URL=postgis://xxxxxxxxxxxxxxxxxxx@xxx-xxx-xxxx-xxxxxx-xxxxxxxxx.xxxxxxxxxx-x.amazonaws.com:XXXX/xxxxxxxxx

Make sure that's the URL for your actual datbase from the heroku config info. Make sure you replace `COLOR` with the color for your database.

#### 5. Push your repo up

    $ git push heroku:master

#### 6. Now let's migrate the database

    $ heroku run rake db:migrate

#### 7. Load your data

    $ heroku run cosa_boundaries:load

#### 8. Your app is good to go

Check it out at project-name-here.herokuapp.com.

# Contribute



# Say hello

This application is being developed by the Code for America San Antonio fellowship team. We call ourselves the Techzans. (Get it?)

* Amy Mok  [twitter](https://twitter.com/amymok) | [website](http://mokamy.com) | [github](https://github.com/amymok)
* Maya Benari  [twitter](https://twitter.com/mayabenari) | [website](http://maya-benari.com/) | [github](https://github.com/maya-)
* David Leonard  [twitter](https://twitter.com/davidleonardii) | [website](http://davidleonard.me) | [github](https://github.com/davidrleonard)

To send us your thoughts, [submit an issue](https://github.com/codeforamerica/homebase/issues) or email us all at [sanantonio@codeforamerica.org](mailto:sanantonio@codeforamerica.org).

# Copyright

Homebase is Copyright (c) 2014 by Code for America. But you can copy or fork and use it to your delight, as long as you read and follow our [license](https://github.com/codeforamerica/homebase/blob/master/LICENSE).
