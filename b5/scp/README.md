# scp

El cliente ssh incluye también el comando "scp" que permite copiar
ficheros entre entre equipos mediante ssh y hacerlo de forma
equivalente a la utilización local del clásico comando "cp".

No es necesario que el equipo origen o destino sea el equipo desde el
que se ejecuta scp, tanto origen como destino pueden ser equipos a los
que pueda acceder el usuario utilizando ssh.

La sintaxis general de scp es:

```
scp [[user@]host1:]file1 [[user@]host2:]file2
```

## Transferir un fichero local a un equipo remoto

```
scp  /etc/resolv.conf usuario@172.22.200.175:resolv.conf.local
```

El fichero remoto quedará como /home/usuario/resolv.conf.local ya
que : indica el punto de acceso al equipo (/home/usuario/ en este
caso)

## Transferir un fichero desde un equipo remoto a mi equipo local

```
scp root@172.22.200.175:/etc/shadow .
```

Que guardaría el fichero /etc/shadow del equipo remoto con el nombre
shadow en el directorio en el que nos encontramos

## Transferir un fichero entre dos equipos remotos

```
scp root@172.22.200.175:/etc/hosts root@172.22.200.176:/etc/hosts
```

Esta opción es muy potente y permite crear sencillos scripts para
unificar configuraciones, por ejemplo imaginemos que queremos tener la
misma configuración DNS en un conjunto de servidores, podríamos
hacerlo de forma sencilla y potente con ssh mediante el siguiente
script:

```
#!/bin/bash

for i in `seq 1 100`; do
  scp root@servidor1:/etc/resolv.conf 192.168.1.$i:/etc/resolv.conf
done
```

## Bonus track

Si utilizamos pares de claves en las conexiones, scp autocompleta el
fichero origen o destino utilizando el doble tabulador 😎.
