==============
django-prepare
==============

Ansible role to prepare the launch of a Django app on a Plesk managed Ubuntu server:

- installs important packages
- imports timezone data to MySQL (optional)
- setup virtualenv (optional)

Packages
--------

- python-pip
- python-dev
- build-essential
- python-django (for django-admin bash autocomplete register only)
- python-mysqldb (for MySQL only)
- libmysqlclient-dev_ (or get error `mysql_config not found`_)
- libapache2-mod-wsgi_
- virtualenvwrapper
- libjpeg-dev_ (for Pillow, or get error decoder JPEG not available)
- python3-dev (for Python 3)
- upgrade pip

.. _libmysqlclient-dev: http://stackoverflow.com/questions/5178292/pip-install-mysql-python-fails-with-environmenterror-mysql-config-not-found
.. _mysql_config not found: http://stackoverflow.com/questions/7475223/mysql-config-not-found-when-installing-mysqldb-python-interface
.. _libapache2-mod-wsgi: https://www.digitalocean.com/community/tutorials/installing-mod_wsgi-on-ubuntu-12-04
.. _libjpeg-dev: http://stackoverflow.com/questions/8915296/python-image-library-fails-with-message-decoder-jpeg-not-available-pil

Timezone data (optional)
------------------------

If the tzinfo data is missing from mysql, a ``ValueError`` occurs
(see `official mysql docs`_, `relevant tzinfo blog`_, `how to check tzinfo`_, `how to set tzinfo`_).

.. _official mysql docs: https://dev.mysql.com/doc/refman/5.5/en/time-zone-support.html
.. _relevant tzinfo blog: http://www.pending.io/django-mysql-time-zones-and-how-to-fix-it/
.. _how to check tzinfo: http://stackoverflow.com/questions/2934258/how-do-i-get-the-current-time-zone-of-mysql
.. _how to set tzinfo: http://stackoverflow.com/questions/930900/how-to-set-time-zone-of-mysql

Setup virtualenv (optional)
---------------------------

Create a directory to keep all virtualenvs and prepare bash profile for the user.

How to use
----------

To use, add the role ``django-prepare``.

Variables
---------

- ``ansible_user_id``: The user id with access to mysql and mysql database.
- ``mysql_pass``: MySQL password (better obtain from vault or prompt in playbook). Omit to skip mysql import of data.
- ``virtualenv_dir``: Virtualenvs home directory. Set to ``false`` to disable virtualenv. Default: ``/var/virtualenvs``.
- ``plesk_server_group``: Plesk server group, default: ``psaserv``.

Playbook examples
-----------------

Plain::

    ---
    - hosts: servers
      roles:
        - django-prepare

Specify variables::

    ---
    - hosts: servers
      gather_facts: no
      roles:
        - role: django-prepare
          virtualenv_dir: ~/.virtualenvs
