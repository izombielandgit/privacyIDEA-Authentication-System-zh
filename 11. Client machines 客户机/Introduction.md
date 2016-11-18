# 11. Client machines|客户机

privacyIDEA lets you define Machine Resolvers to connect to existing machine stores. The idea is for users to be able to authenticate on those client machines. Not in all cases an online authentication request is possible, so that authentication items can be passed to those client machines.

privacyIDEA允许您定义机器解析器以连接到现有机器仓库。这个想法是为用户能够在这些客户机上进行认证。不是在所有情况下，在线认证请求都可用，因此认证项能传递这些客户机。

In addition you need to define, which application on the client machine the user should authenticate to. Different application require different authentication items.

此外，您需要定义，用户应该认证的客户机上的哪个应用程序。不同的应用程序需要不同的认证项。

Therefore privacyIDEA can define application types. At the moment privacyIDEA knows the application luks, offline and ssh. You can write your own application class, which is defined in Application Class.

因此，privacyIDEA可以定义应用程序类型。目前privacyIDEA知道应用程序luks，offline和ssh。您可以编写自己的应用程序类，它在Application Class中定义。

You need to assign an application and a token to a client machine. Each application type can work with certain token types and each application type can use additional parameters.

您需要向客户机分配应用程序和令牌。每个应用程序类型可以使用某些令牌类型，每个应用程序类型可以使用其他参数。

> Note:
> 
> Not all tokens work well with all applications!
> 
> 注：
> 
> 并不是所有的令牌都适用于所有的应用程序！

## 11.1. SSH

Currently working token types: SSH

目前工作的令牌类型：SSH

Parameters:

参数：

**user** (optional, default=root)(可选，默认root)

When the SSH token type is assigned to a client, the user specified in the user parameter can login with the private key of the SSH token.

当SSH令牌类型分配给客户端时，user参数中指定的用户可以使用SSH令牌的私钥登录。

In the sshd_config file you need to configure the AuthorizedKeysCommand. Set it to:

在sshd_config文件中，您需要配置AuthorizedKeysCommand。设置为：

```
privacyidea-authorizedkeys
```

This will fetch the SSH public keys for the requesting machine.

这将获取请求机器的SSH公钥。

The command expects a configuration file /etc/privacyidea/authorizedkeyscommand which looks like this:

该命令需要一个配置文件/etc/privacyidea/authorizedkeyscommand，如下所示：

```
[Default]
url=https://localhost
admin=admin
password=test
nosslcheck=False
```

> Note:
> 
> To disable a SSH key for all servers, you simple can disable the SSH token in privacyIDEA.
> 
> Warning:
> 
> In a productive environment you should not set nosslcheck to true, otherwise you are vulnerable to man in the middle attacks.
> 
> 注：
> 
> 要禁用所有服务器的SSH密钥，只需简单就可以禁用privacyIDEA中的SSH令牌。
> 
> 警告：
> 
> 在生产环境中，不应将nosslcheck设置为true，否则容易受到中间人攻击。

## 11.2. LUKS

Currently working token types: Yubikey Challenge Response

目前工作的令牌类型：Yubikey Challenge Response

Parameters:

参数：

**slot**

The slot to which the authentication information should be written

应写入认证信息的插槽

**partition**

The encrypted partition (usually /dev/sda3 or /dev/sda5)

加密分区（通常为/dev/sda3或/dev/sda5）

These authentication items need to be pulled on the client machine from the privacyIDEA server.

需要从privacyIDEA服务器上拉取客户端机器这些认证项。

Thus, the following script need to be executed with root rights (able to write to LUKS) on the client machine:

因此，需要在客户端机上使用root权限（能够写入LUKS）来执行以下脚本：

```
privacyidea-luks-assign @secrets.txt --clearslot --name salt-minion
```

For more information please see the man page of this tool.

有关更多信息，请参阅此工具的手册。

## 11.3. Offline

Currently working token types: HOTP.

目前工作的令牌类型：HOTP

Parameters:

参数：

**user**

The local user, who should authenticate. (Only needed when calling machine/get_auth_items)

应进行认证的本地用户。（仅调用machine/get_auth_items时需要）

**count**

The number of OTP values passed to the client.

传递给客户端的OTP值数量。

The offline application also triggers when the client calls a /validate/check. If the user authenticates successfully with the correct token (serial number) and this very token is attached to the machine with an offline application the response to validate/check is enriched with a “auth_items” tree containing the salted SHA512 hashes of the next OTP values.

offline应用程序还会在客户端调用/validate/check时触发。如果用户使用正确的令牌（序列号）成功地进行认证，并且这个令牌通过离线应用程序附加到机器，那么对validate/check的响应将通过包含下一OTP值的已盐化SHA512哈希值的“auth_items”树来丰富。

The client can cache these values to enable offline authentication. The caching is implemented in the privacyIDEA PAM module.

客户端可以缓存这些值以启用offline认证。缓存在privacyIDEA PAM模块中实现。

The server increases the counter to the last offline cached OTP value, so that it will not be possible to authenticate with those OTP values available offline on the client side.

服务器将计数器增加到最后一个离线缓存OTP值，因此将无法使用客户端上可离线使用的那些OTP值进行认证。