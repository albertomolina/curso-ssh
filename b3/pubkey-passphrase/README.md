## Autenticación con clave pública y frase de paso

La autenticación con clave privada tiene importantes ventajas respecto
al acceso con contraseña, pero tiene el inconveniente de la custodia
de la clave privada. Cualquier usuario que obtuviese nuestra clave
privada podría entrar en nuestra cuenta de cualquier equipo en el que
tuviésemos exportada la correspondiente clave pública. Para aumentar
la seguridad en esta situación se utiliza una frase de paso para
proteger la clave privada, frase que se introduce al crear la clave
privada o que puede modificarse a posteriori. Vamos a crear una nueva
clave, pero en este caso protegida con frase de paso:

```
ssh-keygen -t ecdsa
Generating public/private ecdsa key pair.
Enter file in which to save the key (/home/alberto/.ssh/id_ecdsa): 
Enter passphrase (empty for no passphrase): <- Teclear frase de paso
Enter same passphrase again: <- Teclear frase de paso
Your identification has been saved in /home/alberto/.ssh/id_ecdsa.
Your public key has been saved in /home/alberto/.ssh/id_ecdsa.pub.
The key fingerprint is:
SHA256:mvCLrZMvdUDOorOkvd/1iosAZmhGS2fWPQmjAVMjjtk alberto@mut
The key's randomart image is:
+---[ECDSA 256]---+
| +o+ o           |
|ooo = = .        |
|o+E= = +         |
|+ = . + .        |
|o* ... .S        |
|=.+  o.o.        |
| +.o o+..        |
|. o.+= + .       |
|  .o==B....      |
+----[SHA256]-----+
```

De esta forma la clave privada no es útil a menos que se conozca la
frase de paso.


Procedemos de igual forma que en el caso anterior, exportando la clave
pública, aunque ahora cada vez que accedamos nos solicitará la frase
de paso de la clave privada:

```
ssh -i ~/.ssh/id_ecdsa debian@172.22.200.175
Enter passphrase for key '/home/alberto/.ssh/id_ecdsa':
```

Hemos ganado en seguridad, pero hemos perdido en usabilidad, porque
ahora tenemos que escribir la frase de paso cada vez que accedamos al
equipo remoto y además no es válido para procesos no
interactivos. Para solucionar este inconveniente usaremos ssh-agent en
una sección posterior.
