# 1. Overview|概述

privacyIDEA is a system that is used to manage devices for two factor authentication. Using privacyIDEA you can enhance your existing applications like local login, VPN, remote access, SSH connections, access to web sites or web portals with a second factor during authentication. Thus boosting the security of your existing applications.

privacyIDEA是用于管理双因素认证设备的系统。使用privacyIDEA，你可以在身份验证期间通过第二种因素增强现有的应用程序，如本地登录，VPN，远程访问，SSH连接，访问网站或Web门户等。从而提高现有应用程序的安全性。

In the beginning there were OTP tokens, but other means to authenticate like SSH keys are added. Other concepts like handling of machines or enrolling certificates are coming up, you may monitor this development on [Github](https://github.com/privacyidea/privacyidea).

开始使用OTP令牌，然后其他的验证手段也增加了，比如SSH密钥。其他概念，如处理机器或注册证书也将实现，你可以在GitHub上查看开发情况.

privacyIDEA is a web application written in Python based on the [flask micro framework](http://flask.pocoo.org/). You can use any webserver with a wsgi interface to run privacyIDEA. E.g. this can be Apache, Nginx or even [werkzeug](http://werkzeug.pocoo.org/).

privacyIDEA是一个用Python编写的基于flask micro framework的web应用程序。你可以使用任何有WSGI接口的Web服务器来运行privacyIDEA，比如Apache，Nginx甚至是werkzeug。

A device or item used to authenticate is still called a “token”. All token information is stored in an SQL database, while you may choose, which database you want to use. privacyIDEA uses [SQLAlchemy](http://www.sqlalchemy.org/) to map the database to internal objects. Thus you may choose to run privacyIDEA with SQLite, MySQL, PostgreSQL, Oracle, DB2 or other database.

用于认证的设备仍称为“令牌”。所有令牌信息存储在SQL数据库中，你可以选择要使用的数据库。privacyIDEA使用SQLAlchemy将数据库映射到内部对象。因此，你可以选择使用SQLite，MySQL，PostgreSQL，Oracle，DB2或其他数据库运行privacyIDEA。

The code is divided into three layers, the API, the library and the database layer. Read about it at Code Documentation. privacyIDEA provides a clean REST API.

代码分为三层，即API，库和数据库层。阅读代码文档。privacyIDEA提供了一个干净的REST API。

Administrators can use a Web UI or a command line client to manage authentication devices. Users can log in to the Web UI to manage their own tokens.

管理员可以使用Web界面或命令行来管理身份验证设备。用户可以登录到Web界面以管理自己的令牌。

Authentication is performed via the API or certain plugins for FreeRADIUS, simpleSAMLphp, Wordpress, Contao, Dokuwiki... to either provide default protocols like RADIUS or SAML or to integrate into applications directly.

通过API或某些用于FreeRADIUS，simpleSAMLphp，Wordpress，Contao，Dokuwiki等的插件执行身份验证。可以提供默认协议（如RADIUS或SAML）或直接集成到应用程序中。

Due to this flexibility there are also many different ways to install and setup privacyIDEA. We will take a look at common ways to setup privacyIDEA in the section Installation but there are still many others.

由于这种灵活性，还有许多不同的方法来安装和设置privacyIDEA。我们将在安装章节中看到安装privacyIDEA的常见方法，但仍有许多其他方法。