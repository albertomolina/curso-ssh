# Autenticación con claves pública/privada

Aunque el mecanismo más fácil de entender al utilizar ssh es la
autenticación del usuario mediante la contraseña en el equipo remoto,
el mecanismo más "natural" y probablemente más habitual es la
autenticación mediante un par de claves pública/privada.

## Creación de la clave privada

Para crear la clave privada utilizaremos la herramienta ssh-keygen,
especificando el algoritmo que deseamos utilizar mediante el parámetro
-t (dsa | ecdsa | ed25519 | rsa | rsa1), por ejemplo:

```
ssh-keygen -t ecdsa
```

Se creará un diálogo mediante el cual nos pedirá una frase de paso
para proteger la clave privada, paso que ignoraremos de momento y
explicaremos con detalle en la siguiente sección:

```
Generating public/private ecdsa key pair.
Enter file in which to save the key (/home/alberto/.ssh/id_ecdsa): [ENTER]
Enter passphrase (empty for no passphrase): [ENTER]
Enter same passphrase again: [ENTER]
Your identification has been saved in /home/alberto/.ssh/id_ecdsa.
Your public key has been saved in /home/alberto/.ssh/id_ecdsa.pub.
The key fingerprint is:
SHA256:QQ0bm3FBXKhLyWUfa7teeHgufwLPdK8nIu0UlMCJ6/M alberto@mut
The key's randomart image is:
+---[ECDSA 256]---+
|        =B==.    |
|       ..BO...   |
|       .=* .oo   |
|        *. .+    |
|       oS. ...   |
|        +   o+. .|
|         o .+*+..|
|          E.=== +|
|           +o++* |
+----[SHA256]-----+
```

En este caso hemos optado por dejar el nombre de la clave por defecto
(~/.ssh/id_ecdsa). Si vamos al directorio ~/.ssh veremos que existen
dos nuevos ficheros, que se corresponden con la clave pública y la
privada:

```
-rw------- 1 alberto alberto   227 feb 18 09:16 id_ecdsa
-rw-r--r-- 1 alberto alberto   173 feb 18 09:16 id_ecdsa.pub
```

Lógicamente la clave privada se ha protegido en el sistema de forma
que sólo el propietario puede leerla o modificarla, mientras que la
pública puede leerla cualquier usuario y en general podrá estar
accesible en cualquier sitio sin restricciones, ya que no es posible
obtener la clave privada a partir de ella.

Vemos el contenido de estas claves (obviamente se muestran aquí a modo
de ejemplo y se trata de claves que no se van a utilizar nunca en un
entorno real):

```
~/.ssh/id_ecdsa
-----BEGIN EC PRIVATE KEY-----
MHcCAQEEIN53r8/ghcQ94wjNPtvz0VvSFsuU7ePsPkriWPhpC137oAoGCCqGSM49
AwEHoUQDQgAEXJKU4yRlIdnKGG8qQA2PXpfCPVz9xpbB3TXOh9ymC9XtjgP3ZCwU
tdNnLTQNJm8PO4MHtFZBTxeFE39lD7WVYQ==
-----END EC PRIVATE KEY-----

~/.ssh/id_ecdsa.pub 
ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAy\
NTYAAABBBFySlOMkZSHZyhhvKkANj16Xwj1c/caWwd01zofcpgvV7Y4D92QsFLXT\
Zy00DSZvDzuDB7RWQU8XhRN/ZQ+1lWE= alberto@mut
```

## Copia de la clave pública en el equipo remoto

Para que se pueda utilizar este mecanismo de autenticación es preciso
que la clave pública del usuario se encuentre en la cuenta que éste
posee en el equipo remoto, de forma más concreta, dentro del fichero
~/.ssh/authorized\_keys, por lo que debemos utilizar algún método para
ubicarla allí:

* Accedemos con contraseña y copiamos y pegamos la clave pública
* Accedemos con otra clave pública que hubiésemos copiado previamente
  y pegamos la nueva clave pública
* Utilizamos cualquier sistema en el arranque de la máquina que
  obtenga la clave pública y la ubique en su sitio (muy habitual en
  cloud computing)
* Utilizamos la herramienta ssh-copy-id

Vamos a ver el último método:

```
ssh-copy-id -i ~/.ssh/id_ecdsa debian@172.22.200.175

/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/alberto/.ssh/id_ecdsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
debian@172.22.200.175's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'debian@172.22.200.175'"
and check to make sure that only the key(s) you wanted were added.
```

Si accedemos al equipo remoto, podremos comprobar que la clave pública
que hemos exportado se encuentra en el fichero
~/.ssh/authorized\_keys.

## Clave privada con nombre no estándar

En el caso anterior hemos creado un par de claves con nombre estándar
(id\_ecdsa e id\_ecdsa.pub), pero es posible definir cualquier nombre
a la hora de crear el par de claves, por ejemplo:

```
ssh-keygen -t ed25519
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/debian/.ssh/id_ed25519): openwebinars
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in openwebinars.
Your public key has been saved in openwebinars.pub.
The key fingerprint is:
SHA256:HzEVg7wVelxLpuHrv+BUG8QG4bI0AsED7PnUvzFdlCI debian@asd
The key's randomart image is:
+--[ED25519 256]--+
|   ..oo. . .O++. |
|    . o.  E*oXo. |
|   . . o. B+*o=  |
|    o . .o.B +.  |
|     o  S.o...o  |
|      .  .+o.. o |
|          .++ .  |
|          .o o   |
|            . o. |
+----[SHA256]-----+
```

Procederíamos de igual forma que en el caso anterior, aunque ahora
para utilizar la clave en cualquier sesión ssh, deberíamos indicarlo
de forma explícita:

```
ssh -i ~/.ssh/openwebinars debian@172.22.200.175
```

## Utilización en cloud computing

Hoy en día es cada vez más habitual la utilización de máquinas
virtuales en algún proveedor de nube de infraestructura pública o
privada (AWS, Azure, OpenStack, etc.), en estos casos es
imprescindible utilizar este mecanismo de clave pública/privada para
acceder a estas máquinas virtuales.

## Generación de una clave pública a partir de la privada

Aunque habitualmente se generan ambas claves, en diferentes
circunstancias puede ocurrir que tengamos la clave privada, pero no la
correspondiente clave pública, en ese caso podemos utilizar ssh-keygen
para obtenerla:

```
ssh-keygen -y -f clave >> clave.pub
```

Evidentemente si tenemos la clave pública y no la privada, no podemos
hacer el proceso inverso.

## Utilización en procesos no interactivos

Puesto que teniendo acceso a la clave privada el acceso se puede
realizar al equipo remoto sin ninguna intervención, este mecanismo es
ideal para su utilización en procesos que no requieran intervención
humana, como muchas conexiones que pueden realizarse entre diferentes
equipos. La conexión es segura y autenticada, aunque es muy importante
custodiar adecuadamente las claves privadas.
