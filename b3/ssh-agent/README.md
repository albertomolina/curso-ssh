# ssh-agent

ssh-agent es un programa que permite almacenar las claves privadas de
una sesión y es muy útil cuando usamos claves con frase de paso, ya
que podemos añadir la clave privada al agente ssh y sólo tendremos que
poner la frase de paso una vez, permitiendo utilizar ssh de forma 
transparente sin volver a introducir la frase de paso todo el tiempo
que dure la sesión del usuario.

ssh-agent se suele ejecutar automáticamente en las sesiones gráficas
de los sistemas, como podemos verificar mediante:

```
env |grep SSH
...
SSH_AUTH_SOCK=/run/user/1001/keyring/ssh
SSH_AGENT_PID=2743
```

O a través de ps:

```
ps aux |grep ssh-agent
alberto   2743  .... ..... 0:00 /usr/bin/ssh-agent x-session-manager
```

De hecho, si tuviéramos alguna clave privada sin frase de paso se
habría cargado automáticamente en el agente ssh y podríamos utilizarla
de forma totalmente transparente.

## Añadir una clave privada a ssh-agent

Mediante la herramienta ssh-add podemos añadir una clave al agente
ssh, por ejemplo:

```
ssh-add ~/.ssh/openwebinars
```

Si la clave está protegida por una frase de paso, se nos pedirá en ese
momento, o se utilizará la aplicación ssh-askpass si se tratase de una
aplicación gráfica u otra que no tuviese asociada una terminal.

Podemos ver las claves cargadas mediante:

```
ssh-add -L
```

Y sus huellas con:

```
ssh-add -l
```

ssh-agent permite que cualquier otra aplicación de la misma sesión
utilice las claves privadas que almacena sin tener que volver a
autenticarse, por lo que es importante controlar el uso de la sesión,
bloqueándola cuando no se esté usando.

Se pueden eliminar claves ssh del agente mediante:

```
ssh-add -d openwebinars
```

O incluso eliminar todas las claves con:

```
ssh-add -D
```

## Ejecución de ssh-agent

En el caso de que utilicemos un sistema que no haya cargado
automáticamente un agente ssh, podemos ejecutarlo directamente,
habitualmente se haría abriendo una nueva shell:

```
ssh-agent /bin/bash
```

