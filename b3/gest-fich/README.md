# Gestión de los ficheros authorized\_keys y known\_hosts

## Fichero ~/.ssh/authorized\_keys

Se almacenan las claves públicas de los *usuarios* que pueden acceder
a esta cuenta mediante clave pública/clave privada, el formato es:

```
algoritmo clave_publica comentario
```

Por ejemplo:

```
ssh-rsa AAAAB3NzaC1yc2EAA...dPh alberto@mut
```

Si queremos que utilizar un par de claves para acceder a un equipo,
debemos asegurarnos de que exista la clave pública en este fichero y
cuando ya dejemos de utilizarla debemos borrar la línea
correspondiente.

## Fichero ~/.ssh/known\_hosts

Se almacenan las claves públicas de todos los *equipos remotos* a los
que nos hemos conectado y que hemos aceptado, el formato es:

```
nombre_o_IP_equipo algoritmo clave_pública
```

Actualmente es más habitual que no se guarde el nombre o dirección IP
del equipo en claro, sino que se almacene el hash. Para encontrar un
determinado equipo por nombre o dirección IP podemos utilizar la
instrucción:

```
ssh-keygen -F 172.22.200.175
# Host 172.22.200.175 found: line 88 
|1|lbA....9Lo= ecdsa-sha2-nistp256 AAAA.....ynTO90=
```

## Cambio de clave pública del servidor

Habitualmente se almacenan las claves públicas de los servidores a los
que nos hemos conectado previamente en el fichero ~/.ssh/known_hosts,
por lo que se verifica cada vez que se conecta que el servidor ofrece
la misma clave pública. En caso de que no coincida veremos el
siguiente mensaje:

```
ssh debian@172.22.200.175
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ECDSA key sent by the remote host is
SHA256:J9CMWSbavkqECRI1KWhy8s/D7UVJWDiysAocAbo1F6k.
Please contact your system administrator.
Add correct host key in /home/alberto/.ssh/known_hosts to get rid of this message.
Offending ECDSA key in /home/alberto/.ssh/known_hosts:88
  remove with:
  ssh-keygen -f "/home/alberto/.ssh/known_hosts" -R 172.22.200.175
ECDSA host key for 172.22.200.175 has changed and you have requested strict checking.
Host key verification failed.
```

Es posible que se trate de una suplantación y por tanto un problema de
seguridad, pero también es posible que se haya realizado un cambio en
el servidor que haya implicado un cambio en las claves del servicio
ssh o una situación muy habitual hoy en día: estamos reutilizando la
misma IP para un nuevo servidor.

En caso de que estemos seguros de que no hay ningún problema de
seguridad al acceder a ese equipo remoto, debemos eliminar la antigua
clave asociada a la dirección IP (o al nombre), mediante la
instrucción:

```
ssh-keygen -R 172.22.200.175
# Host 172.22.200.175 found: line 88
/home/alberto/.ssh/known_hosts updated.
Original contents retained as /home/alberto/.ssh/known_hosts.old
```
