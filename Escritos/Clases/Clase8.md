Todo lo escrito acá, se saca de: https://www.youtube.com/watch?v=HuBPwbzX76Y&feature=emb_logo

# Protocolo TCP:
Es un protocolo de la capa de transporte de Internet, fiable y orientado a conexión. En el que participan un emisor y un receptor,
los cuales al establecer una conexión se produce un acuerdo en 3 fases o handshaking (intercambio de mensajes de control) donde se
inicializa el estado del emisor y del receptor previo al intercambio de datos.

Se envían datos de forma bidireccional o fullduplex por esa misma conexión.
Es un servicio FullDuplex:
	- Flujo de datos bidireccional en la misma conexión
	- MSS: Maximum Segment Size.

Es un protocolo de procesamiento en cadena en la que se tiene una ventana de emisión, que se utiliza para minimizar tanto la
congestión de la red como controlar el flujo para evitar saturar al receptor.  Tiene un control de flujos, para evitar que el
emisor no sature al receptor.

Se tienen buffers tanto en la emisión como en la recepción

## Estructura de un segmento TCP

### Cabecera:
	- Como en UDP se tienen los 16 bits del puerto de origen y del puerto destino, además del checksum.
	- También tiene un campo longitud como en UDP pero en este caso es sólo de la cabecera, indicando el número de 32bits que
	  tiene la cabecera y que ocupa unicamente 4 bits.
	- La longitud de la cabecera se deben indicar porque se tiene un campo de opciones que es de longitud variable, por lo que
	  es opcional y esto hace que el tamaño de la cabecera sea variable (aunque normlamente será de unos 20 bytes).
	- El campo de OPCIONES se suele utilizar por ejemplo para negociar el tamaño máximo de segmentos (MSS descripto arriba).

	- Números de secuencia y Reconomiciento: son nuevos con respecto a UDP (ya que no los tenía) y son de 32 bytes cada uno,
	  que hacen referencia a bytes en la secuencia de datos, no son números de segmentos.

	- Ventana de recepción: Se utiliza para el control de flujo, se indica el número de bytes que está dispuesto a aceptar el
	  host que ha enviado este segmento.

	- Puntero de datos urgente: Se encuentra en desuso.

	- Grupo de bits (UAPRSF): Se utilizan para indicar si son datos urgentes, o si el ACK es válido (dado por el número de
	  reconocimiento); Si el receptor debe pasar los datos inmediatamente a la capa superior o si se trata de un segmento de
	  establecimiento o cierre de la conexión.

### Datos de la aplicación (De longitud variable):
	datos a transportar. Datos de la capa de aplicación que lleva el segmento en su interior.

## Transferencia de datos fiables

Se debe garantizar la transferencia fiable de los datos sobre un servicio como IP que NO es fiable, sino de "mejor esfuerzo". Para
esto emplea ACK acumulativos para reconocer los paquetes recibidos y un temporizador para responder a las pérdidas de paquetes.

También tiene una ventana de emisión que determinará el número de segmentos a enviar en cadena.

Se decidirá retransmitir segmentos cuando el emisor reciba ACK duplicados, además de cuando hay un fin de temporizador.

Se considera un escenario *simplificado* donde se ignoran los ACK duplicados como también el control de flujos y el control de
congestión.

## Números de Secuencia y de Reconocimiento

Como la comunicación es bidireccional, cada segmento llevará en su campo Número de secuencia, el número el primer byte de la
secuencia de datos que está enviando.
En el ejemplo de la filmina del video se utiliza una comunicación entre host donde el emisor envía una C y el receptor le devuelve
esa C, funcionando como una terminal mediante SSH, y realizando un echo.
Se envía el número de secuencia nº 42, donde el dato es 'C', como el dato 'C' es de un byte,entonces el próximo dato que el emisor
envíe, tendrá un número de secuencia 43. Si en caso que enviara 10 bytes, el siguiente nº de secuencia sería 52.

A su vez, el receptor envia un byte al emisor (recordar que hace un echo) y tiene el número de secuencia 79. En caso que el
receptor le vuelva a enviar un byte, entonces el próximo número de secuencia será el nº 80.

En cuanto al número de ACK, cada extremo de la comunicación indicará cuál es el siguiente número de secuencia que espera recibir.
RECORDAR que son ACK ACUMULATIVOS.

En el ejemplo del video:
	- Emisor (A): Seq = 42; ACK = 79; Data = 'C. Le envía un ACK: 79, lo que significa que recibió todos los bytes hasta el
	  78 de forma correctamente.
	- Receptor (B) : Seq = 79; ACK 42; Data = 'C'. Y el número 79 es justamente el que B envía a A, diciéndole a su vez que
	  está esperando el 43 (como número de ACK) ya que acaba de recibir el 42. Como B le envía un byte a A, el 79, A le debe
	  decir a B, que el siguiente segmento que espera es el 80. Así sucesivamente.

¿Qué hace el receptor cuando llega un segmento fuera de orden? ¿Lo descarta como Go Back N o lo almacena como en Repetición
Selectiva? En la especificación de TCP NO se dice nada, dependerá de la implementación del protocolo en CADA caso; normalmente se
almacena para su posterior entrega.

Se continúa en video: https://www.youtube.com/watch?v=nsNRKnuTKcg&feature=youtu.be

# Enventos ante los cuales tiene que responder un emisor en TCP:
	- Que reciba datos desde la capa de aplicación que deben ser enviados.
		* En este caso crea un segmento TCP con esos datos
		* Lo crea con un número de secuencia que corresponda a su primer byte dentro de todo el flujo de bytes de la
		  comunicación que lleve con ese host destino.
		* Inicia el temporizador si no estaba ya corriendo, fijando su valor.
	- Fin de temporización:
	  	* Emisor retransmite el segmento que lo causó
		* Reinicia el temporizador al segmento no reconocido más antiguo.
	- El emisor recibe un ACK:
	  	* Si reconoce segmentos previamente no reconocidos (unACKed), entonces actualiza esa información de segmentos
		  reconocidos (reconociendo los segmentos)
		* Inicia el temporizador si aún hay segmentos no reconocidos.


# Retransmisiones TCP:
---------------------

## Ejemplo nº 1:
	- Emisor (A): Envia un primer segmento con 8 bytes de datos a B. Del 92 al 99.
	- Receptor(B): Lo recibe correctamente y envía el ACK = 100. Pero se pierde este ACK.
	- A : Se da cuenta de esta perdida porque el temporizador finaliza y tendrá que reenviar el segmento (porque no sabe si se
	  perdió el ACK o el segmento).
	- B : Recibe nuevamente el segmento del 92 al 99 LO DESECHA porque ya lo tiene y vuelve a enviar el ACK = 100. En cuanto
	  lo reciba, mueve la ventana de emisión a 100.

## Ejemplo nº 2:

	- A: Envía nuevamente un segmento de 8 bytes de datos a B. Del 92 al 99, con el número de secuencia = 92. Y envía OTRO de
	  20 bytes, con el número de secuencia 92, Secuencia = 100, por lo que sería del 100 al 119.
	- B: Se supone que los dos ACK con número 100 y 120 llegan MAS TARDE que el fin de temporización que tenía A para el
	  primer segmento.
	- A: Reenvía ese primer segmento.
	- B: Lo desecha y responde con un ACK = 120. Que es el siguiente Byte que está esperando.
	- A: Pero A movió su ventana temporal porque le llegaron los ACK pero TARDE.

	Como son ACK acumulativos si se pierde un ACK, pero llega el siguiente, entonces le está diciendo a A que llegaron
	correctamente todos los anteriores segmentos.  En este ejemplo, llega el 120, por lo que B le está diciendo que tiene
	hasta el 119 y espera el 120.


## Eventos del lado del Receptor (B):
	- Receptor: Llegada de un segmento en orden con el número de secuencia esperando. Todos los datos hasta el número de
	  secuencia esperado ya han sido reconocidos.
		* Para ello tiene un mecanismo denominado *ACK retardado* que lo que hace es esperar medio segundo a ver si llega
		  otro segmento en orden que será lo más habitual y probable. En caso de que llegue, enviará un único ACK
		  retardado, reconociendo ambos segmentos. Si tras ese primer segmento recibido no llega otro, envía un ACK para
		  ese segmento.
	- Receptor: Otro posible evento sería la llegada de un segmento con un número de secuencia mayor que el esperado, creando
	  un huevo en la recepción.
	  	* El receptor enviará un ACK que estará duplicado, porque le está diciendo que espera el bit que se encuentra en
		  el límite inferior de ese hueco. En cuanto llegue un segmento que complete ese hueco enviará el receptor un ACK
		  con el siguiente byte esperado.
	[Minuto 4.50 hay una explicación muy buena, recordar que es el RECEPTOR]


# Retransmisión Rápida:

El emisor tenía que esperar al fin del temporizador para retransmitir un paquete. Cuando está recibiendo ACK duplicados del
receptor que ya le estaban indicando que ese paquete muy probablemente se había perdido. Para disminutir el tiempo de respuesta
ante perdida de paquetes, la Retransmisión Rápida se realiza cuando el emisor recibe 3 paquetes ACK duplicados, es decir, el ACK
original, y 2 posteriores, reconociendo los mismos datos. En este caso el *Emisor* retransmite el segmento sin esperar al fin de
temporización.


Continúa en el video: https://www.youtube.com/watch?v=Xk_16aK665g&feature=youtu.be

# Estimación del RTT y del TimeOut:
La pregunta es ¿Cómo establecer el valor del temporizador? Es decir, ¿Cuánto tiempo debe esperar el Emisor al ACK de un segmento?
Es lógico que ese valor sea mayor que el RTT, para darle tiempo al segmento a llegar a destino ya que el ACK viaje de vuelta.

Pero el RTT varía, si se establece un temporizador muy corto, no le daremos tiempo y se retransmitirán muchos segmentos que ya
llegaron a destino; mientras que si se configura un temporizador muy largo esperaremos demasiado cuando se pierde un segmento y
reaccionaremos tarde. Así que lo que nos queda es ajustarlo lo MAS posible al RTT.

¿Cómo sabemos de antemano cuánto tardará un segmento en viajar por la red y en volver su ACK? Lo que se hace es tomar el último
tiempo que tenemos como muestra pero como puede ser este último, atipico se hace el promedio con las medidas recientes, en vez de
con las RTT Muestra.

De esta forma para estimar el RTT, lo que se hace es calcularle una Media Movil exponencialmente ponderada:
	- Se toma la última muestra y dándole un peso de Alfa.
	- Valor de Alfa = 0,125
	- Se le añade al histórico de estimaciones que tendrá mayor peso.
	- La influencia de estas muestras más antiguas irá decreciendo exponencialmente.

De esta forma se logra que los valores que se salen de la normalidad NO afecten demasiado a la estimación del RTT.

Una vez que se tiene el tiempo de Ida Y vuelta de un determinado momento se podrá establecer el temporizador añadiendo al RTT
estimado un "Margen de Seguridad" para no ser muy estrictos. Ese margen, lo ideal es que si el RTT estimado varía mucho, el margen
sea mayor que si tenemos una estimación de RTT que sea bastante constante, por lo que podremos justar más ese margen.
De esta forma si calculamos el error en cada muestra de RTT con lo que se había estimado (Lo que sería la desviación) y se vuelve
a hacer una media móvil exponencialmente ponderada con un valor típico de 25% para la desviación de la muestra última y tomando el
histórico igual que antes se puede tener la desviación del RTT.

El margen que le daremos para el temporizador será de ese RTT Estimado + 4 veces esa desviación del RTT acabado de calcular.

Cabe resaltar que si hay que retransmitir un segmento por fin de termporización el siguiente temporizador se establecerá al DOBLE
de su valor mientras que si iniciamos un temporizador por la recepción de ACK lo establecemos según lo dicho anteriormente.
Esto es así ya que si debemos retransmitir por fin de temporización es que la red está muy mal, no ha llegado el ACK y vamos a
darle un poco más de tiempo para la próxima vez. Mientras que si van llegando ACK que no sean los que se esperaba es que la red no
está tan mal porque al menos llegan paquetes, por lo que estableceremos un temporizador un poco más ajustado.

La continuación del tema se saca de: https://www.youtube.com/watch?v=nBKBN2Re3Xg&feature=youtu.be

# Control de flujo del Emisor:

Para evitar desbordar el buffer del receptor cuando transmitimos muy rápido demasiados datos. Ese buffer se irá llenando con los
datos que van llegando y si la aplicación no va vaciando con frecuencia, puede llegar a llenarse. Lo que hará TCP será adaptar la
velocidad de envío del emisor y de esta forma no llenar el buffer y no perder paquetes.

Para ello el receptor le irá informando al receptor de cuánto espacio le queda libre en el buffer (Ventana de Recepción); entonces
el emisor limitará el número de bytes no reconocidos (Ventana de Emisión) al tamaño de esa ventan de Recepción. Si se llena el
buffer, entonces el receptor dirá que su Ventana de Recepción = 0. Y el emisor no podrá enviar más segmentos. ¿Cómo se entera el
emisor cuando la aplicación vacía el buffer?  El emisor irá enviando segmentos de prueba (sin datos o con bytes de pruebas)  para
detectar cuándo hay espacio en el buffer del Receptor.

# Gestión de la conexión en TCP:

Protocolo orientado a conexión por lo que tanto Emisor como Receptor deben establecer una conexión bidirecciónal antes de enviar
cualquier dato. En este inicialización, ambos inicializarán las variables con sus números de secuencia, buffers, la información de
sus ventanas de recepción, etc.
Será el cliente el que inicie la conexión, creando un socket mientras el servidor recibirá esa conexión de su socket de acogida,
creando un socket especial para recibir y enviar el tráfico hacia el proceso de ese cliente.

# Establecimiento de la conexión en TCP:
Se denomina Acuerdo en 3 fases. O triple Handshake, pues se hará en 3 pasos:
	1. El cliente envía un segmento especial, SIN datos en el que se pone el indicador SYN=1 de la cabecera. Además este
	   segmento indicará al servidor qué número de secuencia de forma aleatoria ha elegido el cliente para iniciar su
	   comunicación.
	2. El servidor contesta con un segmento SYN=1 ACK también sin datos. Y ambos indicadores SYN y ACK estarán a 1. También
	   incluye el número de secuencia que haya elegido de forma aleatoria el servidor a su vez para sus comunicaciones.
	   Pondrá además,que está esperando el siguiente número de secuencia del que envió el cliente. También se establecen las
	   ventanas de cada uno para que tanto el Emisor como el Receptor puedan establecer su control de flujo.
	3. El cliente envía un SYNACK especificando que recibió el ACK del servidor, y quedando la conexión establecida. Este
	   segmento por lo tanto tendrá el bit SYN=0 y el bit ACK=1. Poniendo en su bit ACK el siguiente número secuencial que
	   envió el servidor. Es el ÚNICO momento de la comunicación que los bits de Secuencia y ACK en 1 aunque NO se envíen
	   datos.

# Para cerrar la conexión:
	- Para cerrar la conexión el cliente al cerrar el socket lo que provoca es que le envíe al servidor un segmento de control
	  con el bit FIN=1.
	- El servidor responde con un ACK y cuando termine de enviar (si es que tiene que enviar más segmentos el servidor)
	  también cerrará su socket, enviando a su vez un segmento FIN al cliente.
	  El servidor esperará un determinado tiempo para cerrar definitivamente la conexión.
	- Cuando el Cliente reciba el FIN del Servidor también responde con un ACK y espera un tiempo a su vez, para cerrar la
	  conexión.

