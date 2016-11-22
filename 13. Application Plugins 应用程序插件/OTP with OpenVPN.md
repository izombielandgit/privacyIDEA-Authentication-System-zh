## OTP with OpenVPN

This section describes, how you can setup OpenVPN to authenticate against privacyIDEA. There are basically three ways to integrate OpenVPN with privacyIDEA:

本节介绍如何设置OpenVPN来使用privacyIDEA进行认证。有三种基本的方法来整合OpenVPN与privacyIDEA：

1. use the privacyidea_pam.py module for PAM(使用PAM的privacyidea_pam.py模块)
2. integrate OpenVPN directly with RADIUS(将OpenVPN直接与RADIUS集成)
3. use the PAM module for RADIUS in OpenVPN(在OpenVPN中使用RADIUS的PAM模块)

Each of the methods has its andvantages as well as drawbacks.

每种方法都有其优点和缺点。

On the client side, you need to add:


在客户端，您需要添加：

```
auth-user-pass
```

to the client configuration.

到客户端配置。

Now the user is asked for a password when establishing the VPN connection. The entered password is sent to privacyIDEA. Thus you can require the user to enter a password consisting of a static part he knows and the OTP part which the user needs to generate with the OTP token he possesses.

现在，在建立VPN连接时，系统会要求用户输入密码。输入的密码发送到privacyIDEA。因此，您可以要求用户输入一个密码，该密码由他所知道的静态部分和用户需要用所拥有的OTP令牌生成的OTP部分组成。

Another addition you most probably want to make is adding the following option to both the client and the server configuration:

你有可能想要添加以下选项到客户端和服务器配置：

```
reneg-sec 0
```

By default, the channel key gets renegotiated after 3600 seconds, either partner can request a renegotiation. If only one partner disables this option, the other one will request it. This works fine for static password or dual-factor authentication where both factors are static (e.g. password and certificate/smartcard).

默认情况下，通道密钥在3600秒后重新协商，任一伙伴都可以请求重新协商。如果只有一方禁用此选项，另一方将请求。这对于静态密码或两个因素都是静态（如密码和证书/智能卡）的双因素认证工作正常。

When using OTP authentication, note that this default value may cause the end user to be challenged to reauthorize once per hour. The OpenVPN client with the option –auth-user-pass prompts for username and password for every renegotiation.

使用OTP认证时，请注意此默认值可能会导致最终用户每小时重新授权一次。具有–auth-user-pass选项的OpenVPN客户端每次重新协商时提示输入用户名和密码。

Network-Manager does not rechallenge the user and the VPN connection hangs, so you’ll need to disabled the renegotiation.

Network-Manager不会再挑战用户，VPN连接将挂起，因此您需要禁用重新协商。

If you are also requiring client certificates, the user needs

如果您还需要客户端证书，则用户需要

1. a machine certificate(机器证书)
2. a password(密码)
3. and an OTP token(OTP令牌)

to establish a VPN connection.

以建立VPN连接。

### privacyidea_pam.py module for OpenVPN

OpenVPN的privacyidea_pam.py模块

For this the PAM stack is used. To get the basic information about integrating privacyIDEA with PAM, read [Pluggable Authentication Module](../13. Application Plugins 应用程序插件/Introduction.html#131-pluggable-authentication-module). Since we do not use RADIUS this is the least complex configuration and for most installations probably the preferred one. The biggest drawback is that you need to install the privacyidea-pam package on your OpenVPN server. As long as the package is not part of your distribution you need to handle updates/security fixes manually or by using the packages provided by privacyIDEA.

为使用此PAM堆栈。要获取有关将privacyIDEA与PAM集成的基本信息，请参阅[Pluggable Authentication Module](../13. Application Plugins 应用程序插件/Introduction.html#131-pluggable-authentication-module)。由于我们不使用RADIUS这是最不复杂的配置，因此大多数安装中可能是首选。最大的缺点是你需要在OpenVPN服务器上安装privacyidea-pam软件包。只要软件包不是您的发行版的一部分，您需要手动处理更新/安全修复程序，或使用privacyIDEA提供的软件包。

You can create a file /etc/pam.d/openvpn on your OpenVPN server that basically looks like this:

您可以在您的OpenVPN服务器上创建一个文件/etc/pam.d/openvpn，基本上如下所示：

```
auth    [success=1 default=ignore]      pam_python.so
    /path/to/privacyidea_pam.py url=https://your.privacyidea.server
auth    requisite           pam_deny.so
auth    required            pam_permit.so
session sufficient          pam_permit.so
account sufficient          pam_permit.so
```

Then you need to configure the OpenVPN server like this:

然后你需要配置OpenVPN服务器像这样：

```
port 1194
[...]
plugin /usr/lib/openvpn/openvpn-auth-pam.so openvpn
```

The important line is the last line, which tells OpenVPN to use the PAM stack to authenticate the user and within the PAM stack the configuration for “openvpn”. On certain distributions the library might be located at /usr/lib64/openvpn/plugin/lib/openvpn-auth-pam.so.

重要的是最后一行，它告诉OpenVPN使用PAM堆栈来认证用户，并在PAM堆栈中验证“openvpn”的配置。在某些发行版上，库可能位于/usr/lib64/openvpn/plugin/lib/openvpn-auth-pam.so。

### Integration of OpenVPN directly with RADIUS

OpenVPN直接与RADIUS集成

This configuration does not use PAM, so might be preferred in some installations. You will need the package openvpn-auth-radius which should be part of your distribution. Before you can configure your OpenVPN you need to install freeradius on your privacyIDEA server and configure it according to [FreeRADIUS Plugin](../13. Application Plugins 应用程序插件/Introduction.html#133-freeradius-plugin). Be sure that RADIUS works before you start.

此配置不使用PAM，因此在某些安装中可能是首选。您将需要软件包openvpn-auth-radius，它可能是您的发行版的一部分。在配置您的OpenVPN之前，您需要在您的privacyIDEA服务器上安装freeradius并根据[FreeRADIUS Plugin](../13. Application Plugins 应用程序插件/Introduction.html#133-freeradius-plugin)进行配置。确保RADIUS在开始（配置OpenVPN）之前工作。

Copy the file /usr/share/doc/openvpn-auth-radius/examples/radiusplugn.cnf into /etc/openvpn and adapt it to your configuration. The most important parts of the file should contain:

将文件/usr/share/doc/openvpn-auth-radius/examples/radiusplugn.cnf复制到/etc/openvpn中，并将其适配到您的配置。文件的最重要部分应包含：

```
# The NAS identifier which is sent to the RADIUS server
NAS-Identifier=OpenVPN
OpenVPNConfig=/etc/openvpn/server.conf
[...]
server
{
      # The UDP port for radius accounting.
      acctport=1813
      # The UDP port for radius authentication.
      authport=1812
      # The name or ip address of the radius server.
      name=<your-radius-server>
      # How many times should the plugin send the if there is no response?
      retry=1
      # How long should the plugin wait for a response?
      wait=1
      # The shared secret.
      sharedsecret=<shared-secret>
}
```

After the changes restart your OpenVPN service and keep a look at the logs of OpenVPN on your access server as well as the freeradius logs on your RADIUS server.

更改后重新启动您的OpenVPN服务，并查看您的访问服务器的OpenVPN日志以及您的RADIUS服务器上的freeradius日志。

If you use privacyidea-radius 2.6 or earlier, you make sure you have the following entry in /etc/freeradius/sites-enabled/privacyidea:

如果使用privacyidea-radius 2.6或更早版本，请确保/etc/freeradius/sites-enabled/privacyidea中有以下条目：

```
[...]
accounting {
      detail
}
[...]
```

Otherwise RADIUS will authenticate your user, but refuse to add the accounting data that the OpenVPN plugin sends and the connect will fail.

否则RADIUS会认证您的用户，但拒绝添加OpenVPN插件发送的计费数据，并且连接将失败。

### Using the PAM module for RADIUS in OpenVPN

在OpenVPN中使用RADIUS的PAM模块

The other method to integrate OpenVPN with RADIUS (and privacyIDEA) is to use the PAM module libpam-radius-auth. If you have other services running on your OpenVPN server that should integrate into privacyIDEA as well, this might be your preferred method.

将OpenVPN与RADIUS（和privacyIDEA）集成的另一种方法是使用PAM模块libpam-radius-auth。如果您的OpenVPN服务器上运行的其他服务也需要集成到privacyIDEA中，这可以是您首选的方法。

You can create a file /etc/pam.d/openvpn on your OpenVPN server that basically looks like this:


您可以在您的OpenVPN服务器上创建一个文件/etc/pam.d/openvpn，基本上如下所示：

```
auth    [success=1 default=ignore]      pam_radius_auth.so
auth    requisite           pam_deny.so
auth    required            pam_permit.so
session sufficient          pam_permit.so
account sufficient          pam_permit.so
```

Then you need to configure the OpenVPN server like this:

然后你需要配置OpenVPN服务器这样：

```
port 1194
[...]
plugin /usr/lib/openvpn/openvpn-auth-pam.so openvpn
```

Now we need to tell the PAM plugin which RADIUS server to use. Modify the file /etc/pam_radius_auth.conf to point to your RADIUS server and add the shared secret:

现在我们需要告诉PAM插件使用哪个RADIUS服务器。将文件/etc/pam_radius_auth.conf修改为指向您的RADIUS服务器并添加共享密钥：

```
# server[:port] shared_secret      timeout (s)
#127.0.0.1      secret             1
#other-server    other-secret       3
<your-radius-server>:1812 <shared-secret> 3
```

Now you can restart your OpenVPN service and should be able to connect with your PIN and OTP. Again, have a look at the logs of both OpenVPN and RADIUS.

现在您可以重新启动OpenVPN服务，并且应该能够使用您的PIN和OTP连接。再次查看OpenVPN和RADIUS的日志。