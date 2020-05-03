
# Clase de consulta : 02/05/2020

Monografía de cómo es el funcionamiento del protocolo, tipo de mensaje, etc
	- Introdución
	- Desarrollo
	- Conclusión

Programación y aplicación de red NO se ve.

Frontera de red y núcleo de red.

	- Frontera de red: Acá es donde el intercambio de mensajes de las aplicaciones de red ya que son terminales que tienen
	  este tipo de capas.
	- Núcleo de Red: switch, routers. Acá no ejecutan los dispositivos con este tipo de capa, ya que no la poseen (la capa de
	  aplicación según el esquema TCP/IP)

Cliente servidor: se tiene una máquina siempre online ip-fija (con un nombre de dominio fijo) siempre está en está arriba. El
cliente es el que se desconecta y se desconecta.

p2p: conexiones entre clientes y estos trabajan como cliente y servidores. Torrent y todos los demás son híbridos. La base de
datos (tracker) está en una máquina o máquinas determinadas, luego de saber de dónde obtener esto
p2p es cuando la base de datos está totalmente distribuida entre los clientes.
En cambio en otras aplicaciones lo que hace es un híbrido, el tracker está en un servidor que se usa una sola vez para obtener la
dirección IP a conectarse para comenzar a consumir el archivo que se quiere. Se usa una sola vez, en el p2p original TODOS tienen
el tracker.

Arquitectura Cliente-Servidor:
	- Es complicado porque a mayor cantidad de clientes se complica que responda a todos el servidor.

Arquitectura p2p:
	- Se comunican intermitentemente y cambian las direcciones ip.
	- Es altamente escalable, es jodido administrarlo porque son todos iguales.

Procesos tiene dos tipos de procesos (cliente y servidor) corriendo al mismo tiempo.

Socket: conexión entre dos procesos que se encuentran entre distintas máquinas. Se arman depende del tipo de conexión que se
emplee.

# TCP y UPD:
	Direccionamiento de procesos:
		- Dirección IP (para identificar el host)
		- Número de puerto (para identificar la aplicación que está corriendo). Ejemplo: Servidor HTTP: puerto 80

	Protocolos se definen en los documentos RFCs donde se explica cómo tiene que funcionar el protocolo.

¿Qué servicio necesita la capa de transporte necesita la aplicación? (es

	- confiabilidad en la entrega: el paquete llegue, que si se manda (lo ofrece TCP).
	- Retardo: algunas necesiten bajo retardo para ser (esto no puede ser ofrecido por TCP) porque lo gestiona como quiere la
	  propia ISP.

[En la filmina están los requerimientos que se necesitan en algunas aplicaciones ]


Lo que se ven fuerte son los dos tipos de servicio TCP y UDP.
	[Bien explicado en la filmina]

	UDP es más rápido para enviar rápido un paquete, se utiliza por eso. Porque no abre una conexión de forma previa.

Una página web está compuesto por objetos [en la filmina 19 está la forma en que se solicita un objeto mediante una página web].

[20]
	Cliente son los navegadores, que se usan para conectar al servidor.
[21]
	Se conecta, se envía el objeto, luego se cierra. No guarda estados. Para guardar estados, se utiliza otra cosa.
	Coockies se utilizan para guardar los estados del cliente. Almacenada en el usuario y en el cliente. Guarda un número
	asociado a lo que se hizo, entonces cuando se vuelve conectar manda ese número y el servidor sugiere otras cosas a partir
	de sa cookie.
	Cuando se conecta por primera vez el servidor genera un ID que es lo que almacena. Esta se almacena en una base de datos
	en el servidor.
[22]
	HTML es un objeto donde le dice cómo tiene que estar conformado la página, dónde tiene que ir cada objeto.

HTML: 1.1 se suman más verbos para poder manejar el servidor desde el cliente.

	- los errores del 400 son por parte del cliente
	- los errores del 500 son por parte del servidor

[Todo lo que tiene que ser cookies, están todos en las filminas.]

Webcaché es un servidor interno que va guardando las páginas que vamos visitando, entonces cuando se quiere buscar la misma web,
busca primero en el webcaché, entonces los usuarios de esa forma no saturan el ancho de banda.
Proxy es como alguien que interceda hacia afuera, entonces los de afuera no saben quien hace las consultas, los servidores van a
evita que el ataque hacia adentro.
Conexiones hacia distintos servicios que no sean web. En los ministerios se hace esto,
