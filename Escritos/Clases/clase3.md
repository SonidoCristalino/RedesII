# Clase nº 3

Todo lo dicho acá, está mejor explicado por el español en la [clase2 md](clase2.md)

# Para el TP y el Tp Final:
	Los temas son: bitcoin firewall TLS/ssl Voip IPSEC Cloud Computing snmp QUIC HTTP2
	Las monografías se comparten luego para todos los alumnos para poder profundizar en los protocolos que cada alumno sienta
	que está flojo.
	Preguntas de los finales, son las que están en el cuestionario del campus. ¡¡Esto es lo importante!!

Ver la filmina del campus. Está más resumido que lo del español.

Los enlaces son suficientemente enormes y rápidos por lo que es poco probable que acá se realice un embotellamiento de datos.
Los que tienen problemas son los conmutadores, hay perdidas y retardos en estos lugares.

Retardos:
--------

- Retardo de transmisión: tamaño del paquete / velocidad del enlace.

- Retardo de propagación: Distancia / velocidad (velocidad de la luz): Tiempo que necesita el bit de ir un lado al otro, la
  velocidad es la misma, lo que cambia es la distancia y el medio.  Varía según el medio,por eso no es lo mismo 1 mega de wirless
  que en fibra. Ya que el de wirless al tener mayor ruido el ambiente, tiene más espacio de cabecera para recuperar los datos
  dañados, a diferencia con el mega de firba que el tamaño de la cabecera es más chica y tiene mayor espacio para la cantidad de
  datos.
  Se tiene más errores en wirleess, que en fibra (ya que los campos electromagnéticos no interfieren). La velocidad por
  eso es más alta, en TCP, en cobre hay mucha interferencia y por eso hay un checksum, en esta tiene que mayor cantidad de
  paquetes retransmitidos, a diferencia de fibra (el protocolo TCP se queda corto con fibra).

- Retardo de cola:
	La cantidad de bits que entran y la cantidad de salida es menor, entonces comienza con un delay, porque hay retardos de
	procesamientos y demás.

	Tracerout: se puede ver todos los saltos que tiene. Trabaja con ICMP. Tiene un decremento de un TTL (time to live). Se le
	pone primero en 1, al llegar al cero al llegar al primer router, lo que hace es anunciar cuando pasa por el primer modem.

Por qué se desarrolla en capas los protocolos:
---------------------------------------------
	Para entender un sistema complejo, para realizar un cambio es más fácil para tratar cada uno de los problemas.

	¿Cuál salió el primero?  TCP/IP que fue el que salió andando primero
	El OSI luego fue lo que salió andando, así que primero fue TCP y luego OSI.
	Es más engorroso hacer protocolos para la capa de sesión y de presentación, por eso en TCP/IP se unificaron en lo de
	aplicación.

Encapsulamiento:
----------------
Encabezados que le va agregando cada capa para ser usado cuando llega del otro lado por la misma capa.
Capa de Enlace: le pone la mac de destino y la mac de origen. Dirección física, es única por cada interfaz, está dado por
el fabricante.
Por ejemplo:
	- Capa de Red: Le pone la dirección IP de origin y destino. Identifica host
	- Capa de Transporte: se le indican los puertos. Se identifica el programa, puerto 21 para https.

Protocolo TCP:
--------------
TCP: Realiza el mejor esfuerzo para mandar el paquete. Pero este no tiene calidad de servicio, ya que al pasar por distintos
proveedores, cada proveedor tendrá según sus propósitos, distintos privilegios dependiendo del tipo de paquetes. Es por ello que
se dice que que no se hace responsable en la calidad de cada uno de los paquetes que recibe.
Con IPV6 antes de enviar el paquete, se envia un ICMP para saber el tamaño en que tiene que fragmentar desde el enviar, y con esto
se evita de antemano que haya retrasos cuando hay mucho tráfico en la red.
En IPV4 la fragmentación de paquetes se va realizando a medida que se envían los paquetes, por lo que es más complicado de
prevenir el tráfico de mensajes desde un principio como hace IPV6.

La perdida de paquetes, se da porque el buffer al ser limitado, los nuevos paquetes son descartados y los paquetes que están en
cola, llegan tarde, porque tienen que esperar a salir.

Capitulo 2: Proxy / Web Caché.

