---
chapters: {"pages":{"de/adapterref/iobroker.admin/README.md":{"title":{"de":"no title"},"content":"de/adapterref/iobroker.admin/README.md"},"de/adapterref/iobroker.admin/admin/tab-adapters.md":{"title":{"de":"Der Reiter Adapter"},"content":"de/adapterref/iobroker.admin/admin/tab-adapters.md"},"de/adapterref/iobroker.admin/admin/tab-instances.md":{"title":{"de":"Der Reiter Instanzen"},"content":"de/adapterref/iobroker.admin/admin/tab-instances.md"},"de/adapterref/iobroker.admin/admin/tab-objects.md":{"title":{"de":"Der Reiter Objekte"},"content":"de/adapterref/iobroker.admin/admin/tab-objects.md"},"de/adapterref/iobroker.admin/admin/tab-states.md":{"title":{"de":"Der Reiter Zustände"},"content":"de/adapterref/iobroker.admin/admin/tab-states.md"},"de/adapterref/iobroker.admin/admin/tab-groups.md":{"title":{"de":"Der Reiter Gruppen"},"content":"de/adapterref/iobroker.admin/admin/tab-groups.md"},"de/adapterref/iobroker.admin/admin/tab-users.md":{"title":{"de":"Der Reiter Benutzer"},"content":"de/adapterref/iobroker.admin/admin/tab-users.md"},"de/adapterref/iobroker.admin/admin/tab-events.md":{"title":{"de":"Der Reiter Ereignisse"},"content":"de/adapterref/iobroker.admin/admin/tab-events.md"},"de/adapterref/iobroker.admin/admin/tab-hosts.md":{"title":{"de":"Der Reiter Hosts"},"content":"de/adapterref/iobroker.admin/admin/tab-hosts.md"},"de/adapterref/iobroker.admin/admin/tab-enums.md":{"title":{"de":"Der Reiter Aufzählungen"},"content":"de/adapterref/iobroker.admin/admin/tab-enums.md"},"de/adapterref/iobroker.admin/admin/tab-log.md":{"title":{"de":"Der Reiter Log"},"content":"de/adapterref/iobroker.admin/admin/tab-log.md"},"de/adapterref/iobroker.admin/admin/tab-system.md":{"title":{"de":"Die Systemeinstellungen"},"content":"de/adapterref/iobroker.admin/admin/tab-system.md"}}}
translatedFrom: de
translatedWarning: If you want to edit this document please delete "translatedFrom" field, elsewise this document will be translated automatically again
editLink: https://github.com/ioBroker/ioBroker.docs/edit/master/docs/en/adapterref/iobroker.admin/admin/tab-system.md
title: The system settings
hash: Dn5CtO5okGfvvxL112X2km8W/OB3HqarZ13QntOJiLU=
---
# The system settings
This sets basic parameters for ioBroker.

![Admin system settings](../../../../de/adapterref/iobroker.admin/admin/img/tab-system_Systemeinstellungen.jpg)

## Main settings
### System language
so you can choose between system languages: German, English, Russian

### Unit temperature
this value is used by some adapters. Possible is °C or °F.

### Currency
Currently, this does not use an adapter

### Date format
choose how the date should be displayed in admin and vis.

### Separator
Comma or dot for float values

### Default History Instance
This SQL / History / InfluxDB Adapter instance is used by default for flot and rickshaw (charts).

## Repositories or repositories
![](../../../../de/adapterref/iobroker.admin/admin/img/tab-system_Verwahrungsorte2.jpg)

ioBroker can get the adapter list from different sources. The following sources are listed during installation:

* **default** - http://download.iobroker.net/sources-dist.json - Generated daily at 01:00 on the server.

Access is very fast, but the version information can be up to 24 hours old.

* **online** - https://raw.githubusercontent.com/ioBroker/ioBroker.js-controller/master/conf/sources-dist.json - Repository

is generated by an online source. Access can take a long time, this is the most recent source

* **sources - conf / sources-dist.json** - Is also generated automatically and takes a long time but the links may be out of date (some adapters may be missing)

## Certificates
![](../../../../de/adapterref/iobroker.admin/admin/img/tab-system_2017-01-19-09_33_54-ioBroker.jpg)

Here is the central place for the certificates used for the SSL / HTTPS communication. The certificates are used by admin, web, simple-api, socketio. By default, standard certificates are installed. You can not verify anything with that. They are only for SSL communication. Because the certificates are open you should use your own (self-signed) certificates, buy the right certificates or switch to Let's Encrypt. The communication with default certificates is not secure and if someone has the goal to read the traffic, this could be done. Be sure to install your own certificates. For example under [linux](http://guides.intertech.de/ssl_certificate_self.html).

## Let's Encrypt
![](../../../../de/adapterref/iobroker.admin/admin/img/tab-system_2017-01-19-09_40_07-ioBroker.jpg)

Let's Encrypt is a free, automated and open source _certificate authority_ of the Independent Internet Security Research Group (ISRG).

For more information about Let's Encrypt, see [here](https://letsencrypt.org/).

Some installations use Dynamic DNS or similar. to reach your own domain via an address assigned from there. IoBroker supports the automatic request and renewal of certificates at Let's Encrypt Organization.

The option to use the free allowances from Let's Encrypt exists in almost every adapter that can launch a web server and support HTTPS.

If you enable the option to use certificates, but not the automatic update, the corresponding instance will try to work with stored certificates.

When automatic updates are enabled, the instance will try to request certificates from Let's Encrypt and update them automatically.

The certificates are requested the first time the corresponding address is called for the first time. That if you e.g. "sub.domain.com" configured as an address and then calls [https://sub.domain.com](https://sub.domain.com/) on the certificates are requested for the first time which may take a while before the answer comes.

Issuing the certificates is a complex procedure, but if you follow the following explanation it should be easy to get the free certificates.

**Method:**

1. A new account with the entered email address must be created (Setup in the system settings)
2. A random key is generated as a password for the account.
3. When the account has been created, the system opens a small website on port 80 to confirm the address.
4. Let's encrypt **always** the port **80** to check the address.
5. If port 80 is already being used by another service, point 4 will come to fruition - so assign another port to the other service!
6. When the small web server is started, the request for the certificates for the specified addresses in the system settings is sent to the Let's encrypt server.
7. The Let's Encrypt server sends back a challenge phrase in response to the request and after a while tries to read that challenge phrase at the address "http:// yourdomain: 80 / .well-known / acme-challenge /".
8. When the server gets this challenge phrase back from our site, the Let's Encrypt server sends the certificates. These are stored in the directory that is entered in the system settings.

This sounds complex, but all you have to do is activate a few checkboxes and enter the email address and the web address in the system settings.

The certificates received are valid for approximately 90 days. After these certificates have been issued for the first time, another task is started which automatically extends the validity.

This topic is quite complex and thousands of things can go wrong. If this does not work, it is recommended to use the cloud adapter for access on the road.

** Let's Encrypt works only with a node.js version> = 4.5 **

## Statistics
![](../../../../de/adapterref/iobroker.admin/admin/img/tab-system_2017-01-19-09_48_46-ioBroker.jpg)

ioBroker admin sends the following information to download.iobroker.net:

<pre> {&quot;uuid&quot;: &quot;56cf0d20-XXXX-YYYY-BBBB-66eec47ZZZZZ&quot;, &quot;language&quot;: &quot;de&quot;, &quot;hosts&quot;: [{&quot;version&quot;: &quot;0.15.1&quot;, &quot;platform&quot;: &quot;Javascript / Node. js &quot;,&quot; type &quot;:&quot; win32 &quot;}],&quot; adapters &quot;: {&quot; admin &quot;: {&quot; version &quot;:&quot; 1.0.2 &quot;,&quot; platform &quot;:&quot; Javascript / Node.js &quot;},&quot; hm-rpc &quot;: {&quot; version &quot;:&quot; 1.1.2 &quot;,&quot; platform &quot;:&quot; Javascript / Node.js &quot;}}} </pre>

This can be disabled by setting statistics to "** nothing **".

However, the developers ask for this information:

<pre> We worked hard to get this project going.
In return, we ask you to send us the usage statistics.
No private information will be sent to ioBroker.org.
Each time the adapter list is updated, the anonymous statistics are also sent.
Many Thanks! </pre>