# Clase nº 3: 09/05/2020

Sacado del video: https://www.youtube.com/watch?time_continue=3&v=WK7B7d1Zne4&feature=emb_logo

# Protocolo FTP:
Se emplea menos que antes, pero aún se utiliza para poder mantener servidores y administrar sitios web.

Su nombre significa _File Transfer Protocol_ sirve para transferir ficheros entre un cliente y un servidor, el cual permite
navegar por los directorios de ambos.
Una de las cosas curiosas que tiene FTP es que crea dos conexiones, canales; por un canal se envían las señales de control y por
el otro canal, los datos.
Primero el cliente establece la conexión de control del puerto 21 del servidor, proporcionando clave y contraseña en caso de ser
necesario. Una vez establecida, el cliente puede enviar comandos para navegar por los directorios del servidor, buscando la
ubicación que le interese.

A la hora de conectarse con un servidor FTP, el cliente podría utilizar
	- el modo ACTIVO (o modo PORT): Eligiendo un puerto aleatorio, por encima del 1024 y conectándose con el puerto 20 del
	  servidor para enviar esos datos. Por ejemplo: el cliente elije el puerto 2000, envía un comando PORT al servidor
	  indicándole el puerto que ha elegido, de manera que el servidor pueda abrirle una conexión de datos donde se
	  transferirán los archivos y los listados en ese puerto. Esto tiene un *PROBLEMA DE SEGURIDAD* y es que la máquina
	  cliente debe estar dispuesta a aceptar cualquier conexión por sobre el puerto 1023, con los problemas que ello implica en
	  el firewall.
	- Para solucionar el problema de seguridad que acarrea el Modo Activo, se desarrolló el Modo Pasivo (o modo PASV): cuando
	  el cliente envía un comando PASV por el canal de control, el servidor FTP le indica su puerto (el puerto del servidor,
	  el cual debe ser mayor que el 1023), por ejemplo 2040, al que debe conectarse el cliente. Por lo que el cliente es el
	  que inicia la conexión desde el puerto siguiente al puerto que tenga el control (recordar que son dos puertos los que
	  abre el cliente, uno para control (por ejemplo 1034) y otro para datos (por ejemplo 1035)). Y como es el cliente el que
	  inicia la conexión, no hay problemas de seguridad. En caso de tener que abrir otro fichero, se debe abrir otra conexión.

Al final el protocolo FTP, sirve para lo mismo que el protocolo HTTP, traer ficheros de un servidor y ambos se ejecutan sobre TCP;
de hecho los navegadores también interpretan el protocolo FTP, por lo que también sirven como clientes de este protocolo.

## Diferencia entre FTP y HTTP:
	- La principal diferencia que existe entre FTP y HTTP es el uso que hace FTP de las dos conexiones (una parte para
	  control, donde envía los comandos y otra para los datos). Mientras que en HTTP tanto comandos como datos, viajan sobre
	  la misma conexión, mediante los mensajes HTTP.
	- HTTP envía la información de cabecera de solicitud y respuesta por la misma conexión TCP que transfiere el archivo (en
	  banda), mientras que FTP envía la información de control *fuera de banda* (lo mismo que realiza el protocolo RTSP).
	- El servidor FTP tiene que mantener el estado, ya que debe recordar la carpeta en la que se encuentra el usuario, el modo
	  de transferencia de ficheros que le han sido solicitados, etc.

## Comandos FTP y respuestas:
	- Los comandos del cliente-Servidor y respuestas Servidor-Cliente se envían por la conexión de control ASCII (legibles por
	  el humano) como en HTTP, junto con algún argumento.
	  Comandos por ejemplo pueden ser:
	  	* USER: Para indicar el nombre del usuario
		* PASS: Para indicar el password
		* LIST: Solicitar una lista de archivos que se encuentran en el directorio.
	  Las respuestas, como al igual que sucede en HTTP, tienen un código numérico y una frase explicativa.
	  	* 331 Username OK
		* 125 Data connection already open; transfer starting.
		* 425 Can't open data connection.
		* 452 Error writing File.
	- Cada comando va en una línea y constra de cuatro caracteres ASCII y argumentos opcionales.
	- Cada comando tiene una respuesta de tres dígitos con un mensaje opcional.

Hoy en día se utiliza el protocolo seguro denominado SFTP, en lugar de FTP, para que todo el tráfico vaya cifrado entre cliente y
servidor.

# Protocolos para Correo Electrónico:

Sacado del video: https://www.youtube.com/watch?v=cCnaDwsLQ58&feature=emb_logo

Como elementos fundamentales para un servicio de Correo Electrónico se encuentran:
	- Los clientes de Correo: que pueden ser aplicaciones específicas como Thunderbird u Outlook, o directamente desde el
	  navegador.
	  	* Compone, edita, lee y envía mensajes de correo.
		* Los correos entrantes y salientes se almacenan en el servidor.
	- Servidores de correo que tendrán:
	  	* Buzón (o mail box) que contiene los mensajes entrantes de un usuario.
		* Cola de mensajes de los correos salientes que tienen que ser enviados.
	- Protocolo SMTP (Simple Mail Transfer Protocol), el cual comunicará los servidores de correo con otros.
	  	* Utiliza TCP para enviar mails entre servidores de correo.
		* Lado cliente en el servidor de correo del emisor y lado servidor en el servidor de correo del destinatario.

Ejemplo de envío de correo electrónico:
	Si Alicia quiere enviar un correo a Bob, entonces lo que sucede es lo siguiente:
	- Alicia utiliza un Cliente de Correo (A), en el cual escribe o edita el correo deseado, y luego lo envía.
	- El Agente/Cliente de Correo de Alicia (B) envía el mensaje a su servidor de corro (un servidor aparte del Cliente que
	  utiliza Alicia) que lo pone en su cola de mails.
	- El Servidor de Correo utiliza SMTP para contactarse con el otro Servidor de Correo (C) que utilizará el Cliente/Agente
	  de Bob (D), y abre una conexión TCP.
	- El cliente SMTP envía el mensaje de Alicia por la conexión TCP (de B a C).
	- EL Servidor de Correo de Bob (C) almacena el mansaje en su buzón (mailbox).
	- Bob invoca a su Agente de Correo (D) para leerlo cuando quiera.

SMTP [RFC 5321]: es un protocolo bastante viejo, de hace unos 30 años, por lo que hace que tenga ciertas restricciones:
	- Restricción para el cuerpo y cabecera del mensaje a uan codificación en ASCII de 7 bits.
	- Esto requiere que los datos binarios de archiovs multimedia sean codificados en ASCII antes de ser enviados por SMTP,
	  así como su posterior decodificación. Para esto se utiliza normalmente MIME.
	- HTTP no tiene esta restricción de codificación en ASCII de objetos "No textuales".

SMTP Utiliza TCP para transferencia fiable de correo desde un cliente al servidor, puerto 25 del Servidor. SMTP no utiliza
servidores intermedios y si el servidor de destino se encuentra caído el correo se queda en el servidor Correo Origen que vuelve a
intentar varias veces enviarlo, hasta que lo consigue o falla y avisa al usuario al cabo de determinado tiempo.
Por lo tanto la transferencia se dice que es en tres fases:
	- El cliente especifica el correo electrónico del emisor y destinatario.
	- Una vez reconocido, el servidor envía el mensaje.
	- El cliente repite este proceso para el resto de los mensajes. En caso contrario cierra la conexión.

Comandos y respuestas para SMTP:
	- Comandos: enviados en ASCII.
	- Respuestas: código de estado y frase explicativa, como HTTP o FTP.

El correo se envía entre servidores en tres fases:
	- Primero se indican las direcciones de emisor y receptor.
	- Después se envía el mensaje.
	- Y por último, si se tienen más mensajes para ese servidor, se envían sobre esa misma conexión y sino se cierra.


## Comparación entre SMTP y HTTP:
Semejanzas:
	- Ambos son utilizados para transferir ficheros entre máquinas.
	- Tanto HTTP (de forma persistente) como SMTP utilizan conexiones permanentes.
	- Ambos tienen interacción ASCII para los comandos/respuesta y sus código de estado.
Diferencias:
	- SMTP requiere que los mensajes (cabecera y cuerpo) estén codificados en ASCII de 7 bits.
	- Los servidores SMTP utilizan CRLF (retorno de carro) para determinar el final de los mensajes.
	- HTTP es un protocolo tipo pull, mientras que SMTP es de tipo push, porque la máquina que inicia la comunicación es la
	  que quiere enviar un fichero al otro servidor de correo. Mientras que en HTTP que es pull el que envía el fichero es el
	  Servidor, que recibe la conexión del cliente.
	- Además, en HTTP cada objeto se encapsula en el propio mensaje de respuesta, pero en SMTP múltiples objetos se envía en
	  un único mensaje.
	  Es decir, que en HTTP viaje un objeto por mensaje de respuesta, mientras que en SMTP, pueden viajar varios objetos por
	  mensaje.

Formato de los mensajes de correo Y NO DE LOS MENSAJES SMTP (sino de los que éstos últimos llevan dentro) se siguen un formato
definido dentro de la RFC 5322, con varias líneas de cabecera (To, From, Subject, todos ellos distintos a los comandos SMTP), en
las cuales se indican la dirección correo de origen y destino, etc. y después tras una línea en blanco, el cuerpo del mensaje en
código ASCII.

## Protocolos de Acceso al correo:
Lo anteriormente hablado es para el envío de correo entre Servidores. ¿Cómo hace para acceder desde un Agente a un correo? Para
ello existe los protocolos de acceso al correo. Entre ellos se encuentran:
	- POP3: Post Office Protocol versión nº 3 [RFC 1939] el cual determina una autorización entre Agente y Servidor, la
	  descarga y actualización.
	- IMAP: Internet Mail Access Protocol [RFC 3501] el cual tiene muchísima más funcionalidad pero es más complejo; también
	  permite la manipulación de mensajes almacenados en el servidor.
	- HTTP: También puede ser utilizado como los que usan los ya conocidos: Gmail, Hotmail, Yahoo! Mail, etc.

IMAP: Es mucho más potente que POP3, ya que permite gestionar el buzón de correo creando carpetas para poder organizar los correos
en el propio servidor por lo que tiene memoria de estado entre sesiones.

POP3: Nos permitirá acreditarnos a nuestro Servidor para poder acceder a nuestro buzón, y descargar así los correos que tengamos
almacenados en él. Por defecto, POP3 descarga los correos al Agente de Mail y los borra del Servidor (aunque esto sse puede
configurar para que los deje almacenados).
	- Al conectarse, lo que hace primero es acreditarse para poder acceder al buzón.
	- Luego, pide un listado de los correos y los va descargando y marcando para ser borrados.
	- Al final, se envía la orden para que el Servidor borre los correos que han sido marcados para borrar.


