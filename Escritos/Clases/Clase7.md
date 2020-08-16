Todo lo escrito acá es sacado de: https://www.youtube.com/watch?time_continue=10&v=gKlDXebzm_4&feature=emb_logo

# Transferencia de Datos Fiables:

Si queremos ofrecer a la Capa de Aplicación un servicio de transporte de datos fiable, teniendo en cuenta que por debajo en la
Capa de Red tenemos el Protocolo IP que nos ofrece un canal No Fiable, debemos implementar un protocolo que cuando desde la Capa
de Aplicación solicita enviar los datos de forma fiable, la Capa de Transporte los envíe de forma NO Fiable y en el receptor, se
puedan recibir los datos de manera fiable y entregarlos a la Capa de Aplicación sin errores ni pérdidas de información.

Lo que se propone es construir un protocolo de transmisión de forma progresiva (Kurose lo denomina RDT) y para graficarla se
utilizan máquinas de estados finitos, tanto en el lado del emisor como del receptor.

### rdt1.0:
Se asume que es un canal totalmente fiable, por lo que no se tendrán errores de bits ni pérdidas de paquetes.

# Máquina de estado:
	En este caso el emisor está esperando la llamada de la Capa de Aplicación con datos para enviar. Cuando le pasan los datos
	llama a la función rdt_enviar() y crea un paquete con esos datos; pasándole el paquete a la función udt_enviar(),
	poniéndose de nuevo a la espera

	El receptor a su vez está esperando un paquete de la Capa de Red, en cuanto lo recibe, llama a la función de
	rdt_recibir(), extrae los datos del paquete y los entrega a la Capa de Aplicación. Como el canal de la Capa de Red lo
	estamos suponiendo fiable no tenemos que hacer nada más.

### rdt2.0:
El canal subyacente puede corromper los bits de un paquete. Aunque todavía es lo suficientemente fiable como para no perder
paquetes.
	- Para ello se utiliza un checksum para detectar los errores de bits.
Pero además se debe implementar un mecanismo para recuperarse de los errores. Para ello se realiza
	- Un reconocimiento positivo (ACK) en los que el receptor le indica al emisor, que ha recibido el paquete correctamente.
	- Reconocimiento negativo (NAK), en los que el receptor le indica al emisor que no ha recibido correctamente el paquete y
	  por tanto el emisor debe retransmitirlo.

Los protocolos de transferencia para datos fiables, basados en retransmisiones se conocen como protocolos ARQ (Automatic Repeat
Request).
Por lo tanto ahora en este protocolo, a diferencia del rdt1.0, el receptor tendrá que detectar los errores, deírselo al emisor, y
en caso de haberlos, éste tendrá que retransmitir el paquete.

# Maquina de estado:
	Emisor:  Primero estará esperando los datos de la Capa de Aplicación, en cuanto lo recibe, llama a la función rdt_enviar()
	que genera el paquete con el dato, añadiendo el checksum para que el receptor pueda comprobar si ha habido errores en la
	comunicación. Envía por el canal ese paquete y pasa al estado de Esperar Reconocimiento, positivo o negativo del receptor.
	Si todo va bien, el paquete llega al receptor, en este caso el receptor que estaba esperando el paquete, comprueba que no
	está corrupto y extrae los datos del mismo para entregarlos a continuación a la Capa de Aplicación, entonces envía un ACK,
	reconociendo al emisor que el paquete ha llegado correctamente. Cuando el emisor recibe un paquete comprueba si es un ACK,
	en ese caso, no hace nada,y vuelve al estado de esperar nuevos datos de la Capa de Aplicación para enviar.
	En los casos en los que hubiese habido un error en la comunicación y el paquete llegara corrupto al receptor, éste enviara
	un NAK al emisor, que cuando lo reciba, simplemente volverá a enviar un paquete y se quedará de nuevo a la espera de
	recibir un ACK.

Todo lo escrito acá es sacado de: https://www.youtube.com/watch?v=odMn55aACOo&feature=youtu.be

## rtd2.1:
Este protocolo tiene un problema y es que como este canal puede corromper los paquetes, se ha supuesto que los mensajes de
reconocimiento (ACK y NAK) no se corrompían al viajar del emisor al receptor. Pero podría ser que esos paquetes también tuviesen
errores.
En ese caso el emisor no sabe si el receptor ha recibido correctamente el paquete, podría volver a preguntar si lo ha recibido
correctamente pero ese paquete también se puede corromper y se entraría en un bucle de preguntas y respuestas, imposible salir de
él.
La otra opción es retransmitir igualmente. Pero en caso de que haya llegado bien, se tendría un paquete duplicado en el receptor.
Hay que manejar por lo tanto, esos posibles duplicados, y para ello el emisor añade un número de secuencia al paquete, de forma
que el receptor puede identificar el paquete que le llega y compararlo con el que ya tiene, y de esta forma descartar el
duplicado.
En un protocolo de Parada y espera (stop and wait) en el que el emisor espera recibir la respuesta del receptor para enviar el
siguiente paquete bastará con 2 números de secuencia, 1 o 0, dependiendo de si está transmitiendo por primera vez o
retransmitiendo un paquete.

Como en rdt2.0 se supone que no hay pérdida de datos no hace falta que el ACK o NAK indique a qué paquete está contestando,porque
será el último, transmitido.

## rdt2.1
De esta manera se introduce la variante rdt2.1, en donde se manejan los ACK o NAK ilegibles.
*El emisor* entonces tendrá 4 estados, los dos de arriba para que lea el paquete:
	- Esperar llamada o de arriba y
	- Esperar ACK o NAK 0. En ambos casos, de cumplirse, se pasa a los estados de abajo.
	- Creando el siguiente paquete de secuencia 1 y
	- Esperando su ACK.

*El receptor* entonces si va todo bien, se transiciona a través de esperar un número de secuencia 0, a esperar el número de
secuencia 1 , a volver a esperar el número de secuencia 0 y así todo el tiempo. Enviando cada vez, el ACK correspondiente, con su
propio checksum, para detectar los ACK o NAK ilegibles. En caso de recibir un paquete corrupto, se envía un NAK, también con su
checksum. En caso de recibir un paquete íntegro, pero que tiene un número de secuencia incorrecto (es decir, un duplicado) se
vuelve a enviar un ACK para recordar al emisor que ese paquete ya está en recepción y que por tanto no es necesario.

### Análisis de rdt2.1
Emisor:
	- Número de secuencia añadido al paquete.
	- Dos números de secuencia (0,1) bastan.
	- Debe chequear si el ACK/NAK recibido está corrupto o es válido.
	- El doble de estados, ya que el estado debe recordar si el paquete actual tiene el número de secuencia 1 o 0.

Receptor:
	- Debe chequear si el paquete recibido está duplicado. El estado indica si se espera número de secuecnia de paquete 0 o 1.
	- El receptor no puede saber si su último ACK/NAK ha sido recibido correctamente en el emisor.

# Variante de rdt2.1, rdt2.2:
Hace exactamente lo mismo que la versión 2.1, pero es un protocolo sin NAK, es decir, que se maneja sólo con ACK. En su lugar se
envía un ACK para el último paquete recibido correctamente. El receptor debe incluir explícitamente el número de secuecnia del
paquete al que se refiere el ACK.
Si el emisor recibi ACK duplicados resultan en la misma acción que NAK: retranmisión del paquete actual. Porque se indica que el
que llegó bien, fue el otro.

Los cambios que se introducen en las máquinas de estado, son simplemente sustituir los NAK, por la comprobación de que llega el
ACK erróneo. Además se deben introducir el número de secuencia para los ACK.

Todo lo escrito acá es sacado de: https://www.youtube.com/watch?v=dgY37MNBipk&feature=emb_logo

# rdt3.0
Es una versión supone que el canal sea con pérdidas y errores. Es decir, además de introducir errores en los paquetes, puede que
los pierda, tantos los de datos como el de reconocimiento.
Los checksum, números de secuencia, ACK y las retransmisiones pueden ayudar pero no ser suficientes.

Para detectar esas pérdidas de paquetes, lo que hará el emisor será esperar un ACK durante un tiempo razonable, y en caso de no
recibirlo, suponer que se ha perdido, por lo que se re-transmite.
Si el paquete (o ACK) se ha retrasado (no se ha perdido) entonces, se tiene un duplicado que se deberá de gestionar como en
versiones anteriores:
	- La retransmisión duplica el paquete, pero el uso de números de secuencia resuelve este problema.
	- El receptor debe especificar el número de secuencia al que se refiere el ACK.
Esta versión, requiere temporizador de cuenta atrás.

En la máquina de estados, se introduce la variable del temporizador, que nos ayuda a detectar las posibles pérdidas de paquetes.
Cada vez que se envíe un paquete, se pone en marcha el temporizador y se detiene cada vez que se recibe un ACK correcto.

Posibles situaciones entre la comunicación del Emisor y el Receptor [minuto 1.40]
	- Que la comunicación haya sido bien y que por cada paquete enviado desde el emisor, el receptor le devuelva un ACK.
	- Que se pierda un paquete enviado por el emisor, por lo que en ese caso, como el emisor NO recibe un ACK de parte del
	  receptor, y el temporizador se acaba, entonces VUELVE a enviar el paquete perdido.
	- Que se pierda sea el ACK0 enviado por el receptor como OK de haber recibido un paquete. Como el emisor no recibe el
	  ACK, entonces se vence el temporizador y vuelve a enviar el mismo paquete. El emisor recibe este paquete que es un
	  DUPLICADO y lo descarta y vuelve a enviar el ACK.
	- Que el ACK que envia el receptor esté desfasado, que la red esté muy congestionada y no llegue a recibirlo el emisor
	  antes de que se venza su temporizador. En este caso, como se vence el temporizador, se vuelve a enviar el paquete, como
	  el receptor ya recibió ese mismo paquete, entonces lo trata como un duplicado y lo desecha. Vuelve a enviar el ACK de
	  ese paquete, lo recibe el emisor dos veces, por lo que desecha también el duplicado del ACK y envia el siguiente
	  paquete.

El rendimiento del rdt3.0 funciona bastante bien, pero a costa del rendimiento de la red. Esto es debido a que soporta un paquete
por cada envío, teniendo que esperar su ACK para enviar el segundo paquete.
Esto hace que la utilización de los protocolos de parada y espera sea muy baja. Ya que el emisor sólo tarda MUY poco tiempo en
enviar el paquete pero se queda ocioso esperando que le llegue la respuesta ACK para recién enviar el siguiente paquete.
Una solución a ello es utilizar protocolos de Procesamiento en cadena, que utilizan el pipelining, que es el mismo concepto de
HTTP. En este caso el emisor envia varios paquetes sin esperar los mensajes ACK (reconocimiento) del receptor. Cuantos más
paquetes haya en viaje sin reconocer (recordar que se envía una ráfaga de paquetes y mientras viajan no hay ACK todavía) más
riesgo se corre cuando se produzca un fallo. Esto supone que haya que aumentar el rango de los números de secuencia y que hay que
añadir buffers en el emisor y el receptor.
Por lo tanto, con esta nueva solución, se puede achicar el tiempo ocioso que tiene el emisor entre la emisión del último paquete y
la llegada del primer ACK.

Todo lo escrito acá es sacado de: https://www.youtube.com/watch?v=lEcpVT-SUHc&feature=emb_logo

# Recuperación de errores mediante procesamiento en cadena:

Hay varios métodos de recuperación de errores mediante procesamiento en cadena:
	- Retroceder N
	- Repetición selectiva.

## Retroceder N
El número de secuencia de k bits en la cabecera del paquete.

Se permite una ventana de tamaño N de paquetes consecutivos no reconocidos.

El emisor podrá tener una ventana de emisión de hasta N paquetes consecutivos sin reconocer en el canal, manteniendo un
temporizador único para todos los paquetes de la ventana, que se corresponderá con el paquete no reconocido más antiguo.
En cuanto a los números de secuencia, deberemos de tener al menos N posibles, por lo si empleamos k bits para representarlos,
entonces 2^k >= N.
La base de la ventana vendrá determinada por el número de secuencia del paquete no reconocido más antiguo. El emisor tiene 3 tipos
de paquetes:
	- Los que se enviaron y recibió el ACK
	- [Ya dentro de la ventana] Los que se enviaron y todavía no se recibió el ACK
	- [Dentro de la ventana] Los que puede enviar porque se lo permite la ventana, pero no enviará hasta que no hayan sido
	  confirmados los anteriores.

El receptor enviará un ACK ACUMULATIVO, es decir, que le dirá al emisor que ha recibido bien hasta un determinado número de
secuencia. Según vaya recibiendo ACK el emisor, irá desplazando la ventana de emisión. Es por ello que este tipo de protocolos se
denominan, "Protocolos de ventana deslizante".
Si el temporizador finaliza, el emisor retransmite todos los paquetes, enviados y NO reconocidos y es por ello que se denomina
"Retroceder N".

### Máquina de estados del Emisor

Si analizamos la máquina de estados finitos del emisor, podemos ver como si se reciben datos para enviar, tendremos que mirar si
hay hueco en la ventana de emisión y en ese caso crear el paquete con el siguiente número de secuencia, los datos y el Checksum y
enviarlo.
También se iniciará el termporizador y se actualiza las variables. Si no hay hueco en la ventana, hay que devolver los datos a la
capa superior que volverá a intentarlo más tarde.
Como nos quedamos esperando el ACK y pueden pasar 3 cosas:
	- Que se reciba un ACK correcto, y en ese caso movemos si se puede la ventana hacia adelante y reiniciamos el
	  temporizador.
	- También se puede recibir un ACK corrupto en cuyo caso se desecha y seguimos esperando al ACK bueno.
	- Si no llega nada y finaliza el temporizador, el emisor retransmite todos los paquetes enviados NO reconocidos y reinicia
	  el temporizador.

### Máquina de estados del Receptor

En el receptor,cuando llega un paquete correcto es decir, sin errores y con el número de secuencia que esperábamos, entregamos los
datos a la capa de aplicación y enviamos el ACK de ese número de secuencia indicando que hasta ese número se ha recibido bien
todo. En cualquier otro caso, por ejemplo si recibimos el paquete con errores o si recibimos uno que no corresponde aún. Se envía
al emisor el último ACK del último paquete recibido correctamente y en orden, para recordárselo. Ese paquete que llega desordenado
al receptor lo que hará, será descartarlo, luego el emisor, como va a reenviar toda la ventana no habrá problema.

En otros casos cuando se pierda un paquete, el peor caso será cuando se pierda el primer paquete que se ha enviado desde la
ventana, porque el receptor va a descartarlos todos.
[Explicacion en el minuto 3.55]

Todo lo escrito acá es sacado de: https://www.youtube.com/watch?time_continue=1&v=3zFGbzWm8O4&feature=emb_logo

[Queda por hacerlo]
