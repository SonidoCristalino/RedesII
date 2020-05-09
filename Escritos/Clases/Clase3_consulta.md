# Clase 3 Consulta: 09/05/2020

Clase corta, y por lo tanto el cuestionario también es corto.

FTP: protocolo de transferencia de archivos y cómo trabaja.

	Protocolo que tiene dos canales:
		- puerto 21 que trabaja como de seguridad, manda ordenes de ver, escribir y borrar en los archivos.
		- puerto 20 cuando se quiere bajar o subir el archivo.

Tipos de conexiones:
-------------------
La diferencia entre Modo Activo y Pasivo es quién inicia la conexión cuando quiere bajar y subir un servicio.

	- FTP Activo: Es el que viene por defecto en los servidores. Es decir que el cliente inicia la conexión mediante el puerto
	  21 del servidor y el puerto Origen es uno aleatorio, se intercambian los mensajes y cuando se quiere subir un archivo,
	  se le avisa al servidor y el servidor da paso al archivo para bajar o subirlo por otro canal. Esto es así ya que se
	  tienen un Firewall en el medio, que no permite el tráfico de afuera hacia adentro de la red, para que no entre un
	  mensaje de afuera, por ende para que no infecten la red interna.  Este modo lo que permite es navegar entre todos los
	  archivos del servidor y el servidor permite modificarlos de forma SEGURA.  La configuración es del lado del servidor.

	- FTP pasivo: Cuando se abre el Fireawll en el puerto 21 y en el otro canal que es aleatorio. Por eso no se usa

Ahora se necesita TSL para realizar una conexión segura para poder transferir archivos.

Correo Electrónico:
------------------
Protocolo para el envío de correo: SMTP, el cual consta de tres componentes:
	- Agente usuario: sirve para descargar los correos del servidor (tipo el outlook, el thunderbird).
	- Servidor de Correo
	- Cliente del correo

Los servidores de correo sólo se comunican entre SMTP. No utilizan otros ya que, ellos mismos no se preguntan si tienen ellos
mismos correo, entre ellos sólo se envían correos, por ende el protocolo SMTP. Los agentes usan SMPT y otros para descargar los
correos.
Tres fases de transferencias:
	- Saludo (handshaking)
	- Transferencia de correo
	- Cierre (creo)

Cuando se quiere ver el mail desde web se utiliza el protocolo HTTP, pero sólo entre los servidores utilizan en SMTP. ¡ojo con
esto!.  Protocolos de acceso de correo siempre lo usan los agentes de correo, solo ellos (outlook, thunderbird, etc). IMAP o POP3
son los protocolos para poder descargar los correos:
	- POP3: las configuraciones no se guardan (modificaciones tales como crear carpetas, poner etiquetas, etc). Fue el primero
	  que se implementó, era bastante básico.
	- IMAP: las configuraciones se guardan, todo lo que se hace en el agente de correo, se guarda dentro de los servidores de
	  correo, las configuraciones son permanentes en el servidor. Por lo que la configuración sobre los distintos correos que
	  se realicen en el Cliente de Correo, será también modificado en el servidor.

Hay que recordar que cuando se envía un correo desde el agente hasta otro cliente, se utiliza el protocolo SMTP.

Con telnet se puede probar los puertos de determinada dirección IP o dirección URL.
En las filminas puede verse cómo interactúan mediante mensajes entre el servidor y el cliente de correo. En las filminas [26] se
visualizan las características de cada uno de los protocoloes de acceso de correo.

El servidor de correo sólo usa SMTP, y son los agentes los que usan POP3 o IMAP para consultar y descargar los mails.
A todo lo que se le agrega por el protocolo TLS, para darle mayor seguridad, encriptarlo de punto a punto.

Los nuevos puertos que se usan por el tema de seguridad para correo (los cuales implementan TLS):
	- 465
	- 993
	- 995 POP3

Al ver la conexión en el navegador, cuando está cacheada la dirección, le manda un condicional IF cuando hace el Request. Si la
página se modificó luego de la fecha que está guardada en caché, entonces devuelve la más reciente, en caso contrario, muestra la
que tiene guardada.
