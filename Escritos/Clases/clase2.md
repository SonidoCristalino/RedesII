# ¿Qué es internet?
Varias definiciones:
	- Como una red de dispositivos conectados entre sí. Antes sólo se conectaban ordenadores, ahora también se conectan cualquier tipo
	de dispositivos electrónicos, como heladeras, microondas, autos, etc. IOT (Internet de las cosas). Conexión de muchos dispositivos
	entre si.

	- Red de servicios, que se nos ofrece como una forma de participar en redes sociales, ver videos, enviar mensajes, comprar, vender.

	- También conexión de redes, por lo que se define como _Red de Redes_ . Ya que no deja de ser redes separadas (hogareñas,
	  corporativas, de universidades, institucionales etc) que se conectan todas entre si, mediante servicios denominados ISP
	  (Internet service provider). Intranet (redes privadas)

# Protocolos ¿Qué son?
	- Conjunto de reglas que (en este caso los dispositivos) hay que seguir para poder comunicarse de forma correcta.

Tres funciones principales:
	- Formato
	- Orden de los mensajes enviados y recibidos
	- Acciones que se ejecutan sobre los mensajes.
Algunos de los protocolos más conocidos de internet pueden ser: wifi, tcp/ip, FTP, HTTP, SMTP.
Los protocolos pueden ser definidos e implementados por empresas privadas, los denominados _protocolos propietarios_ o por
entidades de caracter público como la IEEE, ISO, ITU, etc, y lo hacen mediante los documentos RFC (donde se piden referencias o
ayudas a expertos para nuevas funcionalidades que se requieren.

# Estructura de la red:
	- Frontera de red: formada por dispositivos últimos como ordenadores, celulares, impresoras, etc que se conectan a la red
	- Nucleo de la red: formado por todos dispostivios de red (routers, switches por ejemplo) que ponen a disposición las ISP
	  para poder conectar todo entre sí.
	- Redes de Acceso: son las redes que conectan los dispositivos que están en la frontera con los del nucleo (por ejemplo el
	  cable que sale de mi casa y que hace todo el recorrido hassta llegar al ISP contratado).

# Frontera de red
Está formada por sistemas terminales o host, los cuales ejecutan eplicaciones y están conectados a la red. POr ejemplo un
ordenador que ejecuta un navegador o un servidor que ejecuta un servidor de email. Están en los extremos de la comunicación.
Existen dos modelos de comunicación.
	- Cliente/servidor: donde un cliente solicita un servicio y esa petición está dirigida a una máquina que hace de proveedor
	  de un servicio que le responde, como en el caso de las páginas de internet.
	- Peer to Peer: donde los host se comunican entre sí sin la necesidad de un servidor que centralice la comunicación, como
	  puede ser el ejemplo de skype o bitTorrent.

# Red de Acceso y medio físico
Los sistemas terminales se deben poder conectar a la red por lo que lo hacen a través de lo que se denomina _red de acceso_ que
son los routers o puntos de accesos a los que se conectan los host para poder acceder a internet.
Lo hacen mediante un medio físico, por el aire o por un cable.

	## Medios físicos:
		- Par trenzado(twisted pair): Cable más frecuente y más barato. Se utilizaba para las comunicaciones telefónicas.
		  El cable iba enrollado en espiral para reducir las interferencias eléctricas.
		  Para red se utiliza el cable ethernet que lleva 4 pares de cobre trenzados finalizando en un conector RJ45 que
		  puede apantallado o no. Según la calidad del cable y el tipo de apantallado se tienen muchas categorías, lo que
		  mejora la velocidad de transmisión.
		- Cable coaxial: de cobre también, donde los dos conductores de cobre están posicionados de forma concéntrica.
		  Protegidos por una malla (antena de televisión).
		- Fibra óptica: no transporta señales eléctricas sino pulsos de luz, lo que lo hace inmune a las interferencias
		  eléctricas. Aunque hay que tener en cuenta los ángulos de curvatura extremos.
		  Son capaces de transmitir información a muy alta velocidad (llegando al Terabyte por segundo) a grandes
		  distancias, lo que lo hace perfecto para enlaces transoceánicos o enlaces en el núcleo de la red.
			Existen dos tipos:
				- monomodo: equipamiento más caro que transmite a muy altas velocidades a mucha distancia
				- multimodo:
		- Aire: Otro medio para transmitir datos, que al no transmitir por medios. Tiene una ventaja de menor coste, ya
		  que no hay que tirar cable, pero hay problemas de reflexión, interferencias.
		  Muchas tecnologías que emplean este medio como por ejemplo Wifi, Bluethoot, wimax. Las velocidades son muchos
		  menores que con cables, y tienen el problema de las intereferencias y los errores en datos son mucho mayores.
		  Pues el medio que es el aire es compartido por todos.

	## Acceso a la red (entre Redes hogareñas/privadas hasta ISP):
		- Red telefónica: Fue la que se empleo al comienzo dado que ya el tendido de red ya estaba puesto. Para ello fue
		  necesario el uso de un dispositivo denominado Modem en ambos extremos de la comunicación, que se encargaba de
		  convertir la señal digital emitida por el dispositivo en pulsos electrónicos que viajaba mediante el tendido
		  telefónico de par trenzado. La desventaja es que cuando se utilizaba internet, se tenía ocupada la línea de
		  teléfono y las velocidades eran muy bajas (56k/s).
		- xDSL: Permitían meter por el mismo cable de par trenzado tanto teléfono como datos en simultáneo mediante el uso
		  de microfiltros o y splitters (que dividen la señal para introducirla en el cable). Alcanzando velocidades de
		  hasta 1Gb en la versión más moderna.
		  Línea física dedicada para el usuario hasta la estación central telefónica. Lo malo es que la señal sufre mucho
		  por la calidad del cable y la distancia hacia la central, disminuyendo la velocidad final de transferencia
		- Acceso por cable: cable coaxial, la cual aprovecha la infraestructura de la red de televisión por cable (muy
		  difundida en USA). Comparte canales como el Acceso de Red Telefónica, por lo que no es dedicada como el DSL.
		  Son mayores que DSL hasta casi los 10gb. La desventaja es que dependen mucho de la cantidad de usuarios y del
		  tráfico que estos generen, ya que comparten el modem desde el router hasta el ISP.
		- FTTH (Fiber to the home): donde la fibra óptica llega hasta el domicilio ONT, y se despliea hasta la central
		  OLT. Dos tipos de redes, activas y pasivas (más utilizada la primera). Llega a una velocidad de 500mb/s sin
		  perdidas por distancia y metiendo en el mismo cable varios servicios, como telefónica, tv e internet.

	## Acceso dentro de la red privada o institucional:
		- Los host también deben conectarse dentro de la propia red a la que pertenecen y ello lo hacen conmunmente por
		  cable Ethernet (el que tiene por extremos una ficha RJ45), llegando a alcanzar velocidades de hasta 100Gb/s
		  entre terminales. ¡Recordar que esto es DENTRO de la red doméstica, privada o de la institución; hacia afuera de
		  la red, hacia los IPS la velocidad varía dependiendo del servicio contratado por la institución u hogar.
		- La otra opción es conectar los dispostivios mediante puntos de acceso, es decir, de modo inalámbrico. Por
		  ejemplo con redes de wifi (6gb/s), por Wimax que alcanzan distancias más grandes, o sino Bluethoot; también por
		  redes móviles como puede ser 3G, 4G o 5G.

	## Acceso dentro de la red hogareña:
		- De la red hogareña hacia afuera, es decir, desde el router/modem hacia el ISP dependerá del servicio contratado,
		  y hacia adentro de la red, se pueden establecer varios puntos como puntos de acceso de wifi o cableado por
		  ethernet.

# Núcleo de Red
Lo forma toda la infraestructura que nos proveen los servidores de internet (ISP) para conectar todas las redes entre sí. Serán
por ejemplo todos los routers, swtiches, cableado, que transmiten los datos por la red. Son limitados y compartidos entre los
dispositivos que quieran conectarse entre si. Dependiendo de **COMO se compartarn estos recursos** es que se tienen dos formas de
transmisión de datos a través de la red:

## Formas de compartir recursos:
	- Conmutación de Circuitos: consiste en reservar todos los recursos necesarios para establecer una conexión entre
	  dispositivos previa a establecer la conexión. Esta conexión será terminal a terminal, y no se alterará una vez
	  establecida dicha conexión. Cabe mencionar que estos recursos son DEDICADOS, es decir que de no utilizarse, se pierde su
	  uso para otros usuarios. La principal ventaja es que se tienen las prestaciones garantizadas, por lo que si el tráfico
	  sigue un patrón conocido, la comnutación será ideal.
	  Todo ello significa un coste de tiempo y recursos.
	  Esto se logra mediante la multiplexación de las señales de los distintos usuarios. Se puede multiplexar en frecuencia,
	  en tiempo, por código.
	  FDM: por frecuencia, se tienen anchos de frecuencia dedicados a cada usuario. En el caso de que sean 4 usuarios, se
	  tendrán dedicadas 4 distintas frecuencias durante todo el tiempo.
	  TDM: es el uso de los recursos fraccionados en porciones de tiempo, el cual tiene todo el ancho de banda posible para
	  transmitir cada cierta cantidad de tiempo.

	- Conmutación de Paquetes: En este caso los datos se "cortan" en paquetes y se envían por el canal utilizando el ancho de
	  banda completo, SIN RESERVAR previamente los recursos por lo que se puede tener suerte y tener el canal libre o por lo
	  que también podría estar ocupado y producir congestión en la red. Esta técnica, fue propuesta en los 60 y es la que se
	  emplea hoy en día.
	  Esta compartición del ancho de banda del canal se denomina "Multiplexación estadística" y consiste en que al no tener un
	  patrón fijo de cómo se deberá comportar el tráfico los usuarios irán empleando los recursos según lo vayan necesitando
	  SIN reservar los mismos. Esto permite que la Conmutación de Paquetes soporte los mismos o hasta más usuarios que la
	  conmutación de circuitos. Cada nodo tiene una velocidad de salida por el enlace, y cada paquete debe llegar completo al
	  siguiente conmutador antes de que este mensaje completo se vuelva a transmitir, es lo que se conoce como "Almacenamiento
	  y reenvio" . Cada conmutador tiene distintos enlaces de salida, y en la salida de ellos se tiene un buffer o cola de
	  salida, donde se pueden generar esperas (llamados retardos de cola), dependiendo del estado de congestión de la red.
	  Como los buffers tienen una capacidad de almacenamiento finito, este puede estar lleno por lo que los paquetes pueden
	  ser desechados, y se perderá. Por lo que hay posibilidad de pérdidas de paquetes.

## Conmutación de paquetes vs circuitos:
	- En esquemas de rágafas de datos como es lo habitual en internet, es mucho más eficiente, más simple y funciona mejor que
	  la de circuitos además de soportar más usuarios. Aunque en los casos en donde hay congestión, los retardos y pérdidas de
	  paquetes pueden llegar a ser muy grandes.
	- Hay casos en donde es deseable una conmutación de circuitos como por ejemplo en las transmisiones multimedia en lo que
	  los retardos son vitales. Es por ello que hay proveedores de servicios que utilizan protocolos como MPLS para crear
	  circuitos virtuales sobre una red de conmutación de paquetes como es internet.

## Jerarquía de Internet:
Las redes que conforman a internet, no son todas iguales. Internet no tiene una estructura plana, por lo que hay redes más
importantes que otras. Existe una jerarquía de niveles.
	## ISP-1:
		Las redes más importantes por su estructura, y su importancia son las denominadas "redes troncales" o ISP de nivel
		1 (denominadas también TIRE-1). Los proveedores que han establecido este tipo de redes, tienen una cobertura
		internacional, con velocidades muy altas y conectadas entre si.
		Lo más importante es que se tratan como iguales por lo que NO pagan a otras empresas para que lleven su tráfico de
		datos.  Ejemplos de estos proveedores: Level-3, Cogent, Tinet, Deutsche Telecom, Telefónica,etc.
	## ISP-2
		Son los que llegan a acuerdos con algunos ISTS, deben pagar a otros para que lleven su tráfico, por lo que los
		ISP-2 son CLIENTES de los ISP-1 que actúan como proveedores. Los ISP-2 suelen tener cobertura nacional o regional.
		Se conectan entre otros ISP-2 y del nivel 1. Los puntos de conexión entre los ISP "Puntos de Presencia" o Poin of
		Presense.
	## ISP-3
		Proveedores de acceso a internet a nivel local que se conectan a otros ISP para poder dar un servicio. TAMBIEN
		puede pasar que un ISP-1 actúe como uno local, es decir ISP-2 o local (ISP-1) creando una relación vertical.
		Cuando dos ISP se encuentran en el mismo nivel se dicen que son "igualitarios"
		Por lo que un paquete pasa a través de muchas redes de los distintos ISP hasta llegar a us destino.

## Diferencia entre ISP:
	- El que un ISP sea de nivel 1,2 o 3 depende fundamentalmente del flujo del dinero: si un ISP no tiene que pagar a nadie
	  para enviar paquetes por la red, entonces se dice que este es de tipo 1.
	- Los ISP-2 si tienen que pagar a sus proveedores, que serían los ISP-1 para transferir los paquetes. Los del nivel 2 se
	  comunican también entre sí, no solo hacia arriba sino entre si.
	- Los ISP-3 se comunicacn directamente con niveles superiores (ISP-2) y NO entre sí

# Retardos y perdidas de paquetes:
Existen cuatro motivos dentro de la conmutación de paquetes por lo que se pueden retrasar los paquetes:

	- Retardo de Procesamiento en el Nodo: La principal fuente de retardo es la que se produce cuando llega un paquete desde
	  un ordenador a un nodo. El nodo debe procesar ese paquete, es decir, comprobar si tiene errores y determinar por cuál
	  enlace debe salir ese paquete. Es el denominado "Retardo de procesamiento en el nodo". Este retardo suele ser del orden
	  de los microsegundos.
	- Retardo de Cola: El paquete se dirige al enlace de salida (la puerta de salida del nodo) donde se encuentra que ya hay
	  un paquete del host A esperando a salir por él, por lo que tendrá que esperar, para ser transmitido. Ese tiempo de
	  espera en la cola de salida del nodo se conoce como "retardo de cola". Es el tiempo de espera para transmitir el paquete
	  por el enlace de salida. Y dependerá del nivel de congestión que tenga el nodo en el momento de transmitir, variando el
	  tiempo de unos microsegundos o milisegundos si hay mucho tráfico en espera.

	- Retardo de tranmisión: Es el tiempo que tarda el nodo en meter el paquete por el enlace. Este tiempo dependerá de la
	  longitud del paquete (L) y de la capacidad del ancho de banda del enlace (R). El cual se puede calcular fácilmente
	  realizando la división L/R. Suele ser del orden de micro o milisegundos.
	- Retardo de propagación: Es el retardo que se produce al viajar un paquete del nodo de salida al nodo de llegada a través
	  del enlace. Dependerá de este retardo de la longitud del enlace (S) y de la velocidad del medio de propagación (V),que
	  suele ser del orden de los microsegundos, aunque a grandes distancias puede llegar a los milisegundos.

El retardo más interesante para su estudio es el retardo de cola: Porque depende de la congestión y de la naturaleza del tráfico
que haya a la hora de transmitir el paquete, si es en ráfagas o periódicamente. Variando así de un paquete a otro.
En el nodo se puede calcular al tráfico que existe entre haciendo la longitud del paquete * por la taza de paquetes de llegada,
dividido el tráfico de salida. Es la relación que existe entre el tráfico que llega y el que sale.

Los paquetes se pierden porque la cola de salida en el enlace tienen un límite, que de sobrepasarse los paquetes comienzan a
perderse. Cuando esto sucede, el protocolo puede indicar al origen que el paquete se perdió y está en el origen enviarlo de nuevo
o no (capa que intercede aquí es la de transporte).

# Tasa de transferencia:
Se trata de la velocidad de transferencia en bits que hay entre el emisor y el receptor. Esto puede ser de dos tipos:
	- Tasa de transferencia instantánea: informará esa velocidad en un determinado punto de la red y en un determinado
	  momento.
	- Tasa de transferencia media: informará esa misma velocidad pero en un período de tiempo más largo.

Hay que tener en cuenta que la capacidad que tienen los enlaces de transferir paquete es muchísimo más alta que la que se tienen
dentro del hogar. En este caso la **velocidad de transferencia media** vendrá determinada por la velocidad del cliente (la de los
hosts que tenemos en casa, que es la más lenta). Si en caso de que la velocidad del servidor es la más lenta, entonces será la
velocidad del enlace la que limite o condicione de manera directa sobre la velocidad de transferencia media.
Es decir, que la velocidad de transmisión de un enlace vendrá determinada por la menor de las velocidades de los enlaces a lo
largo del camino, que será quien produzca el cuello de botella.

En el ejemplo del video se dice:
-------------------------------
	- Se tienen 10 clientes (computadoras hogareñas) bajando un mismo archivo.
	- Se tienen 10 servidores donde se hospedan ese archivo.
	- Un mismo enlace por donde pasan estas 10 comunicaciones entre clientes y servidores.

	La tasa de transferencia vendrá determinada por la función mínimo, entre la:
		- Velocidad de enlace del servidor(Rs).
		- Velocidad de enlace del cliente(Rc).
		- Velocidad del propio enlace de R/10 [es el enlace dividido la cantidad de conexiones].

	Lo que sucede es que lo que realmente limita no es tanto la velocidad del enlace (R/10) sino las redes de acceso en las
	terminales de la comunicación (red de acceso del cliente y la red de acceso del servidor).

# Capas de Red
¿Por qué se organiza en capas? Porque ayuda a organizar y a identificar las relaciones de elementos de un sistema complejo como
son las redes de ordenadores. La modularización facilita el mantenimiento y la actualización del sistema. Algunas funcionalidades
se implementan en varias capas y a veces se necesita información para una capa, que se encuentra en otra distinta que la propia,
perdiendo la independencia entre ellas.
Hay servicios que si no están implementados en la capa, se pueden introducir metiendo capas intermedias, como por ejemplo la
seguridad que para ello se mete una nueva capa que utiliza el protocolo SSL.

# Pila de Protocolos de Internet
	1. Aplicación: Es la capa más cerca al usuario y donde se encuentran las aplicaciones de red que intercambian *MENSAJES*.
	   Los protocolos utilizados en este capa son por ejemplo: FTP,SMTP, HTTP.  Esta capa le pasa los mensajes que quiere
	   enviar a su capa inferior.
	2. Transporte: La capa de transporte recibe ese mensaje y mediante un programa, lo convierte en los denominados
	   *SEGMENTOS* pasando estos a la capa inferior. Los protocolos por ejemplo son: TCP o UDP.
	3. Red(IP): La capa de Red de internet se caracteriza por tener un único protocolo, que es el denominado IP (Internet
	   Protocol), el mismo convierte el segmento en *DATAGRAMAS* para enviarlos desde la máquina de origen a la máquina
	   destino.
	4. Enlace: Esta capa, toma el datagrama de la capa superior y la convierte en una *TRAMA* que enviará al siguiente
	   elemento de la red y NO al final, puede ser enviado por 3G, Wifi, WiMax, fibra óptica, o el enlace que sea.
	5. Física: En esta capa se encuentra el enlace físico: Cable o aire, por el que van a viajar los bits.

# Modelo OSI
Antes de existir internet, había un modelo de capa para redes de ordenadores que era el Modelo OSI, donde aparecían dos capas mas
que en el modelo de capas de Internet fueron fusionados: la capa de Presentación (que interpretaba los datos) y la capa de Sesión
(que sincronizaba los datos). Estas capas OSI de más, si se requieren, se suelen incluir en la capa de Aplicación.

# ¿Cómo es el flujo del mensaje?
	- El navegador genera un mensaje HTTP (capa de Aplicación). Y se lo pasa a la capa inferior.
	- La capa de Transporte toma este mensaje, lo encapsula y le añade una cabecera propia para transformarlo en un segmento.
	  Luego lo pasa a la capa inferior.
	- La capa de Red, toma este segmento, lo vuelve a encapsular y le añade una cabecera propia, transformandolo en un
	  datagrama.
	- La capa de Enlace toma nuevamente este datagrama y le añade su propia cabecera para convertirlo en una trama que será
	  transmitida por la última capa (La capa Física).
	- La capa Física es la encargada de transportar esta trama a través de los distintos enlaces hasta el host destino.

Todo este flujo se denomina *encapsulamiento*. Cuando la trama llega al destino, el host que lo recibe debe *desencapsularlo*
(realizando el proceso inverso antes descripto) donde cada capa mira el encabezado que le corresponde, y lo pasa a la capa
superior.

Se debe tener en cuenta que para que dos host que se encuentran *conectados en el mismo enlace* se comuniquen, deberan utilizar
*LOS MISMOS* protocolos para sus mensajes.
Pero lo normal es que los host NO se encuentren comunicados en el mismo enlace, es decir, en la misma red, por lo que habrán
elementos intermedios que ayudarán a hacer llegar ese mensaje. Un Ejemplo puede ser, un host que se quiera comunicar con un
servidor que se encuentra en USA. La computadora puede tener un navegador que se conecta dentro de la red hogareña por wifi, pero
ese router se comunica con toda la Red mediante ethernet.
Por lo tanto los mensajes en internet viajan por los distintos nodos que se irán encargando de mirar las distintas cabeceras para
poder encapsular y desencapsular nuevamente y redirigir así las tramas para llegar al destinatario final.


