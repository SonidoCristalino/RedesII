
# Generalidades de TCP

## Cabecera de TCP:
- UDP es más chica la cabecera.
- Puero origen, destino
- Número de secuencia: cantidad de paquetes. Lo que hace es contar los bytes que se envían.
- Número de ACK: envía un número al receptor, apra que el receptor sepa qué paquete fue recibido.
- Largo de la cabecera (comunmente es de 20 bytes, pero como tiene opciones, entonces puede variar).
	Con este campo, lo que permite es saber cómo fragmentarlo en caso de mucho tránsito en la red.

- Opciones:
	* U: Datos urgentes
	* A: Aknoledge, indica que hay una confirmación que llegó bien.
	* P: Que los datos son entregados a la capa de aplicación, significa push, se deben entregar de forma urgente a subir a la
	  capa de aplicación, es un puntero al paquete que a partir de ese se tiene que subir de forma urgente.

[Pag 6]: Se envía una letra C y se hace un echo C desde el otro lado.
En el handshaking es donde se pone en conocimiento los números de secuencia y esas cosas entre los dos host.

Ver que se está confirmado el paquete que le llegó (que es uno más alto) que el Seq.

Round trip time
El timeout tiene que ser más alto que el RTT. Se tiene que hacer toda una especie de cálculo.

[Es mejor verlo desde las filminas]
