# Clientes ssh

Prácticamente cualquier variedad de UNIX hoy en día incorpora de serie
un cliente ssh, en el caso de GNU/Linux es OpenSSH y para los sistemas
debian y derivados, se encuentra en el paquete openssh-client.

La configuración general del cliente ssh se encuentra habitualmente en
el fichero /etc/ssh/ssh_config, aunque ya veremos más adelante en el
curso la forma de modificarlo adecuadamente.

En el caso de sistemas windows, podemos instalar diferentes
aplicaciones que proporcionan funcionalidad más o menos completa de
cliente ssh, quizás la aplicación más extendida en este caso es
[putty](https://www.putty.org/), que veremos en una sección específica
al final del curso.

Muy recientemente se ha anunciado la incorporación de [openssh a
windows 10](https://www.servethehome.com/say-farewell-putty-microsoft-adds-openssh-client-windows-10/),
en fase beta cuando se escribe esto y que permitirá, solo con 20 años
de retraso, utilizar ssh de forma nativa desde windows.
