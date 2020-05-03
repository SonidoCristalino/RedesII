
# Capas TCP y UPD
	La información se obtiene de este [video](https://www.youtube.com/watch?time_continue=1&v=4YCqius0CiQ&feature=emb_logo)

Esta capa se centra en la comunicación entre procesos que no son más que programas que se ejecutan en una máquina. Dentro de un
mismo terminal o _host_ habrán muchos procesos corriendo y muchos se comunicarán entre ellos.
Los que serán el objeto de estudio serán los programas que se encuentran en *distintos* host cuyos mensajes se envían a través de
la red.
Los procesos se pueden clasificar en:
	- Procesos cliente: Es el proceso que inicia la comunicación.
	- Proceso servidor: Es el proceso que espera hasta que alguien le conteste.
	En una red peer to peer también se tendrá procesos cliente y procesos servidores. En cada Peer.

Los procesos enviaran los mensajes desde la capa de aplicación a la capa de transporte mediante una API que se denomina Socket, el
cual depende del tipo de protocolo que se vaya a emplear (TCP o UPD), se utiliza un socket u otro.
El desarrollador de aplicaciones controla totalmente el proceso, pero a la hora de decidir cómo se enviará el mensaje a la capa de
transporte, si será TCP o UDP, esta capa le deja configurar unos pocos parámetros, como el tamaño del buffer o del segmento.

Definición de *socket*: Un socket (enchufe), es un método para la comunicación entre un programa del cliente y un programa del
servidor en una red. Un socket se define como el punto final en una conexión. Los sockets se crean y se utilizan con un sistema de
peticiones o de llamadas de función a veces llamados interfaz de programación de aplicación de sockets (API, application
programming interface).  Un socket es también una dirección de Internet, combinando una dirección IP (la dirección numérica única
de cuatro partes que identifica a un ordenador particular en Internet) y un número de puerto (el número que identifica una
aplicación de Internet particular, como FTP, Gopher, o WWW).

Definición de protocolo de red:
	Definie el tipo de mensaje que se intercambia, especificando su formato, tanto la sintaxis como la semántica, su orden y
	las acciones que se realizarán.
	Estos protocolos pueden ser públicos, que permiten la interoperabilidad y se publican en RFCs; o los propietarios, que son
	los que utilizan las empresas como Skype o spotify.

Bajo la capa de aplicación, está la de transporte, que ofrece a las aplicaciones determinados servicios:
	- Transferencia fiable: aplicaciones tolerantes a fallos; aplicaciones que requieran transferencia de datos 100% (por
	  ejemplo transferencia de archivos) Es decir, se asegura que los mensajes llegan al destinatario, sin errores, completos
	  y ordenados.
	- Temporización (timming): aplicaciones que requieren bajo retardo para ser efectivas( por ejemplo telefonía por internet,
	  juegos interactivos).
	- Tasa de transferencia: Aplicaciones sensibles al ancho de banda: piden una tasa de transferencia mínima garantizada (por
	  ejemplo los paquetes multimedia o VoIP); aplicaciones elásticas: usan la tasa de transferencia disponible.
	  Son las aplicaciones que requieren enviar grandes volúmenes de datos rápidamente.
	- Seguridad: Confidencialidad integridad de datos, autenticación...

	Todos los servicios descriptos anteriormente NO están contemplados en los protocolos como TCP y UDP, por lo que se tendrán
	que poner en capas intermedias o en las propias aplicaciones.

¿Qué servicios ofrece TCP/UPD?
	* TCP: Es un protocolo orientado a conexión, por lo que antes de enviar datos, se establece conexión en ambos sentidos y
	  full duplex, entre cliente y servidor. La transferencia será de forma fiable, entre el proceso de emisión y el de
	  recepción, sin errores, pérdida o duplicación y en el orden correcto.
	  Además tendrá el cuidado de no inundar al receptor con más datos que le quepan, mediante un control de flujo. Y regulará
	  al emisor cuando la red se encuentre saturada.
	  Lo que NO GARANTIZA:
	  	- Servicios de temporización.
		- Una mínima tasa de transferencia.
		- Ni tampoco seguridad.

	* UPD: Protocolo ligero y simple, que NO establece conexión previa, sino que directamente envía los datos, sin garantizar
	  la entrega, ni control de flujo, ni congestión, ni temporización, ni una tasa mínima de transferencia, ni seguridad.
	  ¿Entonces para qué existe? Es un protocolo tan ligero que se tiene total control de cuándo se envían los datos; y si no
	  nos importa perder algún paquete como en las aplicaciones de streaming pues es mejor que TCP.


Cada host en internet tendrá una dirección que la identifique. Como con sóla la dirección IP no es suficiente, para identificar a
la máquina y la aplicación a la que el mensaje es enviado, lo que se hace es agregarle unos números además de la dirección IP que
permita identificar esas aplicaciones, que son los denominados números de puerto.
	Por lo tanto, por ejemplo un servidor web, puede tener la dirección IP: 5.5.5.5 : 80 donde éste último número (el 80)
	le indicará por dónde comunicarse con otras aplicaciones.
	Por ejemplo si se tiene en un host A (cliente) un navegador web y una dirección, se puede formar de la siguiente manera:
	1.2.3.4 : 25000 y se tiene un host B (servidor) con un servidor web apache que se conecta por el puerto 80, entonces de
	esta forma, el navegador de la primer máquina sabrá que se podrá comunicar con la máquina servidor mediante su puerto 80.

# Capa de aplicación
	La información se obtiene de este [video](https://www.youtube.com/watch?v=HVWjeP1cQrM&feature=emb_logo)

Arquitectura de Aplicaciones de red:
	Fundamentalmente hay dos:
		- Cliente - servidor
		- Peer to peer
		- También existen las híbridas cuando se emplean rasgos de una y de la otra.
		- Cloud: es como el cliente servidor, pero con el servidor en la nube.

Las principales características de un *servidor* en una arquitectura Cliente-Servidor, son:
	- Estará siempre activo, normalmente esperando conexiones de los clientes
	- Tendrá una IP fija, permanente y conocida a la que se podrán dirigir los clientes para solicitar los servicios que
	  ofrezca ese servidor.
	* El principal problema que tienen los servidores es el escalado, según la demanda que se tenga de sus servicios.

Las principales características de un *cliente* en una arquitectura Cliente-Servidor, son:
	- Se conectan de forma intermitente con el servidor
	- Pueden cambiar su dirección IP usando direccionamiento dinámico.
	- Los clientes NO se comunican directamente entre si.


Arquitectura *peer to peer* (arquitectura entre pares):
	- host pueden actuar como clientes o servidores de forma simultánea.
	- Por esto, los servidores NO siempre estarán activos.
	- Los host clientes se podrán comunicar entre sí, pudiendo también cambiar su dirección IP.
	- Esta arquitectura es altamente escalable pero muy difíciles de gestionar.
	* Uno de los inconvenientes que tiene esta arquitectura es que las infraestructuras de red que han desplegado los ISP
	  están orientadas a arquitecturas cliente servidor donde es mayor el tráfico de bajada que el de subida (Aunque el uso de
	  fibra óptica está re-dimensionando las líneas, haciendo un uso simétrico del ancho de banda).
	* Otro de los problemas es la seguridad, dada la naturaleza extremadamente distribuida y abierta, es como poner puertas al
	  campo.
	* Con estas arquitecturas se han de crear políticas de incentivos para convencer a los usuarios y que ofrezcan
	  voluntariamente ancho de banda, almacenamiento y computación.

# Skype
Hay arquitecturas p2p donde no todos los host son iguales, por lo que no son p2p puras. Son híbridas.
	- Se tienen pares comunes (peers)
	- Pero también se tienen super-peers por lo que se genera una jerarquía en la red.
	- Se tiene un servidor centralizado de login, pero el índice que asigna los nombres de usuario a las direcciones IP está
	  distribuido entre los super-peers, probablemente con una tabla hash distribuida.
	* Tiene un protocolo privado por lo que no se sabe a ciencia cierta cómo trabaja.


# Protocolo HTTP
	La información se obtiene de este [video](https://www.youtube.com/watch?time_continue=1&v=m70H0wC6JRQ&feature=emb_logo)

Se debe tener en cuenta lo siguiente:
	Arquitectura de Aplicaciones de red:
		Fundamentalmente hay dos:
			- Cliente - servidor
			- Peer to peer
			- También existen las híbridas cuando se emplean rasgos de una y de la otra.
			- Cloud: es como el cliente servidor, pero con el servidor en la nube.

El cliente realiza una petición para un fichero de tipo _http://www.algo.edu.ar/dcc/index.html_ . Las partes son:
	- Https:// : es parte del protocolo
	- www.algo.edu.ar Es parte del host donde se encuentra el fichero.
	- /dcc/index.html es la ruta hasta donde se tiene que dirigir el servidor para encontrar el fichero a transferir.

El servidor le responde devolviendo el fichero solicitado. El navegador mira el fichero HTML que ha recibido y detecta que
necesita otros ficheros para poder visualizarlo de forma correcta (como la hoja de estilo CSS, alguna imagen, algún applet de
java, fichero de música) por lo que antes de visualizarlo, solicita estos, que pueden estar hospedado en el mismo servidor o en
otros.
Una vez que lo tiene todo, el navegador lo muestra al usuario.

La web se basa por tanto en el protocolo http que sigue un modelo cliente-servidor. Donde el cliente es el navegador y el servidor
es el servidor web (como por ejemplo Apache). El protocolo que se utiliza es el *TCP* por lo que el cliente antes de realizar la
trnasferencia de datos, debe establecer una conexión TCP, creando el socket con el servidor web, que tiene su socket esperando en
el puerto 80. Acepta la conexión, y una vez establecida la conexión se intercambian los mensajes http y finalmente se cierra la
conexión.
Http es un protocolo sin memoria, por lo que el servidor no mantiene un historial de las peticiones realizadas del mismo cliente.

Tipos de conexiones HTTP:
	- Las conexiones no persistentes: donde se cierra la conexión TCP luego de haber transferido cada objeto.
	- Las conexiones persistentes: en las que se pueden enviar varios objetos antes de cerrar la conexión.

Round trip time: tiempo que tarda un paquete en ir del cliente al servidor y volver, sin tener en cuenta su tamaño.
En una conexión TCP normal, el cliente envia una solicitud de conexión, y cuando el servidor le contesta que si, estableciendo una
conexión, ese es el tiempo RTT.
Se necesitarán 2 RTT por cada objeto o página HTTP que se requiera del servidor:
	- Un tiempo entre que se envía una petición de conexión y el servidor devuelve que si
	- Otro tiempo en enviar una petición del objeto que se requiere y el servidor lo envía. Luego de ello como es una
	  *conexión NO persistente* se cierra la conexión, por lo que si se quiere pedir OTRO objeto, se deberá de hacer todos los
	  pasos de nuevo.

	* Otra forma de encausar esto, sería pedir por ejemplo 3 conexiones con el servidor, pero esto multiplicaría su
	  rendimiento, porque si por cada objeto solicitado se tienen 2 RTT, por 3 conexiones se tendrían 6 RTT.

Para evitar este problema al cerrar la conexión luego de enviado el archivo y con esto generar empobrecimiento en la transferencia
de múltiples objetos, se utilizan *conexiones persistentes*.
Lo que se hace es mantener una conexión abierta entre cliente y servidor aún luego de haber transferido el objeto solicitado por
el primero, así en caso de que se soliciten más objetos, se envián por la misma conexión abierta que se tiene.
Por lo que el cliente emplea un solo RTT para abrir la conexión y luego un RTT por cada objeto que sea transferido.

Lo que se emplea en éste último tiempo es lo que se denomina *HTTP persistente concurrente* : donde el cliente abre de forma
concurrente (al mismo tiempo) varias conexiones con el MISMO servidor para realizar la transferencia de objetos de forma
simultánea y de forma más ágil.

Otra mejora es la denominada _pipelining_ que consta de NO esperar a recibir el objeto completo para pedir el siguiente, sino
hacer varias peticiones, simultáneas al servidor y que este entregue los objetos según los vaya enviando, o recibiendo las
solicitudes de objetos. Esto puede llevar a problemas en algunos navegadores, porque algunos objetos pueden bloquear a otros
objetos más importantes. Y es difícil implementarlo correctamente para que la mejora sea apreciable, esto hace que todos los
navegadores tengan deshabilitado el _pipelining_ por defecto. Se emplea un algoritmo mejor que se denomina *multiplexación*.


# Protocolo HTTP (continuación):
	La información se obtiene de este [video](https://www.youtube.com/watch?v=YcRfqNhSA5M&feature=emb_logo)

Los mensajes HTTP pueden ser de dos tipos. Ambos en formato ASCII (pueden leerlo las personas):
	- Solicitud: Tiene una primera línea donde va primero un comando (GET por ejemplo), espacio una URL sobre la que actúa el
	  comando y la versión en la que está el mensaje.
	  En las siguientes líneas se establecen ciertos parámetros con el formato:
	  	_[Nombre del parámetro] : [Valor]_
		* Host: www.unaj.edu.ar 	- nombre del host donde se solicta al objeto.
		* User-agent: mozilla/4.0 	- nombre del cliente que lo solicita.
		* Conection: close 		- el tipo de conexión
		* Accept-language: es-ES 	- el idioma en el que se pide el objeto. En caso de que no lo encuentre, se devuelve el
		  objeto en el lenguaje que se encuentre por default.
		* [Linea en blanco]
		* Cuerpo del mensaje: 		- si es necesario enviar información al servidor (en caso de que sea un PUT por
		  ejemplo) la información se sitúa en este espacio.

		Los métodos disponibles son:
			* GET: el navegador pide el objeto indicado en la URL. Cuerpo vacío
			* POST: el navegador pide el objeto, pero el contenido depende de lo escrito en un formulario /datos en el
			  cuerpo de la entidad)
			* HEAD: es similar a GET pero no envía el objeto solicitado (se utiliza para depuración).
			* PUT: Carga el objeto en una ruta específica
			* DELETE: borra el objeto de un servidor web.

	- Respuesta:
	  El mensaje de respuesta desde el servidir tiene un formato similar.
	  	_[Nombre del parámetro] : [Valor]_
		* HTTP/1.1 200 OK		- Primer linea de estado donde se indica la versión del protocolo del mensaje,
		  después un código numérico del estado[404, 400, 505, etc), seguido de un texto explicativo.
		* Conection: close 		- parámetro de la conexión
		* date: [Fecha] 		- fecha del paquete
		* server: apache		- servidor que se está utilizando.
		* Last-modified: [fecha]	- última modificación del objeto
		* Content-lenght: [numero]	- longitud del objeto enviado
		* Content-type: [tipo]		- tipo del objeto que se envía.
		* [Linea en blanco]
		* Cuerpo del mensaje		- Información que se adjunta perteneciente al mensaje.

A lo largo del tiempo se han ido incrementando la cantidad de datos transferidos en las páginas como también el número de
peticiones que se realizan al servidor. Por lo generó una búsqueda constante de nuevas formas para agilizar esto.

# 4 problemas en HTTP 1.1
	* Los 3 primeros son producto de las cabeceras que incluye el protocolo, ya que son muy pesadas, redundantes y no se
	  pueden comprimir.
	* Bloqueo HOL (Head of Line): El problema surge cuando se quiera realizar _pipelining_ para mejorar la eficiencia. Es que
	  si el servidor tarda en atender una petición de un objeto, esto retrasará a todos los que están esperando ser atendido
	  en la cola de espera.

	Posibles soluciones:
		- Por un lado se fueron realizando distintos parches para solucionar los problemas anteriores, realizando:
		  	* Conexiones concurrentes
			* Domain sharding
			* Usando CDNs (que es realizar réplicas y que el cliente obtenga una replica más cercana los objetos).

		- Por otro lado, se fue tratando de que se reduzca el número de solicitudes al servidor, realizando:
		  	* Concatenando archivos
			* Enviando imágenes en sprite sheets
			* Resource inlining: Introduciendo scripts en el propio código HTML.

# HTTP 2.0
Algunas mejoras fueron:
	- Emplear UNA conexión TCP
	- Las peticiones se convierten en flujos (streams).
	- Los streams se pueden multiplexar y priorizar.
	- Los mensajes en binario (ya no más en ASCII) y de longitud fija.
	- Mensajes de control (cabeceras) separados de los mnesajes de datos.
	- Compresión de las cabeceras-
	- Server push.
	- Normalmente sobre TLS (cifrando los paquetes).

Todo esto hace que este protocolo sea soportado ya por la mayoría de navegadores web. Google de forma experimental, está tratando
de dejar de lado a TCP con TLS implementando un protocolo QUIC (que ofrece servicios de cifrado, confiabilidad y control de
congestión) , es decir es todo lo que ofrece TCP con TLS pero utilizando UDP.

# Cookies y Caché
	La información se obtiene de este [video](https://www.youtube.com/watch?v=svzqZrzZP-I&feature=emb_logo)

HTTP es un protocolo SIN memoria, por lo que si se quiere tener un control del historial de sus usuarios, hay que pedir que se
registren y tener todas sus peticiones y transferencias en una base de datos y leerlas cada vez que ingresen a la página.
El uso de *COOKIES* evita este procedimiento. Almancenando en el navegador del lado del cliente, determinados datos.

Una cookie consiste en:
	- Un par nombre-valor con datos almacenados.
	- Una fecha de expiración a partir de la cual la cookie será inválida y eliminada.
	- El dominio y el directorio del servidor a cual se enviará dicho dato.

El uso general de la cookie es:
	- Cuando se quiere conectar por primera vez desde un navegador web a una página, por ejemplo amazon, se envía un típico
	  mensaje HTTP de solicitud.
	- El Servidor de Amazón nos contestará el mensaje de petición enviado PERO con una solicitud de cookies que se guardará en
	  nuestro navegador, de la siguiente forma: set-cookie: 1234.
	  Por otro lado, el servidor de Amazon, generará en su base de dato la entrada 1234 para guardar información sobre mis
	  conexiones.
	- Cada vez que nos conectemos a Amazon, lo que hará nuestro navegador es enviarle la cookie almacenada con el número 1234
	  para que el servidor pueda buscar en su base datos toda la información nuestra y así poder tener disponibles todo el
	  historial de navegación.

El uso de las cookies en los servidores es muy variada:
	- Para gestionar sesiones y facilitar el login para que el usuario no necesite ingresar usuario y contraseña cada vez que
	  se conecte.
	- Mantener carrito de la compra.
	- Ayudan a la personalización del sitio, almacenando como el color, idioma o tamaño de letra favorito.

Privacidad:
	- Se puede conocer información personal: nombre, email, etc.
	- Pueden guardar información a lo largo de internet, como por ejemplo los anuncios que se muestran.
	- Se puede hacer un seguimiento del historial de la navegación del usuario.
	- Muchas empresas venden sus cookies a terceros, eso generó una controversia por lo que en la Unión Europea se decretaron
	  leyes que atentan contra el desconocimiento de los navegantes en páginas que guardan cookies.

Caches Webs (servidores proxy):
	Otras mejoras que se implementan para la mejora en la navegación.
	La idea es que haya un servidor intermediario, gestionando las peticiones que los clientes hacen hacia los servidores,
	para que en caso de que haya una segunda solicitud a una misma página ya visitada, no tener que remitir la solicitud al
	servidor original, sino que sea el servidor proxy quien conteste primero esa petición, cuya página estará guardad en su
	memoria caché.
	¡No es la caché del navegador!
	El servidor proxy será por el que pasarán todas las peticiones de un cliente, si tiene lo solicitado, este se lo
	devolverá, en caso contrario, redirigirá la petición al Servidor original y cuando el objeto pase por el servidor proxy,
	se guardará en él en caso de que se vuelva a consultar.

	Por lo tanto el Servidor proxy actuará como servidor para mi navegador web, como de cliente para otros servidores webs.
	Estos son instalados por el ISP para acelerar la navegación y evitar tráfico innecesario.

Ventajas:
	- Navegación más rápida.
	- Reducción del tráfico.
	- Control de tráfico.
	- Navegación anónima.
	- Navegación segura.
	- Navegación como si se se estuviera navegando desde un país determinado.

Para actualizar el contenido de los servidores proxy se utiliza el comando GET condicional, mediante el cual se puede introducir
en la cabecera, ciertas condiciones para que se traiga el objeto del servidor sólo en casode que se cumplan.
Por ejemplo la cláusula: _if-modified-since_ es decir, si ha cambiado desde la última fecha que tiene constancia. Esta fecha se
obtuvo en el proxy cuando le llegó el objeto por última vez en su campo last-modified.
En este caso, si el servidor proxy tiene el objeto actualizado el servidor le devolverá un 304 not-modified evitando enviar un
objeto de nuevo. En caso contrario, si el objeto ha cambiado, le devolverá un 200 OK con el objeto solicitado.

