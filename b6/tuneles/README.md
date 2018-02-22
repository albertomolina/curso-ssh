# Túneles ssh

Esta técnica, también conocida como TCP forwarding o port forwarding,
permite utilizar cualquier aplicación de forma segura a través de una
conexión ssh (de ahí la utilización del término túnel).

## Local forwarding

Desde nuestra máquina queremos acceder a un servicio, pero este
servicio o bien no es accesible (por ejemplo por un cortafuegos que lo
impide), o no es seguro hacerlo desde nuestra máquina, pero tenemos
acceso a otro equipo a través de ssh y desde ese equipo sí podemos
acceder al servicio que queremos o es seguro hacerlo. Vamos a verlo
con un ejemplo: queremos enviar un correo desde nuestro servidor de la
empresa (puerto 25/tcp), pero estamos fuera y no queremos conectarnos
de forma insegura, por lo que establecemos un túnel ssh de la
siguiente forma:

```
ssh -f -L 1025:smtp.example.com:25 remoto.example.com -N
```

Esto abrirá el puerto 1025/tcp en nuestra máquina que podremos
utilizar de forma segura para enviar correo, ya que se pasará a través
de ssh al equipo remoto.example.com y de ahí al servidor
smtp.example.com.

## Remote forwarding

Supongamos el caso contrario, en el que queremos que una máquina
remota utilice un servicio de nuestra máquina, pero o bien no es
accesible de forma directa o bien no es seguro hacerlo, por lo que
podríamos definir una conexión ssh para realizarlo como la siguiente:

```
ssh -f -L 8080:localhost:80 remoto.example.com -N
```

De esa manera desde el equipo remoto, quien acceda al puerto 8080/tcp
estará accesiendo al puerto 80/tcp de nuestra máquina a través de un
túnel ssh.

La directiva "GatewayPorts yes|no" limita el puerto 8080 del caso
anterior a que se pueda acceder sólo desde el propio equipo remoto, o
bien desde cualquier equipo.

## Forwarding dinámico

Mediante este mecanismo, lo que hacemos es crear un servidor SOCKS en
nuestro equipo, que permite utilizar desde nuestra máquina cualquier
cliente que soporte este mecanismo, en particular un navegador
web. Definiríamos un servidor socks local que sale a través de un
túnel ssh con un equipo remoto de la siguiente forma:

```
ssh -D 8080 -f -C -q -N remote.example.com
```
