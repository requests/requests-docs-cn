.. _install:

安装
============

这部分文档包含了Requests的安装过程，使用任何软件的第一步就是合适的安装好它.


Distribute & Pip
----------------

使用 `pip <http://www.pip-installer.org/>`_ 安装Requests非常简单 :: 

    $ pip install requests

或者使用 `easy_install <http://pypi.python.org/pypi/setuptools>`_ 安装 ::

    $ easy_install requests

但是你最好 `不要这样 <http://www.pip-installer.org/en/latest/other-tools.html#pip-compared-to-easy-install>`_.



Cheeseshop Mirror
-----------------

If the Cheeseshop is down, you can also install Requests from one of the
mirrors. `Crate.io <http://crate.io>`_ is one of them::

    $ pip install -i http://simple.crate.io/ requests


Get the Code
------------

Requests is actively developed on GitHub, where the code is
`always available <https://github.com/kennethreitz/requests>`_.

You can either clone the public repository::

    git clone git://github.com/kennethreitz/requests.git

Download the `tarball <https://github.com/kennethreitz/requests/tarball/master>`_::

    $ curl -OL https://github.com/kennethreitz/requests/tarball/master

Or, download the `zipball <https://github.com/kennethreitz/requests/zipball/master>`_::

    $ curl -OL https://github.com/kennethreitz/requests/zipball/master


Once you have a copy of the source, you can embed it in your Python package,
or install it into your site-packages easily::

    $ python setup.py install
