Lo que sigue se saca de: https://www.youtube.com/watch?time_continue=1&v=Y0amVWTl2a0&feature=emb_logo

# Control de Congestión TCP:
Se entiende por congestión, cuando se tienen muchas fuentes enviando muy rápido demasiado tráfico a se manejado por la red. Es
importante *diferenciarlo* del Control de Flujo, donde se intenta en este último, *no saturar el buffer del receptor*.
Con el manejo de congestión, lo que se busca es *no congestionar los buffers PERO de los routers* que se encuentran en el camino.
Cuando hay mucho tráfico, los paquetes pueden acumularse en los buffers de los routers y esto ocasiona grandes retardos
(encolamiento en los buffers de los routers). Hay que destacar que si los buffers de estos routers se llenan, se ocasionan
pérdidas de paquetes.
Este problema de la red, se ha convertido en un importante problema para lo que es la teoría de internet.

## Escenario nº 1:
[0.40 minutos]: Se tienen dos emisores, dos receptores que comparten la red mediante 1 router que asimismo, tiene los buffers
infinitos. Por lo que no se perderán paquetes nunca y no se deberá de retransmitir.
Lo que se tratará de hacer, es administrar el mejor ancho de banda suficiente, para no congestionar la red. Lo que se querrá
permitir, es cada uno de ellos obtenga R/2 de ancho de banda (siendo R la capacidad del canal).
Si llegan determinada cantidad de paquetes, a X velocidad, entonces el router tendrá asignado en cada ancho de banda a R/2.
Si la velocidad de X aumenta por sobre R/2 (el ancho de banda que tiene asignado cada una de las comunicaciones entre el
Cliente y el Servidor) entonces saturará el medio. Cuanto más se acerque X a R/2 el retardo de cola (lo que tiene que esperar un
paquete en el buffer para se despachado) irá creciendo, siendo mayor.

## Escenario nº 2:
[2.20] En este nuevo escenario, se tiene la misma disposición que en el nº 1 pero el buffer NO es infinito, sino finito, por lo
que se podrán perder paquetes y los emisores tendrán que retransmitirlos.
Esto hace que se tengan dos velocidades en el emisor, la IN a la que la aplicación envía los datos al socket y la IN' que es la
velocidad en la que la capa de transporte envía los segmentos que incluirá los datos originales y los retransmitidos (también
denominada carga ofrecida a la red).

Bajo este escenario se pueden tener tres opciones:
	* No perdemos ningún paquete. Enviar sólo con buffer libre, es decir, se envía todo lo que se recibe de la aplicación. Por
	  lo que IN = IN' = OUT. A esta velocidad de transmisión se le suele denominar, Goodput. Las velocidades serán iguales
	  hasta alcanzar el punto de R/2
	* Retransmisión perfecta: sólo se retransmiten, los que realmente se han perdido. Estas retransmisiones quitarán sitio al
	  tráfico original y esto hará que se reduzca la velocidad que se pueda alcanzar en la red.
	* Retransmisión de paquetes retrasados (no perdidos) hace que IN' sea mayor (que en el caso perfecto) para el mismo OUT.
	  Por lo que realizamos transmisiones innecesarias, que llegaron a destino pero se piensan que se habían perdido.

Una vez analizado esto, se tienen dos opciones para poder controlar la congestión:
	* Es aquella en donde la red no proporciona ninguna ayuda, y todo lo tienen que hacer los sistemas terminales de origen y
	  destino. Por lo que se deberá de inferir la congestión por los terminales observando los paquetes que se pierden o se
	  retrasan. Es el método aplicado por TCP.
	* Otra opción es que los routers de la red nos ayuden, proporcionando ayuda a los terminales por ejemplo:
	  	- Mediante un bit que pueda indicar la congestión (SNA, DECbit, TCP/IP, ECN, ATM).
		- Velocidad de transmisión que el router es capaz de soportar.
		- Puede ser directa (choke packet, los cuales son los paquetes de congestión) o a través del receptor.

Sacado de: https://www.youtube.com/watch?v=N4EREQHeY0I&feature=youtu.be

# Mecanismo de control de TCP:
El objetivo en el control de la congestión es intentar transmitir a la máxima velocidad posible, sin congestionar la red, para
perder el menor número de paquetes posible y que no haya que retransmitir. Se debe encontrar la tasa justo por debajo del nivel de
congestión. Hay que hacerlo de forma descentralizada, lo que significa que cada emisor elige su propia tasa basándose en una
realimentación implícita (basándose en la información que recibe mediante los ACK):
	- ACK segmento recibido (buena noticia) -> red no congestionada -> se puede aumentar la tasa de envío.
	- Segmento perdido: se asume que la perdida es debida a la congestión de la red -> decrecer la tasa de envío.

Lo que se hace es ir probando el ancho de banda que se tiene disponible, aumentando la velocidad hasta perder paquetes (donde se
disminuyen la velocidad). Esto hace que se tengan unas velocidades en dientes de sierra características de TCP.
Se dice que TCP es *autotemporizado* porque si llegan los ACK rápido (no hay congestión) aumenta rápido su velocidad de
transmisión, pero si llegan lentamente (lo que indica que hay más congestión) lo hará de forma más lenta.
¿Cómo variamos esa velocidad de transmisión ? El emisor podrá aumentar o disminuir su velocidad de emisión mediante la ventana de
emisión que se denominará Ventana de Congestión, es decir, que si se aumenta el número de paquetes que enviamos y esperamos su
reconocimiento, aumentaremos la velocidad.
Ventana de Congestión NO ES la Ventana de Recepción (esta es la que se tiene en cuenta para el control de flujos). El emisor
deberá de observar ambas ventanas y vendrá limitado por la *menor de ambas ventanas*.
La Ventana de Congestión es dinámica y varía en función de la congestión percibida.

La ventana del emisor será determinada por los paquetes que puede emitir (Ventana de Congestión) por cada RTT (Round Time Trip)
TCP en su versión antigua (1986) y cuando no tenía control de congestión,  se alejaba mucho de la velocidad ideal, ya que se
perdía mucho tiempo retransmitiendo paquetes una y otra vez.

# Variación de la velocidad de transmisión
Lo que realiza TCP cuando se detectan pérdidas de paquetes (control de Congestión) es reducir la velocidad (Ventana de
Congestión). Esto puede ocurrir porque:
	- Se acabó el temporizador, lo que se traduce en no tener respuestas del receptor (por lo que se reduce la ventana de
	  Congestión a 1).
	- 3 ACK duplicados: Esto significa que al menos han pasado algunos segmentos (debido a la retransmisión rápida). En este
	  caso, se reduce la Ventana de Congestión a la mitad (es menos agresivo que el caso anterior).

En caso contrario, TCP aumenta la velocidad cuando recibe ACK indicando que todo va bien. En este caso, aumenta la ventana de
Congestión. Cuando se decide aumentar la velocidad, entonces se tienen dos opciones:
	- Fase de arranque lento: Se aumenta exponencialmente (de forma rápida, al contrario de lo que indica el nombre) al inicio
	  de la conexión o tras un fin de temporización.
	- Evitación de la congestión: Aumentar la velocidad de forma lineal

Cabe resaltar que el aumento o la disminución de la velocidad depende de la versión de TCP que se utilice, por lo que se pueden
implementar los métodos antes descriptos, uno u otro caso.

## Arranque Lento
Lo que se trata es de aumentar la velocidad de forma muy rápida. Esto es así para encontrar el punto de saturación de la red lo
antes posible. De esta forma se eleva la velocidad de forma exponencial, incrementando en un segmento la ventana de Congestión por
cada ACK recibido correctamente: Al comienzo se envía un paquete, se recibe el ACK, se envían 2 paquetes, se recibe el primer ACK
de esos dos, se envían 3 paquetes por cada ACK recibido, y luego por cada ACK recibido se envían 4 paquetes.

La idea es aumentar la velocidad exponencialmente hasta el primer evento de pérdida de paquetes o hasta que se alcance el umbral
de saturación. Se incrementa la Ventana de Congestión en 1 por cada ACK recibido. De esta forma se duplica la Ventana por cada
RTT.

Se establece un valor de umbral de congestión, que será la mitad de la Ventana de Congestión (una vez que se hayan detectado
pérdidas y por lo tanto el umbral de la red se haya detectado).
Una vez alcanzado el umbral, en vez de multiplicar por dos la velocidad (lo cual llevará nuevamente a la congestión de la red) se
pasará a la fase de la *Evitación de la congestión* donde se aumentará la velocidad pero de forma *lineal*; acercándonos al límite
de una forma más gradual.
En cuanto se detecten nuevamente pérdida de paquetes, se lleva nuevamente a la fase de arranque lento, disminuyendo la velocidad
al mínimo.

## Evitación de la congestión
Trata de acercarse a la congestión pero de una forma más lenta que en arranque lento; aumentando la Ventana de Congestión, en un
segmento en total por ventana. Esto se implementa aumentando por cada ACK recibido *1/velocidad de la congestión* . Ejemplo:
	* Si se tenía una ventana de 4 segmentos, cada ACK de estos, aumentará la Ventana de Congestión en 1/4, de forma que
	  cuando reciba los cuatro, se habrá aumentado la ventana en 1 segmento. Estos algoritmos se denominan AIMD (porque
	  aumenta lentamente conforme vaya recibiendo un ACK, pero disminuyen de forma drástica la velocidad; en este caso
	  dividiendo por dos).

Entonces para resumir lo que hace *TCP Tahoe* es:
	- Comienza con un arranque lento, tratando de llegar al umbral de congestión lo antes posible. Cuando se encuentra con
	  pérdidas de segmentos (sea por fin de temporización por ejemplo) lo que se hace es fijar el umbral de congestión A LA
	  MITAD, disminuyendo la velocidad al mínimo.
	- Comienza desde el mínimo nuevamente con Arranque Lento, pero una vez que llega al umbral, cambia el método y pasa a la
	  fase de Evitación de la Congestión. En caso de encontrarse nuevamente con pérdidas de segmentos (por ejemplo con un
	  triple ACK duplicados) entonces vuelve a realizar lo siguieinte:
	- Fija nuevamente el umbral, disminuye la velocidad al mínimo y comenzando con Arranque Lento para que cuando llegue al
	  umbral, adopte el método Evitación de la Congestión.

## Recuperación Rápida
TCP ha ido mejorando con el tiempo, por lo que la versión *TCP RENO* sumó una nueva fase, denominada *Recuperación Rápida*. Esta
fase diferencia la pérdida de paquetes por fin de temporización (lo que indica que la red está muy mal y que en este caso siempre
pasaremos a Arranque Lento disminuyendo la velocidad al mínimo). Esto diferencia de la pérdida por Triple ACK duplicado, lo que
indica que la red no está tan mal, porque llegan ACK por lo tanto se puede disminuir la velocidad menos, en concreto, a la mitad.
Es lo que implica esta nueva fase de recuperación rápida:
	- Si estamos en Evitación de la Congestión, se detecta 3 ACK duplicado o estamos en Arranque Lento y detectamos 3 ACK
	  duplicado se pasa a la fase de *Recuperación Rápida* disminuyendo la velocidad a la mitad.
	- Si recibimos un ACK nuevo, pasamos nuevamente a *Evitación de la congestión* aumentando la velocidad de una forma
	  lineal.

La diferencia entre Taho y RENO es que con la primera se baja la velocidad al mínimo, en cambio con RENO se parte desde el umbral
(entrando en la fase de Evitación de la Congestión) por lo que se recupera más rápido de la pérdida de paquetes.
[Imagen explicativa en 9.00]

Lo que sigue se saca de: https://www.youtube.com/watch?v=J4Igm2hrC_A&feature=youtu.be

De forma Resumida se puede decir que:

	* Cuando la Ventana de Congestión ES MENOR que el umbral, el emisor entrará en fase de *Arranque Lento*, por lo que la
	  ventana crece de forma exponencial.
	* Cuando la Ventana de Congestión ES MAYOR O IGUAL que el umbral, el emisor entrará en fase de *Evitación de la
	  Congestión*, por lo que la ventana crece de forma lineal.
	* Cuando ocurre un Triple ACK Duplicado el umbral se fija al valor (Ventana de Congestión / 2), la VC se fija en el valor
	  del Umbral, y se pasa a la fase de *Recuperación Rápida*.
	* Cuando ocurre un Fin de Temporizador el umbral se fija al valor (Ventana de Congestión / 2), la VC se fijal al mínimo,
	  es decir 1 MSS y se pasa a la fase de *Recuperación Rápida*.

Todas las implementaciones de los distintos protocolo TCP, están implementadas a nivel SO, más bien en los kernels de cada uno de
estos.

## Equidad TCP
El objetivo de la equidad un mecanismo de control es equitativo si cuando N sesiones TCP compartiendo el mismo enlace, el cual
tiene un ancho de banda R, y cada una de esas sesiones tiene una velocidad media de conexión de R/N (se distribuye el ancho de
banda disponible de forma equitativa entre las N sesiones).

### ¿Por qué se dice que TCP es equitativo?
Porque si se tienen 2 conexiones compitiendo por el mismo enlace (de capacidad R) y si una de esas conexiones transmitiría a toda
la capacidad del ancho de banda (asemejándose a R), entonces no dejaría lugar para la otra conexión. Lo mismo sucede para la
segunda conexión.
	- El aumento aditivo aumenta en 1 cuando la tasa de transferencia aumenta
	- El decremento multiplicativo disminuye la tasa de transmisión proporcionalmente

Se dice que un mecanismo de control de congestión es equitativo si la velocidad media de transmisión de cada conexión es
aproximadamente igual a R/N; es decir, cada conexión obtiene la misma cuota del ancho de banda del enlace.
Dado los métodos que emplea TCP para poder gestionar la pérdida de paquetes (vistos anteriormente), la congestión de los enlaces
también se ve beneficiada en tanto que TCP permite que dos conexiones que compiten entre sí por el ancho de banda, puedan
transmitir tendiendo a emplear el mismo ancho de banda.

En este escenario ideal hemos supuesto que sólo las conexiones TCP atraviesan el enlace de cuello de botella, que las conexiones
tienen el mismo valor de RTT y que sólo hay una conexión TCP asociada con cada pareja origendestino. En la práctica, estas
condiciones normalmente no se dan y las aplicaciones clienteservidor pueden por tanto obtener cuotas desiguales del ancho de
banda del enlace. En particular, se ha demostrado que cuando varias conexiones comparten un cuello de botella común, aquellas
sesiones con un valor de RTT menor son capaces de apropiarse más rápidamente del ancho de banda disponible en el enlace, a medida
que éste va liberándose (es decir, abren más rápidamente sus ventanas de congestión) y por tanto disfrutan de una tasa de
transferencia más alta que aquellas conexiones cuyo valor de RTT es más grande.

## Equidad en UDP:
Las aplicaciones multimedia, a menudo utilizan UDP para transportar datos de audio y video. Esto es así ya que no se quiere que
la velocidad de transferencia quede limitado por el control de congestión que establece TCP. Se opta por UDP para que la entrega
de datos multimedia sea a una velocidad constante ya que dadas las características del streaming es tolerante a la pérdida de
paquetes.

El problema que se tiene a nivel red, es que dado que la velocidad aumenta para UDP, por no tener mecanismos de control de
congestión, el tráfico UDP podría llegar a expulsar al tráfico TCP. Como no se controla la velocidad de emisión, un emisor UDP
dadas las ráfagas constantes de paquetes, podría llegar a "adueñarse" del canal de comunicación, quedando reducido el tráfico TCP
a un mínimo. Para este problema, hay varias investigaciones llevadas a cabo que tratan de solucionar esto.

## Equedad en conexiones TCP paralelas
Nada impide que una aplicación abra conexiones paralelas entre 2 hosts, de hecho es lo que hacen los navegadores. Se tiene el
problema por el cual un navegador decide abrir varias conexiones TCP, podría expulsar a otras aplicaciones que sólo abren una.

Se puede notar la diferencia entre que una aplicación pida una sola conexión a que pida varias (pudiendo adueñarse del canal):
	- Por ejemplo, se tiene un enlace que YA TIENE 9 conexiones realizando transmisión de datos, y una nueva aplicación pide
	  una conexión más, va a obtener una tasa de transmisión de R/10 (ya que el ancho de banda se divide entre las 10
	  conexiones).
	- Pero ahora una nueva aplicación en un nuevo enlace pide 11 conexiones TCP, el ancho de banda se ve reducido sólo a R/2,
	  es decir, la mitad del canal para ella sola, porque de las 20 conexiones, tendría 11 para ella.

