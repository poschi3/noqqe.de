---
date: 2011-03-04T19:53:19+02:00
layout: post
slug: apache-authentifizierung-uber-mod_auth_pam
status: publish
comments: true
title: Apache | Authentifizierung über mod_auth_pam
alias: /archives/1494
categories:
- Bash
- Coding
- Debian
- Linux
- PlanetenBlogger
- Ubuntu
- Web
tags:
- apache2
- auth
- authentication failure
- AuthPam_Enabled
- auth_pam
- ftp
- pam
- unix_chkpwd
- vsftpd
---

FTP-User, die Dateien auch über HTTP durchsuchen wollen, werden bei vsftpd häufig über /etc/passwd als SystemUser authentifiziert. Um nicht noch eine zusätzliche htpasswd Datei pflegen zu müssen, bietet sich das Apache2 Modul mod_auth_pam an. Allerdings nur wenn man weiss wie.

```
$ aptitude install libapache2-mod-auth-pam
```


```
$ vim /etc/apache2/sites-availabe/ftp.domain.com
<Location /dir>
AuthType Basic
AuthName "FTP-Auth"
AuthPAM_Enabled On
AuthBasicAuthoritative Off
Require user FTPUSER
</Location>
```


Sehr wichtig an dieser Stelle **AuthBasicAuthoritative Off . **Ansonsten Internal Server Error. Die Schnittstelle mit der sich Apache2 gegen PAM anmeldet, wird automatisch definiert.

```
# cat /etc/pam.d/apache2
@include common-auth
@include common-account
```


Nunja, auch wenn eigentlich soweit alles klar sein sollte, schmeisst Apache2 einen Internal Server Error.

```
Mar  1 12:37:59 host unix_chkpwd[2682]: password check failed for user (FTPUSER)
Mar  1 12:37:59 host apache2: pam_unix(apache2:auth): authentication  failure; logname= uid=xx euid=xx tty= ruser= rhost=123.123.123.123   user=FTPUSER
```


Was an den fehlenden LeseRechten des Users "www-data" liegt. Fügt man diesen der Gruppe shadow hinzu, funktioniert die Authentifizierung einwandfrei.

```

$ vi /etc/group
shadow:x:42:www-data
```


Zusätzliche Hilfe:
[http://pam.sourceforge.net/mod_auth_pam/configure.html](http://pam.sourceforge.net/mod_auth_pam/configure.html)
[http://biblio.l0t3k.net/howto/en/user-authentication-howto/x302.html](http://biblio.l0t3k.net/howto/en/user-authentication-howto/x302.html)