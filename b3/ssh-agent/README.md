# ssh-agent

ssh es un programa que permite almacenar las claves privadas de una
sesión y es muy útil cuando usamos claves con frase de paso, ya que
podemos añadir la clave privada al agente ssh y sólo tendremos que
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

## Añadir una clave privada a ssh-agent

Mediante la herramienta ssh-add podemos añadir una clave al agente
ssh, por ejemplo:

```
ssh-add ~/.ssh/id_ecdsa
```
