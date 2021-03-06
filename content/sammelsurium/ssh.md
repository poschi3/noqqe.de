---
title: ssh
date: 2013-07-27T12:15:30
tags:
- Software
- ssh
---

## Client Options

Passwort Auth erzwingen

```
ssh host -o PasswordAuthentication=yes -o PubkeyAuthentication=no
```

Speziellen Port angeben

```
ssh host -p22
```

Speziellen Key angeben. Nicht das .pub!

```
ssh host -l noqqe -i ~/.ssh/private
```

## Secure ciphers

Vorsicht. Ändert sich auch immer mal wieder.

Im Zweifel mal hier nachschauen. [Crypto Hardening](https://bettercrypto.org/static/applied-crypto-hardening.pdf)

```
## Crypto shit
Ciphers aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes128-ctr
MACs    umac-128-etm@openssh.com,hmac-sha2-512,hmac-sha2-256,hmac-ripemd160
KexAlgorithms diffie-hellman-group-exchange-sha256,diffie-hellman-group14-sha1,diffie-hellman-group-exchange-sha1
```

## Escape Characters

```
~. Disconnect.
~^Z Background ssh.
~## List forwarded connections.
~& Background ssh at logout when waiting for forwarded connection / X11 sessions to terminate.
~? Display a list of escape characters.
~B Send a BREAK to the remote system (only useful for SSH protocol version 2 and if the peer supports it).
~C Open command line. Currently this allows the addition of port
forwardings using the -L and -R options (see above). It also
allows the cancellation of existing remote port-forwardings using
-KR[bind_address:]port. !command allows the user to execute a
local command if the PermitLocalCommand option is enabled in
ssh_config(5). Basic help is available, using the -h option.
~R Request rekeying of the connection (only useful for SSH protocol
version 2 and if the peer supports it).
```

## Terminal Description

Um den sauberen Hostname im Tab zu haben.

```
Host *
PermitLocalCommand yes
LocalCommand if [[ $TERM == screen* ]]; then printf "\033k%h\033\\"; fi
```