deepdishatx :: view and search austin's restaurant inspections, on a map
-------------

This is a test application to kick the tires of [cartodb](http://cartodb.com/). It was originally made with 
[Health & Human Svs Data](https://data.cityofchicago.org/Health-Human-Services/Food-Inspections/4ijn-s7e5) from
Chicago, zachwill's [Flask_Heroku](https://github.com/zachwill/flask_heroku), and the souls of django ponies by [Jesse Bounds](https://github.com/boundsj).  This app uses [similar data](https://data.austintexas.gov/dataset/Restaurant-Inspection-Scores/ecmv-9xxi) from Austin.

Instructions
------------

First, you'll need to clone the repo.

    $ git clone git@github.com:codeforamerica/deepdish.git 
    $ cd deepdish 

A forked repo of cartodb-leaflet is a submodule of deepdish. It's forked because (as of 4/9/2012) the cartodb repo does not handle touchevents so a solution for that is in the fork of cartodb-leaflet that this project uses. So, to set it up, run:

    $ git submodule init
    $ git submodule update

Second, let's download `pip`, `virtualenv`, `foreman`, and the [`heroku`
Ruby gem](http://devcenter.heroku.com/articles/using-the-cli).

    $ sudo easy_install pip
    $ sudo pip install virtualenv
    $ sudo gem install foreman heroku

Now, you can setup an isolated environment with `virtualenv`.

    $ virtualenv --no-site-packages env
    $ source env/bin/activate


Installing Packages
--------------------

### Gevent

To use `gevent`, we'll need to install `libevent` for the
`gevent` production server. If you're operating on a Linux OS, you can
`apt-get install libevent-dev`. If you're using Mac OS X, consider
installing the [homebrew](http://mxcl.github.com/homebrew/) package
manager, and run the following command:

    $ brew install libevent

If you're using Mac OS X, you can also install `libevent` through [a DMG
available on Rudix](http://rudix.org/packages-jkl.html#libevent).


### Without Gevent

If you'd rather use `gunicorn` without `gevent`, you just need to edit
the `Procfile` and `requirements.txt`.

First, edit the `Procfile` to look the following:

    web: gunicorn -w 4 -b "0.0.0.0:$PORT" app:app

Second, remove `gevent` from the `requirements.txt` file.

### pip

Then, let's get the requirements installed in your isolated test
environment.

    $ pip install -r requirements.txt


Running Your Application
------------------------

Now, you can run the application locally.

    $ foreman start

You can also specify what port you'd prefer to use.

    $ foreman start -p 5555


Deploying
---------

If you haven't [signed up for Heroku](https://api.heroku.com/signup), go
ahead and do that. You should then be able to [add your SSH key to
Heroku](http://devcenter.heroku.com/articles/quickstart), and also
`heroku login` from the commandline.

Now, to upload your application, you'll first need to do the
following -- and obviously change `app_name` to the name of your
application:

    $ heroku create app_name -s cedar

And, then you can push your application up to Heroku.

    $ git push heroku master
    $ heroku scale web=1

Finally, we can make sure the application is up and running.

    $ heroku ps

Now, we can view the application in our web browser.

    $ heroku open

And, to deactivate `virtualenv` (once you've finished coding), you
simply run the following command:

    $ deactivate

Reactivating the Virtual Environment
------------------------------------

If you haven't worked with `virtualenv` before, you'll need to
reactivate the environment everytime you close or reload your terminal.

    $ source env/bin/activate

If you don't reactivate the environment, then you'll probably receive a
screen full of errors when trying to run the application locally.
