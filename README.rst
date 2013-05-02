.. contents::

Introduction
============

This buildout installs crate.io webservice with all dependencies and components needed.
It's based on the crate.io project. crate.io and crate.pypi are forked due to some enhancments
and compatibility reasons.
It also installs supervisor to monitor the running services.

Configuration
=============

You are free to change the domain an port settings in buildout.cfg before running buildout

Buildout
========

Buildout must not be run as root, as postgresql won't start with root privileges::
    
    mkdir downloads
    python bootstrap.py
    ./bin/buildout -v


Django Dance
============

After running buildout execute following command, to create DB::

    ./bin/manage.py syncdb
    ./bin/manage.py migrate
    ./bin/manage.py runserver

Let's go Pypi with Vegetables
==============================

Now we want to start downloading and start our celery workers::
    
    ./bin/manage.py celeryd -c 4 -l info
    ./bin/manage.py trigger_bulk_sync

This starts 4 celery worker, with additional information, and creates an Queue Entry in Redis

All about Monitoring
====================

With Supervisord (settings in buildout.cfg) you can watch/restart/stop your services, by a simple webinterface::

    ./bin/supervisord

After that you can acces SuperVisor Webinterface, or use **supervisorctl**

ElasticSearch
=============

to find our entries/packages by crate.io webinterface we have to index the packes by running::

    ./bin/manage.py rebuild_index

This rebuilds an existing index, or creates a new one, if it doesn't exist.
