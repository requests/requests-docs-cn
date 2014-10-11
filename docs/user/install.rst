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





获得源码
------------

Requests 一直在Github上被积极的开发着，你可以在此获得代码，且总是
`可用的 <https://github.com/kennethreitz/requests>`_.

你也可以克隆公共版本库::

    git clone git://github.com/kennethreitz/requests.git

下载 `源码 <https://github.com/kennethreitz/requests/tarball/master>`_::

    $ curl -OL https://github.com/kennethreitz/requests/tarball/master

或者下载 `zipball <https://github.com/kennethreitz/requests/zipball/master>`_::

    $ curl -OL https://github.com/kennethreitz/requests/zipball/master


一旦你获得了复本，你就可以轻松的将它嵌入到你的python包里或者安装到你的site-packages::

    $ python setup.py install
