# 3. First Steps|第一步

You installed privacyIDEA successfully according to [Installation](../2. Installation 安装/Introduction.html) and created an administrator using the command pi-manage admin as e.g. described in [Ubuntu Packages](../2. Installation 安装/2.2. Ubuntu Packages Ubuntu软件包.html).

您已根据[Installation](../2. Installation 安装/Introduction.html)成功安装privacyIDEA，并如在[Ubuntu Packages](../2. Installation 安装/2.2. Ubuntu Packages Ubuntu软件包.html)中描述一样，使用pi-manage admin命令创建了管理员。

These first steps will guide you through the tasks of logging in to the management web UI, attaching your first users and enrolling the first token.

这些第一步将引导您完成登录管理Web界面，新增第一个用户和注册第一个令牌的任务。

* [3.1 Login to the Web UI](3.1. Login to the Web UI 登录Web界面.html)|登录Web界面
* [3.2 Creating your first realm](3.2. Creating your first realm 创建第一个域.html)|创建第一个域
* [3.3 Enrolling your first token](3.3. Enrolling your first token 注册第一个令牌.html)|注册第一个令牌

After these first steps you will be able to start attaching applications to privacyIDEA in order to add two factor authentication to those applications. You can

* use a PAM module to authenticate with OTP at SSH or local login
* or the RADIUS plugin to configure your firewall or VPN to use OTP,
* or use an Apache2 plugin to do Basic Authentication with OTP.
* You can also setup different web applications to use OTP.

在这些第一步之后，您可以开始将应用程序附加到privacyIDEA，以便为这些应用程序添加双因素认证。您可以：

* 使用PAM模块让SSH或本地登录通过OTP进行认证
* RADIUS插件配置您的防火墙或VPN使用OTP
* 使用Apache2插件通过OTP进行基本认证
* 您还可以设置不同的Web应用程序以使用OTP

To attach applications read the chapter [Application Plugins](../13. Application Plugins 应用程序插件/Introduction.html).

You may also go on reading the chapter [Configuration](../4. Configuration 配置/Introduction.html) to get a deeper insight in the configuration possibilities.

要附加应用程序，请阅读[Application Plugins](../13. Application Plugins 应用程序插件/Introduction.html)章节。

您还可以阅读[Configuration](../4. Configuration 配置/Introduction.html)章节，以深入了解配置可能性。