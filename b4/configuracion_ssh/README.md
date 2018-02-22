# Configuración del cliente ssh

Existe el fichero /etc/ssh/ssh_config en el que se especifican los
parámetros de configuración generales que van a utilizar por defecto
todos los clientes ssh que se ejecuten en esa máquina.

Los posibles parámetros que podemos definir en ese fichero se detallan
en la página 5 del manual de ssh_config.

## Algunos parámetros significativos

### SendEnv LANG LC_*

Mediante este parámetro se define  en el equipo remoto los parámetros
de localización del cliente, siempre que estos estén definidos
allí. Por ejemplo, supongamos que la variable local sea
LANG=es.ES.UTF-8, seguirá siendo en el equipo remoto siempre que
exista, en caso contrario se pondrá la localización por defecto del
sistema:

```
usuario@cliente:~$ echo $LANG
es_ES.UTF-8
usuario@cliente:~$ ssh root@servidor1

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
No mail.
Last login: Thu Feb 22 10:06:47 2018 from ...
root@servidor1:~# echo $LANG
en_US.UTF-8
```
### HashKnownHosts yes|no

Para ofuscar mediante un hash la IP o el nombre de los servidores de
los que almacenamos las claves públicas en el fichero
~/.ssh/known_hosts

### GSSAPIAuthentication yes|no

Para habilitar este método de autenticación, en sistemas Debian viene
habilitado, aunque sólo es necesario en los casos en los que se vaya a
usar este método de autenticación.

### Carácter de escape

En algunas ocasiones podemos tener problemas con nuestra conexión ssh
y que la shell remota no responda a las instrucciones que mandamos, en
esos casos siempre se podrá cerrar la conexión mediante el carácter de
escape que hayamos definido en nuestro cliente ssh, este caracter por
defecto es "~":

```
EscapeChar ~
```

Para ejecutarlo escribiríamos la secuencia "~."

### ForwardAgent, ForwardX11

Explicados en la sección específica de forwarding

### GlobalKnownHostsFile fichero

Permite la utilización de un fichero known_hosts para todos los
usuarios de un equipo.

### NumberOfPasswordPrompts

Número de intentos de acceso con contraseña. Por defecto es 3

### StrictHostKeyChecking yes|ask|no

Parámetro muy importante, utiliza para la gestión de las claves
públicas de los equipos remotos. La opción por defecto es "ask", de
manera que si no se ha almacenado previamente la clave pública se
pregunta qué hacer. En el caso de ponerla en "yes", se rechazará una
conexión si no existe previamente la clave pública y en caso de optar
por la opción "no", no se hará ninguna verificación.

### UserKnownHostsFile fichero

Fichero known\_hosts de usuario, por defecto ~/.ssh/known\_hosts




