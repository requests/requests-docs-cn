.. _install:

安装 Requests
=============

这部分文档包含了 Requests 的安装过程。
使用任何软件的第一步是正确地安装它。


$ python -m pip install requests
--------------------------------

要安装 Requests，只要在你的终端中运行这个简单命令即可::

$ python -m pip install requests

获得源码
-------

Requests 一直在 Github 上积极地开发，你可以一直从\
`这里 <https://github.com/psf/requests>`_\获取到代码。


你可以克隆公共版本库::

$ git clone git://github.com/psf/requests.git

也可以下载 `tarball <https://github.com/requests/requests/tarball/master>`_::

    $ curl -OL https://github.com/psf/requests/tarball/master
    # Windows 用户也可选择 zip 包

获得代码之后，你就可以轻松的将它嵌入到你的 python 包里，或者安装到你的 site-packages::

    $ cd requests
    $ python -m pip install .
