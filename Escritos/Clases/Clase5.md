# Clase nº 5 : Sockets
	La información detallada a continuación se obtiene de: https://www.youtube.com/watch?time_continue=1&v=v20amqH9mqs&feature=emb_logo

# Sockets UDP
Sockets, son interfaces por los cuales los procesos envían o reciben mensajes a o desde otro proceso. Cuando se construye un
socket hay que especificar si la conexión será mediante el protocolo de transporte TCP o UDP.

La aplicación que utilizará el sockets, deberá de utilizar la API de sockets para enviar y recibir mensajes.

El servidor estará ejecutando el proceso que tendrá un socket para recibir y enviar mensajes por un determinado puerto. Este
puerto junto con la dirección IP del servidor los debe conocer el cliente para poder enviarle mensajes desde su propio socket.

El servidor debe estar ejecutando el proceso antes de que el cliente envíe algo. Este también debe tener un socket a través del
cuel recice y envía segmentos. Por su parte, el cliente necesita conocer la IP y el puerto del servidor.  El socket se identifica
localmente con un puerto.
Los lenguajes comúnmente utilizados son:
	- C.
	- C++.
	- Java.
	- Python.

Un flujo o stream es una cadena de caracteres que entran (flujo de entrada) por ejemplo desde el teclado; o salen (un flujo de
salida) por ejemplo al monitor desde un proceso.
	- Un flujo de entrada está asociado a una fuente de entrada para el proceso, por ejemplo un socket o un teclado.
	- Un flujo de salida está asociado con una fuente de salida, por ejemplo un socket o monitor.

Hay que recordar que en UDP no hay conexión entre cliente y servidor, por lo que:
	- No hay una fase inicial de negociación.
	- El emisor explícitamente asocia la IP y el puerto de destino a cada paquete. El servidor recibirá un datagrama UDP
	  (llamado también "paquete UDP") del que sacará la IP y el puerto del emisor para poder contestarle.
	- Modelo de servicio no fiable en el que se hace el máximo esfuerzo por suministrar el lote de bits al destino.
	- El servidor extrae la IP y el puerto del emisor del datagrama recibido.

## Programación en JAVA UDP:
	Las clases a utilizar para poder programar el socket serán:
		- InputStream y Outputstream: Flujos de entrada y salida.
		- Reader y Writer: Flujos de caracteres UTF-16 de entrada o salida.
		- InputStreamReader: Transforma un flujo de bytes (InputStream) en un flujo de caracteres (Reader).
		- BufferedReader: Lee caracteres de un flujo de caracteres (Reader) y los almanecena hasta forma una línea (cuando
		  detecta el fin de línea).
		- DatagramSocket: Socket UDP para la comunicación entre dos máquinas.
		- DatagramPacket: Para contruir el paquete UDP, con los datos a enviar, y la dirección IP y puerto destino.

[No se trascribe la explicación en programación JAVA]

Observaciones:
	- Tanto el cliente como el servidor usan DatagramSocket.
	- La IP destino y el puerto se introducen explícitamente en el paquete.
	- ¿Puede el cliente enviar un segmento al servidor sin conocer su IP y/puerto?
	- ¿Pueden n múltiples clientes usar el servidor?


# Sockets TCP
	La información detallada a continuación se obtiene de: https://www.youtube.com/watch?v=0i3hmXEkLRU&feature=emb_logo

Se debe recordar que en las comunicaciones bajo TCP, es el cliente el que debe iniciar el contacto con el servidor, quien debe
estar ejecutando un socket esperando ser contactado.
	- El proceso servidor debe estar ejecutándose.
	- El servidor debe tener un socket que permita el contracto de acogida.

El cliente contacta al servidor:
	- Creando un Socket TCP
	- Especificando la IP y el perto del proceso servidor.
	- Acuerdo en 3 fases: transparente para procesos clientes y servidor.
	- Cuando el cliente se contacta con el servidor, se crea un socket TCP para comunicarse con el cliente. Esto permite al
	  servidor comunicarse con varios clientes, porque utilizarán distintos sockets.
	- Una vez establecida la conexión, ambos disponen de una tubería donde podrán transferirse datos para leer y escribir.
	- Esta transferencia es fiable y se garantiza que llega en orden.

## Programación en JAVA TCP:
	Las clases a utilizar para poder programar el socket serán:
		- InputStream y Outputstream: Flujos de entrada y salida.
		- Reader y Writer: Flujos de caracteres UTF-16 de entrada o salida.
		- Socket: punto final de la comunicación entre dos máquinas. Tiene métodos que devuelven InputStream y un
		  OutputStream.
		- ServerSocket: genera un socket por cada petición de conexión que acepta. Usada en los servidores para aceptar
		  múltiples conexiones.
		- DataOutPutStream: para convertir tipos de datos primitivos de Java(char, int, etc) en un flujo de bytes de
		  salida.
		- InputStreamReader: Transforma un flujo de bytes (InputStream) en un flujo de caracteres (Reader).
		- BufferedReader: Lee caracteres de un flujo de caracteres (Reader) y los almanecena hasta forma una línea (cuando
		  detecta el fin de línea).
