# Forwarding: Agent forwarding y X forwarding

## Agent forwarding

Mediante esta técnica, es posible que el cliente ssh se comunique con
un agente ssh que corre un una máquina remota y sin necesidad de poner
las claves privadas en él, poder saltar a otro equipo remoto no
accesible directamente. Es una técnica muy útil que permite no exponer
directamente los servidores a los que realmente queremos acceder
mediante ssh, sino que accedemos a ellos de forma transparente usando
un equipo a modo de bastión.

Esta posibilidad debe estar habilitada en el servidor intermedio
mediante la directiva:

```
allowagentforwarding yes
```

Que está habilitada por defecto.

En el cliente es necesario habilitar el parámetro:

```
ForwardAgent yes
```

## X11 forwarding

A través de la técnica de X11 forwarding podemos ver en nuestra
pantalla aplicaciones gráficas que se ejecutan a través de ssh en un
equipo remoto (y viceversa).

Aunque en primer lugar tiene que estar permitido en el servidor a
través de las directivas:

```
X11Forwarding yes
X11DisplayOffset 10
```

El cliente para conectarse y utilizar esta funcionalidad, deberá
configurar adicionalmente la opción:

```
ForwardX11 yes
```

Al conectarnos por ssh podremos comprobar que está definida la
variable **DISPLAY** con un valor definido a través de la variable
X11DisplayOffset, por ejemplo:

```
env |grep DISPLAY
DISPLAY=localhost:0
```

Al ejecutar una aplicación en el equipo remoto sobre la conexión ssh
nos aparecerá en nuestra pantalla.
