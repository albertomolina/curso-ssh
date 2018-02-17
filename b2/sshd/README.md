# Instalación de un servidor ssh

Habitualmente los equipos con alguna variedad de GNU/Linux traen un
servidor ssh instalado, en el caso de los sistemas Debian y derivados
el paquete que proporciona el servidor ssh se llama
openssh-server. Podemos comprobar si ya está instalado mediante la
instrucción:

```
dpkg -l |grep openssh-server
ii  openssh-server            ......
```

El fichero de configuración de este servicio se encuentra
habitualmente en /etc/ssh/sshd_config y contiene las opciones de
configuración.

Las opciones aplicadas las obtendríamos mediante:

```
grep -v '^$\|^#' /etc/ssh/sshd_config

ChallengeResponseAuthentication no
UsePAM yes
X11Forwarding yes
PrintMotd no
AcceptEnv LANG LC_*
Subsystem	sftp	/usr/lib/openssh/sftp-server
```

Sin embargo, en el caso de ssh hay muchas opciones que no vienen
definidas y que se asume un valor por defecto, lo que puede resultar
confuso. Sin embargo podemos utilizar la opción "-T: extended test
mode" que comprueba la validez del fichero de configuración y muestra
las opciones efectivas que se aplican (las que están habilitadas de
forma explícita y las que tienen valores por defecto (en sistemas
GNU/linux sshd sólo puede ejecutarlo un usuario privilegiado):

```
sshd -T
...
port 22
addressfamily any
listenaddress [::]:22
listenaddress 0.0.0.0:22
usepam yes
logingracetime 120
x11displayoffset 10
maxauthtries 6
maxsessions 10
clientaliveinterval 120
clientalivecountmax 3
streamlocalbindmask 0177
permitrootlogin without-password
ignorerhosts yes
ignoreuserknownhosts no
hostbasedauthentication no
hostbasedusesnamefrompacketonly no
pubkeyauthentication yes
kerberosauthentication no
kerberosorlocalpasswd yes
kerberosticketcleanup yes
gssapiauthentication no
gssapikeyexchange no
gssapicleanupcredentials yes
gssapistrictacceptorcheck yes
gssapistorecredentialsonrekey no
passwordauthentication yes
kbdinteractiveauthentication no
challengeresponseauthentication no
printmotd no
printlastlog yes
x11forwarding yes
x11uselocalhost yes
permittty yes
permituserrc yes
strictmodes yes
tcpkeepalive yes
permitemptypasswords no
permituserenvironment no
compression yes
gatewayports no
usedns no
allowtcpforwarding yes
allowagentforwarding yes
disableforwarding no
allowstreamlocalforwarding yes
streamlocalbindunlink no
useprivilegeseparation sandbox
fingerprinthash SHA256
pidfile /run/sshd.pid
xauthlocation /usr/bin/xauth
ciphers chacha20-poly1305@openssh.com,aes128-ctr, ...
macs umac-64-etm@openssh.com,umac-128-etm@openssh.com, ...
versionaddendum none
kexalgorithms curve25519-sha256,curve25519-sha256@libssh.org, ...
hostbasedacceptedkeytypes ecdsa-sha2-nistp256-cert-v01@openssh.com, ...
hostkeyalgorithms ecdsa-sha2-nistp256-cert-v01@openssh.com, ...
pubkeyacceptedkeytypes ecdsa-sha2-nistp256-cert-v01@openssh.com, ...
loglevel INFO
syslogfacility AUTH
authorizedkeysfile .ssh/authorized_keys .ssh/authorized_keys2
hostkey /etc/ssh/ssh_host_rsa_key
hostkey /etc/ssh/ssh_host_ecdsa_key
hostkey /etc/ssh/ssh_host_ed25519_key
acceptenv LANG
acceptenv LC_*
authenticationmethods any
subsystem sftp /usr/lib/openssh/sftp-server
maxstartups 10:30:100
permittunnel no
ipqos lowdelay throughput
rekeylimit 0 0
permitopen any
```

