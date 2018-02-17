# Autenticación con usuario y contraseña

El método inicial de autenticación es utilizando los usuarios del
sistema y las contraseñas que tienen almacenadas en el sistema. A SSH
no le afecta la forma en la que el sistema almacena las contraseñas, 
entra en la forma en la que se almacenan las contraseñas (fichero,
LDAP, etc.).

Las opciones de configuración que afectan en este caso son las
siguientes:

```
passwordauthentication yes|no
challengeresponseauthentication yes|no
permitemptypasswords yes|no
```

Teóricamente challengeresponseauthentication es un mecanismo más
complejo que permite preguntar al usuario otras cuestiones, no sólo la
contraseña, pero en la práctica se suele preguntar la contraseña.

En sistemas GNU/Linux se añade la opción

```
usepam yes
```

Que permite utilizar el subsistema PAM como mecanismo de
autenticación.

## Ejercicio simple de acceso con usuario/contraseña

Accedemos a un servidor remoto con:

```
ssh usuario@172.22.200.175
The authenticity of host '172.22.200.175 (172.22.200.175)' can't be established.
ECDSA key fingerprint is SHA256:Bsv9OS7Qf94ANguOiDLNPHn7J+XlwisWZydmfqa4QMo.
Are you sure you want to continue connecting (yes/no)? 
```

A lo que tecleamos "yes" y a continuación nos pide la contraseña de
acceso, la introducimos y accedemos a una shell en el equipo remoto:


```
Warning: Permanently added '172.22.200.175' (ECDSA) to the list of known hosts.
usuario@172.22.200.175's password: **********
Last login: Fri Feb 16 17:34:41 2018 from 172.23.0.22
usuario@host:~$ 
```

## Ejecución remota

SSH permite ejecutar una orden remotamente de forma no interactiva, lo
que resulta muy cómodo cuando hay que realizar tareas muy específicas
en un equipo remoto. Por ejemplo:

```
ssh usuario@172.22.200.175 sudo apt update
```

También se pueden encadenar varias órdenes o ejecutar un script:

```
ssh usuario@172.22.200.175 'sudo apt update && sudo apt upgrade'
```

## Consideraciones acerca de root

Se puede restringir el acceso con el usuario root utilizando
contraseña, aspecto importante desde el punto de vista de seguridad,
por lo que hoy en día habitualmente se utiliza la opcion:

```
PermitRootLogin without-password
```

En caso de que quisiéramos permitir acceder con el usuario root y
contraseña, deberíamos poner esta opción a "yes".

## Otras opciones

No específicas del acceso con usuario y contraseña, pero adecuadas
para empezar:

```
PrintLastLog yes|no
PrintMotd yes|no
Banner Ruta_a_fichero
```


