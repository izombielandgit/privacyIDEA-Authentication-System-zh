## RADIUS plugin configuration

The FreeRADIUS plugin is a perl module, that e.g. requires on a Debian system the following packages to be installed:

FreeRADIUS插件是一个perl模块，需要在Debian系统上安装以下软件包：

* libconfig-inifiles-perl
* libdata-dump-perl
* libtry-tiny-perl
* libjson-perl

The RADIUS plugin configuration is read from the file /opt/privacyIDEA/rlm_perl.ini.

RADIUS插件配置从文件/opt/privacyIDEA/rlm_perl.ini中读取。

Starting with version 2.7 the plugin first tries to read from the following locations:

从2.7版本开始，插件首先尝试从以下位置读取：

* /etc/privacyidea/rlm_perl.ini
* /etc/freeradius/rlm_perl.ini
* /opt/privacyIDEA/rlm_perl.ini.

If no file exists, the default values are:

如果没有这些文件，默认值为：

```
[Default]
URL = https://localhost/validate/check
REALM =
```

But it can also look like this:

但它也可以像这样：

```
[Default]
URL = https://your.server/validate/check
REALM = someRealm
RESCONF = someResolver
SSL_CHECK = true
DEBUG = true
```

> Note:
> 
> The default behaviour is to not check the SSL certificate. So in a productive environment where the privacyIDEA system is located on another server than the RADIUS server, you should set “SSL_CHECK = true”.
> 
> 注：
> 
> 默认行为是不检查SSL证书。因此，在privacyIDEA系统位于RADIUS服务器以外的其他服务器上的生产环境中，应设置“SSL_CHECK = true”。

### Radius and Realms

FreeRADIUS also has a notion of realms. In general the RADIUS realms are not necessarily the same as the privacyIDEA realms, but they can be mapped.


FreeRADIUS也有一个域的概念。通常，RADIUS域不一定与privacyIDEA域相同，但它们可以被映射。

A user can authenticate to the FreeRADIUS either with a simple username “fred”, or a username combined with a RADIUS realm in the format like “fred@realm1” or “realm1fred”.

用户可以使用简单的用户名“fred”或者以诸如“fred@realm1”或“realm1fred”的格式与RADIUS域组合的用户名来认证到FreeRADIUS。

> Note:
> 
> The format of the realms is defined in /etc/freeradius/modules/realm as “suffix” and “ntdomain”. I.e. you could also change the delimiter. The “suffix” and “ntdomain” is referenced in the authorize section in /etc/freeradius/sites-enabled/privacyidea.
> 
> 注：
> 
> 域的格式在/etc/freeradius/modules/realm中定义为“suffix”和“ntdomain”。也就是说，您还可以更改分隔符。“suffix”和“ntdomain”在/etc/freeradius/sites-enabled/privacyidea的authorize部分中引用。

The RADIUS server tries to split the realms according to the definition of “suffix” or “ntdomain”. I.e. a User-Name “fred@realmRadius” would be split into Stripped-User-Name “fred” and Realm (RADIUS realm) “realmRadius”. **But only if** FreeRADIUS can identify “realmRadius” as a RADIUS realm. For FreeRADIUS to identify this as a REALM you need to add this to the file /etc/freeradius/proxy.conf:

RADIUS服务器尝试根据“suffix”或“ntdomain”的定义拆分域。也就是说，User-Name “fred@realmRadius”将被拆分为Stripped-User-Name “fred”和Realm（RADIUS域）“realmRadius”。**但只有**FreeRADIUS可以识别“realmRadius”作为一个RADIUS域时。为了使FreeRADIUS能够将其识别为域，您需要将其添加到文件/etc/freeradius/proxy.conf中：

```
realm realmRadius {
}
```

### Realm processing in FreeRADIUS

A User-Name “fred@realmRadius” or “realmRadiusfred” is sent to the FreeRADIUS server.

将User-Name “fred@realmRadius”或“realmRadiusfred”发送到FreeRADIUS服务器。

If “realmRadius” can not be identified as RADIUS realm (missing entry in proxy.conf), then no realm can be split and the complete User-Name will be sent to privacyIDEA for validation. This can work out with “fred@realmRadius”, since privacyIDEA might split the @-sign. But this probably will not work out for “realmRadiusfred”.

如果“realmRadius”不能被识别为RADIUS域（在proxy.conf中缺少条目），则不能划分域，完整的User-Name将被发送到privacyIDEA进行认证。这样“fred@realmRadius”可正常工作，因为privacyIDEA可以拆分@-sign。但是可能“realmRadiusfred”不能正常工作。

If the “realmRadius” can be identified as RADIUS realm (entry in proxy.conf), then FreeRADIUS will split the User-Name into the RADIUS attributes Stripped-User-Name and Realm and the “fred” will be sent as user and “realmRadius” as the realm to privacyIDEA.

如果“realmRadius”可以被识别为RADIUS域（条目在proxy.conf中），则FreeRADIUS将把User-Name分割成RADIUS属性Stripped-User-Name和Realm，“fred”将作为用户名发送，“realmRadius”作为privacyIDEA的域。

This way you can directly map RADIUS realms in the RADIUS user name to realm in privacyIDEA.

这样，您可以直接将RADIUS域在RADIUS用户名映射到privacyIDEA中的域。

> Note:
> 
> If the User-Name could be split into the RADIUS attributes Stripped-User-Name and Realm, then these values are sent to the privacyIDEA server. If the User-Name could not be split (and Stripped-User-Name is empty) then User-Name is sent to the privacyIDEA server.
> 
> For a deeper insight take a look at the code
> <https://github.com/privacyidea/FreeRADIUS/blob/master/privacyidea_radius.pm#L276>
> 
> Note:
> 
> The NAS-IP-Address is sent as the client parameter to the privacyIDEA server. Using [Override Authorization Client](../4. Configuration 配置/4.3. System Config 系统配置.html#4316-override-authorization-client) you can pass the RADIUS client IP to the privacyIDEA server to perform policies based on the RADIUS client’s IP address.
> 
> Note:
> 
> You can define a realm in /opt/privacyIDEA/rlm_perl.ini. Such a realm definition will override a RADIUS realm in the User-Name.
> 
> 注：
> 
> 如果User-Name可以拆分为RADIUS属性Stripped-User-Name和Realm，则这些值被发送到privacyIDEA服务器。如果User-Name不能被拆分（并且Stripped-User-Name为空），则User-Name被发送到privacyIDEA服务器。
> 
> 有关更深入的了解，请查看代码
> <https://github.com/privacyidea/FreeRADIUS/blob/master/privacyidea_radius.pm#L276>
> 
> 注：
> 
> NAS-IP-Address作为客户端参数发送到privacyIDEA服务器。使用[Override Authorization Client](../4. Configuration 配置/4.3. System Config 系统配置.html#4316-override-authorization-client)，您可以将RADIUS客户端IP传递给privacyIDEA服务器，以根据RADIUS客户端的IP地址执行策略。
> 
> 注：
> 
> 您可以在/opt/privacyIDEA/rlm_perl.ini中定义域。这样的域定义将覆盖User-Name中的RADIUS域。

### Using the Token serial number

In case of a successful authentication privacyIDEA returns the serial number of the token used.

在成功认证的情况下，privacyIDEA返回使用的令牌的序列号。

If available (see [no_detail_on_success](../8. Policies 策略/8.4. Authorization policies 授权策略.html#844-nodetailonsuccess) and [no_detail_on_fail](../8. Policies 策略/8.4. Authorization policies 授权策略.html#845-nodetailonfail)) the FreeRADIUS server can receive this serial number. The serial number is contained in the response value privacyIDEA-Serial.

如果可用（请参阅[no_detail_on_success](../8. Policies 策略/8.4. Authorization policies 授权策略.html#844-nodetailonsuccess)和[no_detail_on_fail](../8. Policies 策略/8.4. Authorization policies 授权策略.html#845-nodetailonfail)），FreeRADIUS服务器可以接收此序列号。序列号包含在响应值privacyIDEA-Serial中。

To see the privacyIDEA-Serial in the RADIUS response, you need to include the dictionary.netknights in your FreeRADIUS dictionary.

You can get it here <span id="id1">[[1]](#netknights-dict)</span>.

要在RADIUS响应中查看privacyIDEA-Serial，您需要在FreeRADIUS字典中包含dictionary.netknights。您可以在这里获取 [[1]](#netknights-dict)。

### Debugging RADIUS

If you need to DEBUG the FreeRADIUS go like this.

如果你需要调试FreeRADIUS，像这样操作。

Add “DEBUG = true” to /opt/privacyIDEA/rlm_perl.ini. Then stop the FreeRADIUS and run it in debug mode as user root:

将“DEBUG = true”添加到/opt/privacyIDEA/rlm_perl.ini。然后停止FreeRADIUS并在调试模式下以root用户身份运行它：

```
/etc/init.d/freeradius stop; freeradius -X
```

Now you can send requests to the RADIUS server like this:

现在您可以像这样向RADIUS服务器发送请求：

```
echo 'User-Name=realm3\\cornelius, Password=test' | radclient -s \
   127.0.0.1 auth test
```

Of course you need to replace the IP of your RADIUS server and the RADIUS secret “test” with your clients secret.

当然，您需要将您的RADIUS服务器的IP和RADIUS密钥“test”替换为您的客户端密钥。

Footnotes

> <span id="netknights-dict">[[1]](#id1)</span>:
> 
> <https://github.com/privacyidea/privacyidea/blob/master/authmodules/FreeRADIUS/dictionary.netknights>