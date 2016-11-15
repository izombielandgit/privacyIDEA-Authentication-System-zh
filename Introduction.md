# Welcome to privacyIDEA

privacyIDEA is a modular authentication system. Using privacyIDEA you can enhance your existing applications like [local login](13. Application Plugins 应用程序插件/#131-pluggable-authentication-module), [VPN](13. Application Plugins 应用程序插件/#133-freeradius-plugin), [remote access]((13. Application Plugins 应用程序插件/#133-freeradius-plugin), [SSH connections](13. Application Plugins 应用程序插件/#131-pluggable-authentication-module), access to web sites or [web portals](14. Code Documentation 代码文档/14.1.1. REST API.html) with a second factor during authentication. Thus boosting the security of your existing applications. Originally it was used for OTP authentication devices. But other “devices” like challenge response  and SSH keys are also available. It runs on Linux and is completely Open Source, licensed under the AGPLv3.

privacyIDEA是一种模块化认证系统。使用privacyIDEA，你可以在认证期间通过第二种因素增强现有的应用程序，如[local login](13. Application Plugins 应用程序插件/#131-pluggable-authentication-module)，[VPN](13. Application Plugins 应用程序插件/#133-freeradius-plugin)，[remote access]((13. Application Plugins 应用程序插件/#133-freeradius-plugin)，[SSH connections](13. Application Plugins 应用程序插件/#131-pluggable-authentication-module)，访问网站或[web portals](14. Code Documentation 代码文档/14.1.1. REST API.html)等。从而提高现有应用程序的安全性。最初它被用于OTP认证设备。但其他“设备”，如挑战响应和SSH密钥也可用。它运行在Linux，完全开放源代码，AGPLv3许可。

privacyIDEA can read users from many different sources like flat files, different LDAP services, SQL databases and SCIM services. (see [Realms](4. Configuration 配置/4.2. Realms 域.html))

privacyIDEA可以从许多不同的来源（如平面文件，LDAP服务器，SQL数据库和SCIM服务）读取用户。（参阅[Realms](4. Configuration 配置/4.2. Realms 域.html)）

Authentication devices to provide two factor authentication can be assigned to those users, either by administrators or by the users themselves. [Policies](8. Policies 策略/) define what a user is allowed to do in the web UI and what an administrator is allowed to do in the management interface.

提供双因素认证的认证设备可以由管理员分配给用户或由用户自己分配。[Policies](8. Policies 策略/)定义了允许用户在Web界面中执行的操作以及允许管理员在管理界面中执行的操作。

The system is written in python, uses flask as web framework and an SQL database as datastore. Thus it can be enrolled quite easily providing a lean installation. (see [Installation](2. Installation 安装/))

该系统是用python编写的，使用flask作为web框架和一个SQL数据库作为数据存储。因此，可以很容易地注册提供精益安装。（参阅[Installation](2. Installation 安装/)）

> Note:
> 
> Some parts are marked as “**(TODO)** Not yet implemented”. These are components that have not been migrated from 1.5 to 2.0. If you are missing an important, not-yet-migrated part, drop us a note!
> 
> 注：
> 
> 某些部分标记为“**（待做）**尚未实现”。这些是尚未从1.5迁移到2.0的组件。如果您缺少一个重要的，尚未迁移的部分，请给我们留言！

If you are missing any information or descriptions file an issue at [github](https://github.com/privacyidea/privacyidea/issues) (which would be the preferred way), drop a note to info(@)privacyidea.org or go to the [Google group](https://groups.google.com/forum/?hl=en#!forum/privacyidea).

This will help us a lot to improve documentation to your needs.

Thanks a lot!

有什么问题可以发邮件或是到[github](https://github.com/privacyidea/privacyidea/issues)，[Google group](https://groups.google.com/forum/?hl=en#!forum/privacyidea)提交issue。