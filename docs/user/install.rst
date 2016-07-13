.. _install:

安装
============

这部分文档包含了 Requests 的安装过程，使用任何软件的第一步就是正确地安装它。


Pip Install Requests
---------------------

要安装 Requests，只要在你的终端中运行这个简单命令即可：

::
    $ pip install requests

如果你没有安装 pip （啧啧），这个 `Python installation guide <http://docs.python-guide.org/en/latest/starting/installation/>`_
可以带你完成这一流程。


获得源码
------------

Requests 一直在 Github 上积极地开发，你可以一直从
`这里 <https://github.com/kennethreitz/requests>`_ 获取到代码。

你可以克隆公共版本库：

::

    git clone git://github.com/kennethreitz/requests.git

也可以下载 `tarball <https://github.com/kennethreitz/requests/tarball/master>`_::

    $ curl -OL https://github.com/kennethreitz/requests/tarball/master
      # Windows 用户也可选择 zip 包

获得代码之后，你就可以轻松的将它嵌入到你的 python 包里，或者安装到你的 site-packages::

    $ python setup.py install
