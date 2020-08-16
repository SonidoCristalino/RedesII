# Clase nº 6:
	Todo lo escrito acá, se obtiene de: https://www.youtube.com/watch?v=8VNjxo7QRBw&feature=youtu.be

# Servicios que ofrece la Capa de Transporte:

La capa de transporte se encarga de la Comunicación Lógica entre procesos de aplicación que se ejecutan en diferentes host, de
forma que parezcan que estos hosts, estuviesen conectado directamente entre sí.

Los protocolos de transporte se ejecutan en sistemas terminales, y NO en el núcleo de la red. Por lo tanto, cuando el emisor
requiera enviar un mensaje, la aplicación pasará el mensaje a la capa de transporte, el cual es dividido en segmentos y los pasa a
la Capa de Red. Una vez encapsulado el mensaje con sus correspondientes cabeceras de transporte, de red y enlace, este se
transmite por la Red hasta el destino.

En el Receptor, es la la capa de transporte la encargada de recoger los distintos segmentos en los que se dividió el mensaje en el
origen, también el de recomponer los segmentos del mensaje y pasarlos a la capa de aplicación.

Los protocolos más habituales de transporte en Internet pueden ser: TCP y UDP.

## Protocolo TCP:
	* Es un protocolo orientado a la conexión, es decir, antes de enviar algún dato entre los procesos, se establece una
	  conexión full duplex, es decir, en ambos sentidos, entre las dos máquinas.
	* Garantiza que la entrega sea fiable, es decir, sin errores ni pérdidas ni duplicados, y en el orden correcto.
	* También controla el flujo, que implica que el emisor NO inunde al receptor.
	* También controla la congestión de la red, regulando la velocidad de emisión cuando la red se encuentra saturada.
	* NO proporciona:
	  	- Mecanismos de temporización.
		- No garantiza una tasa de transferencia mínima.
		- Tampoco garantiza seguridad alguna.

## Protocolo UDP
	* Es un protocolo ligero y simple.
	* No precisa establecer conexión previa entre procesos.
	* Entrega entre procesos no fiable y sin orden.
	* Sólo tiene un control de errores MUY básico.
	* No proporciona:
	  	- Control de flujo.
		- Control de Congestión.
		- Temporización.
		- Tasa de transferencia mínima.
		- Seguridad.
	¿Para qué existe entonces? Es un protocolo muy simple que unicamente toma el mensaje que le provee la aplicación, le
	añade una cabecera con los puertos de origen y destino y se lo pasa a la capa de red para su envío inmediato.
	La ventaja que tiene es que al ser tan ligero, las aplicaciones multimedia en tiempo real funcionan mejor con UDP, que con
	la regulación de velocidad que introduce TCP en su mecanismo de control de congestión.


Se puede apreciar que NI TCP NI UDP garantizan:
	- Temporización
	- Ni tasa de transferencia mínima.
	- Ni tampoco seguridad.

Para cubrir los aspectos antes mencionados, se realizan diseños inteligentes, que en internet NO se pueden garantizar.

Mientras la capa de aplicación, se encargaba de la comunicación lógica entre aplicaciones, la capa de transporte se va a encargar
entre la comunicación lógica entre procesos. ¿Qué tiene por debajo? La Capa de Red.

	* La Capa de Red, ofrece sus servicios a la Capa de Transporte que se encarga de la comunicación lógica entre host. Por lo
	  tanto es la encargada de comunicar dos máquinas (se identifican las máquinas mediante una dirección IP).
	* La Capa de Transporte, se encarga de comunicar LOS PROCESOS de esas máquinas (se identifican los procesos mediante un
	  número de puerto).
	* Y la Capa de Aplicación comunica las aplicaciones que se ejecutan en esos procesos.

La comunicación que ofrece la Capa de Red, en Internet es mediante el protocolo IP. La misma, ofrece:
	- Servicio de entrega de mejor esfuerzo, es decir,
	- No fiable, por lo que la Capa de Red NO garantiza ni la entrega, ni el orden, ni la integridad de los segmentos que le
	  pasa a la Capa de Transporte para ser enviados.

Si se quieren garantizar lo anterior, se debe emplear, el protocolo TCP, en la Capa de Transporte.

En la Capa de Red identificaremos a cada Host mediante una dirección IP (al igual que en la de Transporte se identificaba cada
proceso con un número de puerto).

# Segunda parte:
	Todo lo escrito acá, se obtiene de: https://www.youtube.com/watch?v=Nl5TJApXZcA&feature=youtu.be


TCP y UDP, amplían por tanto el servicio de entrega que ofrece IP entre dos terminales, y los amplía realizando un servicio de
entrega entre los procesos de esos terminales. Para ello se emplea la Multiplexación y la Demultiplexación.

## Demultiplexación
Se entiende el proceso por el que el emisor recopia datos de diferentes sockets, les añade distintas cabeceras y crea los
segmentos que pasa a la Capa de Red para su envío. Pero el proceso más interesante es el de demultiplexación, ya que es en el
destino donde la Capa de Transporte que recibe todos los datos de la Capa de Red debe decidir por qué socket entrega cada
segmento.

## Ejemplo:
	Tenemos 3 máquinas, A, B y C. B es un servidor, que tiene tanto un proceso HTTP como un FTP.
	Si A quiere obtener la página web que se hospeda en B, entonces se envía un mensaje de petición de A hacia B, pasando por
	las siguientes capas:
		- Aplicación (desde donde se solicita la página web)
		- Transporte
		- Red
		- Enlace
		- Física
	El servidor B le llega esta solicitud por y escala en la pila de capas:
		- Física
		- Enlace
		- Red
		- Transporte: es esta capa la que deberá decidir qué proceso se debe entregar el mensaje, que será el puerto 80
		  para HTTP.

	En cambio, si la máquina C, solicita un fichero del servidor FTP que también está ejecutando B, entonces envía un mensaje
	de solicitud pasando mediante las siguientes capas:
		- Aplicación (desde donde se solicita la página web)
		- Transporte
		- Red
		- Enlace
		- Física

	El servidor B le llega esta solicitud por y escala en la pila de capas:
		- Física
		- Enlace
		- Red
		- Transporte: es esta capa la que deberá decidir qué proceso se debe entregar el mensaje, el cual entregará al
		  socket que abrió el puerto 20,el cual es el indicado para este tipo de transferencias.

Hay que recordar que el número de puerto es un número de 16 bits, por lo que podrá tener valores entre 0 y 65535, aunque los
primeros 1024 puertos están reservados. Como por ejemplo:
	* 20/21 : FTP
	* 22 : SSH
	* 25 : SMTP
	* 53 : DNS
	* 80 : HTTP

## ¿Cómo decide la Capa de Transporte a qué socket envía cada segmento que recibe?
En el host destino, la capa de Transporte, recibe un datagrama IP. Este datagrama contendrá en su cabecera, los datos de IP
origen (la del host emisor), la IP destino (la del host destino). Dento de ese datagrama va el segmento de transporte que se
entregó al socket.
	* Cada datagrama tiene una IP de origen y una IP de destino.
	* Cada datagrama lleva un segmento de la capa de transporte.
	* Cada segmento tiene un número de PUERTO de origen y de destino.
	* El segmento de la capa de transporte (TCP/UDP) por lo tanto tiene:
	  	- Número de origen y destino.
		- Otros campos de cabecera.
		- Datos de la aplicación (mensaje).

El host utiliza las direcciones IP y los números de puerto para entregar el segmento al socket adecuado.

## Demultiplexación SIN conexión (UDP):
El socket UDP vendrá identificado unicamente por la IP del host destino y con el número de puerto de proceso destino.
Por lo tanto, cuando un host recibe un segmento UDP:
	* Observará el número de puerto de destino en el segmento
	* Dirige ese segmento UDP al socket con ese número de puerto.
Esto significa que el origen de los datagramas IP y PUERTO NO importa, ya que todos iran al mismo socket de la máquina destino,si
el puerto destino, es el mismo.
La Ip y el puerto origen se utilizan únicamente para poner la dirección de retorno pero NO para identificar al socket.
La Ip y el puerto origen unicamente se utilizan para crear la dirección de retorno de los segmentos, pero NO para decidir a qué
socket se entrega en destino al demultiplexar.

Si por ejemplo se tiene un servidor que atiende en el puerto 53 (reservado para) peticiones DNS, entonces se tendrán muchas
peticiones para un solo socket, por lo que se deberá emplear un Buffer UDP, para poner en cola todas las peticiones que vayan
llegando a ese puerto, para que ese proceso las vaya atendiendo en orden.

## Demultiplexación orientada a la conexión (TCP):

En estos casos el socket TCP vendrá identificado no sólo por la IP y Puerto destino, sino también por la IP y puerto ORIGEN, de
forma que en un servidor se podrán tener varios sockets en el mismo puerto. Tantos como procesos cliente de otras máquinas, estén
conectadas a ese puerto.

Es decir, que al contrario de lo que sucede en UDP, dos segmentos entrantes con distinta IP de origen, se dirigirán a sockets
diferentes que podrían estar escuchando en el mismo puerto. Por lo tanto, los servidores web tendrán diferentes sockets para cada
cliente que se conecte a ellos. Si consideramos por ejemplo, HTTP No Persistente, en ese caso el servidor utilizaría un socket
para cada petición y lo cerraría al finalizar la misma. [Ejemplo del funcionamiento en 7:28]

Los servidores actuales tiene un mismo proceso que crean un nuevo hilo (o hebra) para cada socket a cada petición que les llega.
En un servidor de estas características, puede tener varios sockets de conexión con distintos ID asociados al mismo proceso, y en
las conexiones persistentes se utiliza el mismo socket durante todo el intercambio de información; y en conexiones no persistentes
se crea un socket para cada solicitud de respuesta.
Puede existir la posibilidad que un proceso de un host solicitante, abra el mismo puerto aleatorio que otra máquina, pero esto no
es inconveniente para el servidor, dado que reconocerá a cada host no sólo por el número de puerto, sino por la dirección IP de
esos host.

## Servicio de Transporte SIN conexión con el protocolo UDP:

UDP es un protocolo de transporte sencillo y ligero, que pretende comunicar dos procesos de forma ágil por lo que no implementa
mecanismos para garantizar que los segmentos lleguen, ni que lleguen en orden. Esto se los denomina, "protocolo de mejor
esfuerzo".
Los segmentos UDP:
	* se pueden perder.
	* o entregar desordenados.

Es un protocolo sin conexión:
	* No hay una fase previa de negociación entre el emisor y el receptor UDP,sino que simplemente se pone la IP y el puerto
	  destino del receptor y se envía de forma inmediata e independiente al resto de segmentos.
	* Cada segmento UDP se trata de forma independiente al resto.

### ¿Por qué existe UDP?
	* Mejor control en la Capa de Aplicación sobre qué se envía y cuándo, en cuanto le llega un mensaje de la Capa de
	  Aplicación, UDP lo encapsula en un segmento y lo pasa a la Capa de Red.  TCP, en cambio, tiene mecanismos de congestión
	  que regula el flujo de envío o el reenvío de segmentos perdidos que enlentecen la emisión.
	* Sin establecimiento de conexión (retardos), en UDP directamente se comienza a transmitir, sin el retardo propio de TCP.
	* Sin información del estado de conexión (servidor soporta más clientes activos): Los servidores son más ligeros y
	  soportan más clientes activos, ya que en TCP se deben mantener parámetros de congestión, números de secuencia, de
	  reconocimiento, etc.
	* Poca sobrecarga debida a la cabecera de los paquetes, las cabeceras de UDP son muy pequeñas, de 8 bytes, frente a los 20
	  de TCP, lo que introduce menor sobrecarga en la red.

UDP se suele utilizar en aplicaciones multimedia de streaming. En las que la pérdida de algún paquete no son muy importantes, pero
si son sensibles a mecanismos de control de congestión o a la temporización.
También se utiliza en las consultas DNS, SNMP para la administración de redes, o en RIP el cual es un protocolo de enrutamiento.

Incluso se puede implementar una transferencia fiable sobre UDP, añadiendo fiabilidad a nivel de aplicación, siendo la capa de
aplicación la que deba recuperar los errores causados.

El paquete de UDP consta de las siguientes partes:
	- Campo para el Puerto origen
	- Campo para el Puerto destino
	- Campo de Longitud en bytes de todo el segmento, incluída la cabecera
	- Un campo de checksum
	- Y los datos de la aplicación (mensaje).

## Checksum:
	El objetivo de este campo es detectar errores (como por ejemplo, bit intercambiados) en los segmentos transmitidos.

	# Emisor:
		- Para calcular esto, el emisor suma todas las palabras de 16bits del segmento, y calcula el complemento a 1 de
		  esa suma.

	# Receptor:
		- El receptor cuando recibe el segmento calcula a su vez el checksum del segmento recibido y compara el que recibe
		  con el que él calcula.
		- Si no coinciden, es que hay un error.
		- De lo contrario, da por supuesto que no lo hay (aunque si podría haberlo). Para comprobar si coinciden o no,el
		  receptor puede sumarlo todo con el checksum, debe dar todos unos.
UDP no hace nada para recuperarse del error, o lo descarta o se lo dice a la aplicación para que ella decida.
