## RADIUS plugin configuration

The FreeRADIUS plugin is a perl module, that e.g. requires on a Debian system the following packages to be installed:

* libconfig-inifiles-perl
* libdata-dump-perl
* libtry-tiny-perl
* libjson-perl

The RADIUS plugin configuration is read from the file /opt/privacyIDEA/rlm_perl.ini.

Starting with version 2.7 the plugin first tries to read from the following locations:

* /etc/privacyidea/rlm_perl.ini
* /etc/freeradius/rlm_perl.ini
* /opt/privacyIDEA/rlm_perl.ini.

If no file exists, the default values are:

```
[Default]
URL = https://localhost/validate/check
REALM =
```

But it can also look like this:

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

### Radius and Realms

FreeRADIUS also has a notion of realms. In general the RADIUS realms are not necessarily the same as the privacyIDEA realms, but they can be mapped.

A user can authenticate to the FreeRADIUS either with a simple username “fred”, or a username combined with a RADIUS realm in the format like “fred@realm1” or “realm1fred”.

> Note:
> 
> The format of the realms is defined in /etc/freeradius/modules/realm as “suffix” and “ntdomain”. I.e. you could also change the delimiter. The “suffix” and “ntdomain” is referenced in the authorize section in /etc/freeradius/sites-enabled/privacyidea.

The RADIUS server tries to split the realms according to the definition of “suffix” or “ntdomain”. I.e. a User-Name “fred@realmRadius” would be split into Stripped-User-Name “fred” and Realm (RADIUS realm) “realmRadius”. But only if FreeRADIUS can identify “realmRadius” as a RADIUS realm. For FreeRADIUS to identify this as a REALM you need to add this to the file /etc/freeradius/proxy.conf:

```
realm realmRadius {
}
```

### Realm processing in FreeRADIUS

A User-Name “fred@realmRadius” or “realmRadiusfred” is sent to the FreeRADIUS server.

If “realmRadius” can not be identified as RADIUS realm (missing entry in proxy.conf), then no realm can be split and the complete User-Name will be sent to privacyIDEA for validation. This can work out with “fred@realmRadius”, since privacyIDEA might split the @-sign. But this probably will not work out for “realmRadiusfred”.

If the “realmRadius” can be identified as RADIUS realm (entry in proxy.conf), then FreeRADIUS will split the User-Name into the RADIUS attributes Stripped-User-Name and Realm and the “fred” will be sent as user and “realmRadius” as the realm to privacyIDEA.

This way you can directly map RADIUS realms in the RADIUS user name to realm in privacyIDEA.

> Note:
> 
> If the User-Name could be split into the RADIUS attributes Stripped-User-Name and Realm, then these values are sent to the privacyIDEA server. If the User-Name could not be split (and Stripped-User-Name is empty) then User-Name is sent to the privacyIDEA server.
> 
> For a deeper insight take a look at the code <https://github.com/privacyidea/FreeRADIUS/blob/master/privacyidea_radius.pm#L276>
> 
> Note:
> 
> The NAS-IP-Address is sent as the client parameter to the privacyIDEA server. Using Override Authorization Client you can pass the RADIUS client IP to the privacyIDEA server to perform policies based on the RADIUS client’s IP address.
> 
> Note:
> 
> You can define a realm in /opt/privacyIDEA/rlm_perl.ini. Such a realm definition will override a RADIUS realm in the User-Name.

### Using the Token serial number

In case of a successful authentication privacyIDEA returns the serial number of the token used.

If available (see no_detail_on_success and no_detail_on_fail) the FreeRADIUS server can receive this serial number. The serial number is contained in the response value privacyIDEA-Serial.

To see the privacyIDEA-Serial in the RADIUS response, you need to include the dictionary.netknights in your FreeRADIUS dictionary.

You can get it here <span id="id1">[[1]](#netknights-dict)</span>.

### Debugging RADIUS

If you need to DEBUG the FreeRADIUS go like this.

Add “DEBUG = true” to /opt/privacyIDEA/rlm_perl.ini. Then stop the FreeRADIUS and run it in debug mode as user root:

```
/etc/init.d/freeradius stop; freeradius -X
```

Now you can send requests to the RADIUS server like this:

```
echo 'User-Name=realm3\\cornelius, Password=test' | radclient -s \
   127.0.0.1 auth test
```

Of course you need to replace the IP of your RADIUS server and the RADIUS secret “test” with your clients secret.

Footnotes

> <span id="netknights-dict">[[1]](#id1)</span>:
> 
> <https://github.com/privacyidea/privacyidea/blob/master/authmodules/FreeRADIUS/dictionary.netknights>