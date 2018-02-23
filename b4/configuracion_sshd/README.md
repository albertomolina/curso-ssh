# Configuración del servidor ssh

Tal como vimos en la instalación y configuración elemental del
servidor ssh, podemos ver las opciones de configuración que se aplican
a nuestro servidor mediante:

```
sshd -T |less
```

Vamos a comentar algunas que pueden ser interesantes porque se
modifican con cierta frecuencia.

### Port 22

Puerto tcp en el qu va a escuchar peticiones el servidor ssh, por
defecto es el 22/tcp, pero puede cambiarse sin problema.

### AddressFamily any|inet|inet6

El protocolo o protocolos de red a utilizar, puede ser inet(IPv4),
inet6 (IPv6) o any(ambos) que es la opción por defecto.

### ListenAddress

Si queremos especificar que se permitan conexiones sólo a través de
una dirección IP concreta (IPv4/IPv6).

### logingracetime, maxauthtries y maxsessions

Para especificar el tiempo que se espera para que el usuario se acceda
con éxito al sistema, el número máximo de veces que puede introducir
la contraseña y el número máximo de sesiones en la misma conexión.

### loglevel y syslogfacility

Para controlar el nivel de detalle que mostrar en los logs del
sistema, así como la "facility" a utilizar.

### hostkey

Fichero o ficheros que especifican la clave o claves privadas a
utilizar por el servidor.
