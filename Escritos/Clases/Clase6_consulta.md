# Clase nº 6: 13/06/2020

	* En el libro de Kurose, es a sección 3.4.1

[faltan 15 minutos]

# rdt2.0:
Cuando los NAK vienen SIN ERRORES.

Está todo explicado en las filminas.

Manejo de ACK/NAKs errados.
Transimisor y el doble de estados que se tenía.
Ver que en la filmina de cambio de estados, para el emisor.

Receptor:
	- Espera un paquete de la capa de red: udt_sender (se envía por la capa de red).
	- Ver que la parte derecha espera un número 1 de secuencia, a diferencia el de la izquierda, que espera un 0. Son cero y
	  uno nomás. TCP utiliza más números de secuencia.
# rdt2.1:
	- rdt2.1: Transimisor es suficiente 0 y 1 porque es un protocolo de stop and wait.

Herramientas para recuperarmos de los bits errados:
	- Checksum
	- Además de los mensajes entre emisor y transmisor (ack y nak).
	- Numeros de secuencias, para que el receptor pueda dar cuenta de los duplicados.

# rdt2.2:
	- Prescindir NAK porque consumen más recursos al duplicar el envío de paquetes.
	- Otra herramienta para el paquete que nunca llega: Se busca usar sólo ACKs.
	- ACK se le agrega número de secuencia. Para poder confirmar qué paquete le llega. Envía un ACK con la secuencia que SI le
	  llegó, si envía un 0 y le llega bien entonces, envía un ACK con ls secuencia 0,y espera un 1. En caso de no recibirlo, o
	  recibirlo con error, entonces vuelve a enviar un ACK con 0.

	  Fragmentos del transmisor y receptor:
	  	- El único que cambia la máquina de estado.

# rdt3.0:
Esperar un tiempo prudencial para retransmitir un paquete [Ver bien la filmina]

# Máquina de Estados finitos:
	- Esperamos de la capa de aplicación.
	- Se envia el paquete a la capa de red, y se inicia un timer.
	- Si está corrupto o es un AKS 1, no se hace nada, sino que se reenvía el paquete.
	- Se envía un paquete, es un ACK, con el número de secuencia correcto: se reactiva el timer y pasamos de estado.
	-

[No terminé la clase]


