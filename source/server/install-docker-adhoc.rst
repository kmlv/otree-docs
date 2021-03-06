.. _install-docker-adhoc:

Docker on your own computer
===========================

You can use Docker to run a pre-configured oTree server
on your Linux, Windows, or Mac computer.

Advantages:

-   Easier setup because everything is preconfigured

Disadvantages:

-   Each time you make a change,
    Docker needs to re-build and re-install everything, which can take a couple minutes.
-   Requires a lot of RAM

If interested, follow the below steps.

Upgrade oTree
~~~~~~~~~~~~~

Upgrade oTree, to get the latest bugfixes:

.. code-block:: bash

    $ pip3 install -U otree-core

Save to requirements_base.txt
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Run::

    otree --version

The version that is output will look something like ``1.X.X``.
Open the file in your project's root directory
called ``requirements_base.txt``, and modify the line with ``otree-core>=``
to that version. The file should look like this (substitute actual version for ``1.X.X``):
::

    otree-core>=1.X.X

    # Heroku requires Django to be explicitly in requirements file
    # in order to run collectstatic
    Django==1.8.8

Docker will read this file and install the
same version of each library.
If you are depending on any extra Python libraries (e.g. Numpy or Pandas),
they need to be added to your ``requirements_base.txt`` also.


Download Docker configuration files
-----------------------------------

On your local computer's command line, go to your project folder and run these commands to download
the 4 Docker files (should add them to the same folder as ``requirements.txt``).

Windows::

    iwr https://raw.githubusercontent.com/oTree-org/otree-docker/master/Dockerfile -OutFile Dockerfile
    iwr https://raw.githubusercontent.com/oTree-org/otree-docker/master/entrypoint.sh -OutFile entrypoint.sh
    iwr https://raw.githubusercontent.com/oTree-org/otree-docker/master/pg_ping.py -OutFile pg_ping.py
    iwr https://raw.githubusercontent.com/oTree-org/otree-docker/master/.dockerignore -OutFile .dockerignore
    iwr https://raw.githubusercontent.com/oTree-org/otree-docker/master/docker-compose-adhoc.yaml -OutFile docker-compose.yaml
    iwr https://raw.githubusercontent.com/oTree-org/otree-docker/master/.env -OutFile .env

Mac/Linux::

    curl https://raw.githubusercontent.com/oTree-org/otree-docker/master/Dockerfile > Dockerfile
    curl https://raw.githubusercontent.com/oTree-org/otree-docker/master/entrypoint.sh > entrypoint.sh
    curl https://raw.githubusercontent.com/oTree-org/otree-docker/master/pg_ping.py > pg_ping.py
    curl https://raw.githubusercontent.com/oTree-org/otree-docker/master/.dockerignore > .dockerignore
    curl https://raw.githubusercontent.com/oTree-org/otree-docker/master/docker-compose-adhoc.yaml > docker-compose.yaml
    curl https://raw.githubusercontent.com/oTree-org/otree-docker/master/.env > .env

Install Docker
--------------

Windows
~~~~~~~

-   Download & run the `installer <https://download.docker.com/win/stable/InstallDocker.msi>`__.
-   You may need to restart your computer
-   Start Docker
-   If you get a "not enough memory" error, open Docker settings,
    go to the "Advanced" tab, and reduce memory usage to 1024 MB.
-   To further reduce RAM usage, close programs that use a lot of RAM like PyCharm,
    Firefox, Chrome.

Mac
~~~

-   Download & run the `installer <https://download.docker.com/mac/stable/Docker.dmg>`__.
-   Start Docker

Run the server
--------------

Run this command, which will install all dependencies
(Python, oTree, Postgres, Redis), and run the production server::

    docker-compose up --build --force-recreate

If you get an error like "'WaitNamedPipe', 'The system cannot find the file specified.'",
then it's probably because Docker is not running.

To stop the server, press Ctrl+C as usual.
If you need to restart the server but didn't make any changes,
enter::

    docker-compose up

If you modify your database models,
you will need to reset the database.
With Docker, instead of ``otree resetdb``, you should do::

    docker-compose down -v

Allow other computers to connect
--------------------------------

Instructions :ref:`here <server-adhoc>`.

Configure environment variables
-------------------------------

To set environment variables, edit the file ``.env``.
You should decide what ``OTREE_PORT`` to use.
You should use port 80 if you are a superuser,
and especially if your site needs to be accessed from the internet.
Otherwise, you can use a higher port number like 8000, 8001, etc.
