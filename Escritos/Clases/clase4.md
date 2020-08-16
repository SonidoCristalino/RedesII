# Clase nº 4:
Todo lo escrito acá se obtiene de: https://www.youtube.com/watch?time_continue=7&v=DxeYtI9tMdw&feature=emb_logo

# DNS: Sistema de nombre de dominios
En internet las máquinas tienen direcciones IP, pero para evitar recordar estos números se emplean nombres de dominio. El DNS o el
Domain Name System, es el sistema que permite almacenar las correspondencias entre direcciones IP y nombres de dominio.
También se emplea el nombre DNS para referirse al protocolo que se emplea para consultar esa base de datos distribuida.
El servidor DNS suele ser máquinas de tipo UNIX que ejecutan BIND sobre UDP utilizando el puerto nº 53.

## Servicios
	- Traducción del nombre del host a direcciones IP.
	- También proporciona alias para esas máquinas.
	- Alias para servidores de correo.
	- También ayuda en la distribución de carga haciendo corresponder un único nombre con varias direcciones IP.

	Todo esto lo hace de una forma transparente al usuario, además se hace de forma decentralizada. Esto es así ya que:
		- Un único DNS sería un único punto de fallo.
		- Con un gran volumen de tráfico
		- Gran distancia a la Base de datos centralizada, al único nodo.
		- Provocaría grandes problemas de mantenimiento.
		- Como también contribuiría a tener problemas de escalado .
	Por lo tanto se creó una base de datos, distribuida y jerárquica en la que los servidores se distribuyen principalmente en
	tres niveles:
		- Raíz
		- Top Level Domain
		- Autorizativos.

	Por ejemplo si queremos solicitar la dirección www.unaj.edu.ar lo que se hace es solicitarla al servidor Raíz, este
	devuelve la dirección del servidor TL Domain .ar, a este le pedimos la IP y nos devuelve la del servidor autorizativo
	unaj.edu.ar y a este servidor se la pedimos nuevamente, y nos devuelve la IP de la máquina www.unaj.edu.ar.

	- Los DNS Raíz son 13 y están replicados y configurados en clusters por todo el mundo.
	- Los DNS TLD : Son los responsables de los dominios de nivel superior y de países: .com, .org, .ar, etc.
	- Los DNS autorizativos son los que tendrán los registros DNS de los host que están accesibles al público, como los
	  servidores webs o el correo.
	- Por último encontramos a los servidores DNS locales o servidores de nombres predeterminados, que son implementados por
	  cada ISP y NO pertenecen a la jerarquía antes descripta, son a los que primero pedimos a una traducción IP y son los
	  encargados de buscarla, por la jerarquía DNS. Tienen una caché por lo que de cada búsqueda que hacen, se quedan una
	  copia, por lo que si se vuelve a pedir, no tienen que volver a buscarla.
	  Los registros DNS almacenados en estas caches expiran al cabo de un tiempo y hay mecanimsmos de actualización y
	  notificación para mantener la información al día.

Es habitual que la información sobre los servidores TLD esté cacheada en nuestro DNS local y no se hagan muchas consultas a los
servidores raíz preguntando por los TLDs.

## Ejemplo de Resolución de nombres:
	Por tanto la consulta DNS más habitual es la consulta interactiva en la que delegamos el nuestro DNS local y este es el
	que va buscando primero en el raíz y luego en el TLD y por último en el autoritativo hasta que consigue el registro DNS
	con la dirección IP que nos devuelve.

## Otro ejemplo:
Otra opción sería que me den ese local hiciera una consulta recursiva en la que pide al DNS raíz que haga toda la consulta por él
y así sucesivamente. Este tipo de consultas no se emplea porque cargaría en exceso a los servidores raíz y TLD.

Los registros DNS o Research Récords serán los que se almacenen en los servidores DNS y no son más que una tupla con un nombre un
valor un tipo y un tiempo de vida o TTL indicando el tiempo que se debe almacenar este registro en una caché.

Hay varios tipos de registros DNS pero los más comunes son los del tipo A en el que el campo nombre tendremos el nombre del
host y en el campo valor su dirección IP. Los de tipo NS los cuales en el nombre estará el nombre de dominio y en el valor el nombre de un
servidor DNS autoritativo para ese dominio.
O los de tipo CNAME en el nombre está el alias para un host cuyo nombre canónico es el que figura en el campo valor.

El de tipo MX que es como un CNAME me pero para servidores de correo electrónico

Si un servidor es autoritativo de un host tendrá un registro tipo A para ese host indicando su IP, si no lo es lo podría tener en
su caché o tener un registro DNS con el nombre del servidor DNS que tenga el dominio del host y otro registro tipo A con la IP de
ese servidor.

Tenemos entonces que DNS es una base de datos distribuida que consultan los equipos empleando un protocolo. Veamos ahora cómo son
los mensajes de ese protocolo DNS.

## Mensaje DNS
Tendremos dos tipos de mensaje: para hacer las consultas y para responderlas; pero ambos con el mismo formato. Tienen una cabecera
con un número de identificación de 16 bits y una serie de indicadores que dirán si
	- Se trata de una consulta de una respuesta
	- Si se desea emplear repercursión o
	- Si se soporta esa recursión
	- Si se trata de un dns autoritativo

Después vienen cuatro campos que nos van a indicar
	- El número de consultas
	- El número de registros de respuesta
	- El número de registros autorizativos
	- El número de registros adicionales que lleva ese mensaje

De esta forma, tras estos cuatro números vendrán esas
	- Consultas
	- Respuestas
	- Registros autorizativos
	- O registros adicionales Que se devuelven

## Ejemplo
Como ejemplo podemos ver este mensaje de respuesta que manda un servidor DNS local con el registro tipo A con la dirección IP del
servidor web de la Universidad de la Sorbona. Vemos que me responde en un datagrama UDP desde su puerto 53, con un mensaje DNS con
el formato visto antes:
	- Un campo de identificación
	- Unos indicadores que dicen que se trata de una respuesta a una consulta recursiva que es
	  soportada por el servidor (nuestro servidor DNS local normalmente siempre le vamos a hacer consultas recursivas y él las
	  hará iterativas con el resto de servidores).

	- Los campos siguientes nos indican que este mensaje lleva una consulta a la que está respondiendo
	- No lleva registros autoritativo ni registros adicionales

Se puede ver que la consulta en una consulta tipo A, que se quiere la IP de la máquina de nombre www.sorbonne-university.com y al
final del mensaje está ese registro tipo A con la IP de la máquina y con un tiempo de vida de 43.200 segundos

# Parte nº 2:
Todo lo escrito acá se habla en: https://www.youtube.com/watch?time_continue=43&v=YTYe2XPHrWQ&feature=emb_logo

## Funcionamiento de DNS:
Existen unos servidores DNS raíz o Root, que en este caso se tratan de 13 servidores que en realidad son granjas de servidores. Se
encuentran con una alta replicación para tener una alta disponibilidad y estos 13 servidores están distribuidos por todo el mundo.
Hay una alta concentración en Norteamérica pero también hay servidores en Europa y en Asia.

Cada uno de ellos, están configurados en un clúster del servidor, por seguridad y fiabilidad. A estos servidores son
raramente consultados gracias a la caché que existen en los servidores de niveles anteriores, por tanto, no se les consulta
bastante o no tanto como se pensaría. Los servidores TLD (top level domain) son los responsables de los dominios de nivel superior
son los responsables de los niveles .com .org .net .edu etcétera, además de los dominios de países como .es, .uk, .fr, etc

El siguiente nivel de servidores, son los servidores DNS Autoritativos. Estos servidores son mantenidos por organizaciones que
tienen los hosts que quieren que sean accesibles públicamente pues deben publicar esa dirección IP del servidor que tienen de la
máquina que tiene junto con el nombre y esa pareja nombre e IP deben publicarla en un servidor DNS autoritativo. Por lo tanto son
o bien mantenidos por esta organización o bien pueden contratar los servicios de un proveedor externo que les provea de este
servicio de DNS. Por lo tanto serán estos DNS autoritativo los que finalmente tengan el registro tipo A que relaciona una IP con
un nombre de una máquina. Los servidores DNS locales no pertenecen estrictamente a la jerarquía, es decir, cada ISP (servidor de
proveedores de servicios de internet) ya sea doméstico, empresa, universidad. etcétera. tiene un servidor DNS local (también se llama
servidor de nombres predeterminado), por lo tanto cuando una máquina va a pedir una IP para un determinado nombre, se va a
conectar este servidor DNS local. Va a actuar por lo tanto como si fuese un proxy, por ejemplo: si la IP ya la tiene pues la
devuelve y si no, envía ya la petición a toda la jerarquía de servidores DNS que hemos visto anteriormente por lo tanto este
servidor DNS local va a ser el primero en ser consultado por una máquina para consultar la IP de un nombre determinado.

# Ejemplo de resolución de Nombres
Por ejemplo si una máquina quiere saber la IP de un servidor web de la universidad, este servidor es una máquina determinada que
se llama www.google.ar (que está dentro de un dominio que es google.ar) y este dominio está dividido en dos
zonas:
	- google y
	- .ar
Podemos hacer consultas o bien interactivas o bien recursivas. Una consulta interactiva sería la máquina solicita la dirección IP
de la máquina web google.com a su DNS local. En ese DNS local la maquina solicitante lo conoce o bien porque se ha introducido
manualmente, o bien porque se lo han facilitado gracias a una consulta al servidor DHCP cuando le pidió la dirección ip para poder
introducirse en internet esta máquina solicitante.

Esta máquina solicitante conoce cuál es su DNS local, conoce su dirección IP y puede consultarle. Le primero consulta cuál es la
dirección IP de la máquina www.google.ar, si este DNS local tiene esa dirección IP porque lo tiene en una caché almacenada se la
devuelve inmediatamente y le devuelve la IP y problema resuelto.
Si no tiene esa dirección IP, esa correspondencia entre dirección IP y nombre, este DNS local va a ser el encargado de ir realizando
diversas consultas a los distintos servidores DNS para conseguir esa IP. Esto por tanto serían consultas iterativas,
en las cuales este servidor DNS local va a ir interaccionando con el resto de servidores. Esta primera consulta que ha realizado el
solicitante al DNS local sería una consulta recursiva el cual el delega a todas las funciones de búsqueda en el siguiente
servidor, por lo tanto, lo habitual es esta unión entre esta primera consulta que es recursiva con el resto de consultas que
va a hacer el DNS local que son iterativas.
¿Como las hace? Pues bien si el DNS local no conoce la IP de esa máquina, lo que puede hacer es preguntar al DNS raíz cuál es la
IP de esa máquina y el DNS raíz no la va a conocer, lo que va a conocer va a ser la IP de los servidores Top Level Domain (TLDs)
que tengan en este caso la dirección IP de los servidores DNS que saben de los dominios TLD, en este caso .ar.

Por lo tanto el DNS raíz de va a contestar a mí de ese local diciendo "la IP de esa máquina no la conozco pero sé que esa máquina
está dentro del dominio punto es y entonces te devuelvo los servidores TLD qué saben las IPs que están dentro de ese dominio .ar"
El DNS local va a consultar a ese servidor TLD que tienen las máquinas del dominio .es y el servidor DNS le puede decir si tiene
en su caché la IP de esa máquina, si la tiene se la puede devolver y si no, le devolvería la IP de la máquina que lleva los
registros de direcciones IP de google que cae dentro de su dominio .ar.

Por lo tanto en este ejemplo, le devolverá google.ar, le dirá la IP del servidor DNS que tiene las IP desde ese dominio, por lo
tanto finalmente el DNS local puede preguntarle a ese servidor DNS (el cual es autoritativo de ese dominio) es decir va a tener
los registros que hacen corresponder el nombre de máquina con la IP en este caso le va a preguntar por la máquina www que cada dentro
de su dominio y le devuelve ese registro porque al ser un servidor DNS autoritativo tiene los los registros de las IPS de todas
las máquinas que caben dentro de ese dominio.  Por lo tanto le devuelve finalmente la IP www.google.ar y el dns local se la devuelve
finalmente al solicitante como había pedido al inicio.

La otra opción sería hacer consultas recursivas, en las cuales ya no solo se le pregunta al DNS local para que haga la búsqueda,
sino que en ese local también delegaría al DNS raíz para que este sea el que pregunte al TLD y este, a su vez, sea el que pregunte
al autoritativo. El camino de vuelta también pasaría por todos los servidores. Aunque esto sería una carga excesiva para los
servidores (en este caso para los raíz por ejemplo), por lo que estas consultas se desestiman.

Gracias al almacenamiento que existe en las distintas calles de los distintos servidores dns estos van almacenando las
correspondencias entre nombre e IPs que van consiguiendo y de esta forma van a reducir muchísimo los retardos y los mensajes de
DNS que se transmiten por la red. Esa caché la van a descartar al cabo de un tiempo y por lo tanto se irá actualizando a medida
que se vayan descartando las correspondencias los registros nombre-IP que vayan quedando obsoletos

Esos mecanismos de autorización y de notificación actualmente están siendo rediseñados, pero se tarda un tiempo considerable en ser
propagadas los cambios por la red. Se trata que estos tiempos sean menores cuando hay una nueva correspondencia entre un Nombre y
una IP. Cuando sucede esto, como es una base de datos distribuida estas nuevas correspondencias van siendo propagadas y tardan
un tiempo considerable, lo que se busca, son nuevos mecanismos para poder actualizar la información que ya existe.

Los servidores TLD por ejemplo habitualmente están ya en las cachés de los DNS locales y ya sabe cuál es el servidor para el .ar
.net, etc ya que eso lo han consultado cientos de veces al cabo del día entonces esa consulta ya la tiene
en su caché, no tiene que repetir y hacérsela al servidor raíz. Por lo tanto los servidores raíz como se comentó al principio no
son muy visitados. Los servidores TLD esa información ya la tienen

## ¿Como son todos estos registros DNS?
Al final los registros DNS no son más que pues cuádruplas que se denominan "Resource Records", RR, del nombre, el valor, luego
el tipo de registro que es y un tiempo de vida.

	* Formato RR(Nombre, valor, tipo de registro y TTL (tiempo de vida))

Los registros más típicos son el tipo A que es el que hace una correspondencia entre el nombre de la máquina y la IP. En el
nombre tendrá el nombre de la máquina y y en el valor tendrá la ip de la máquina.
Estos son los registros que más se consultan.

También se puede solicitar un registro tipo NS o "Name Server" que el nombre se tendrá el dominio, por ejemplo:  google.com y el
valor se tendrá el nombre de un DNS autoritativo para ese dominio, es decir, devolverá un nombre de un servidor DNS que es
autoritativo para ese dominio, es decir, devolverá un nombre de un servidor DNS, que es autoritativo para ese dominio, y que tiene
los registros que corresponden a las máquinas de ese dominio. Por lo tanto, si se quiere saber qué servidores DNS tienen el dominio
google.com, se puede realizar una consulta tipo NS.

También hay consultas tipo "canonical name" (CNAME), que son consultas o registros (se realiza una consulta y se devuelve un
registro de este tipo) en el que este registro en el nombre va a figurar un alias para un nombre canónico que se le indica en el valor
por ejemplo, el nombre canónico es la máquina real, digamos www.ibm.com, el cual es un servidor que se llama
con ese nombre, este es servereast.backup2.ibm.com, por lo que cabe resalta que NO es www, ya que eso es un alias
realmente la máquina que está sirviendo será la otra (servereast.backup2.ibm.com), es por ello que si se hace la consulta CNAME en otro
sitio, a lo mejor hay otro servidor que está más cercano y le va a contestar con otro nombre, porque es otra máquina.

Por último, tenemos registros tipo MX en los cuales se va a tener en el valor el nombre canónico del servidor de correo que se
corresponde a ese dominio que se ingresará en el nombre. Por lo tanto me va a dar información sobre el servidor de correo que se
corresponde al dominio que le estoy pidiendo.

# Aplicaciones Peer To Peer

Todo esto es sacado de: https://www.youtube.com/watch?v=j9qVIgqBIzg&feature=youtu.be

Como vimos en el tema de introducción las arquitecturas Peer-To-Peer puras no necesitan de servidores y los host directamente se
conectan entre sí haciendo a la vez del Cliente y de Servidor. Estos se denominan pares a los host, se van a conectar de forma
intermitente y van a cambiar su IP por lo tanto es una estructura muy dinámica

Vamos a estudiar esta arquitectura Peer-To-Peer mediante dos ejemplos:
	- El primero de ellos es la distribución de ficheros con el ejemplo de bittorrent y
	- El segundo es un ejemplo de voz sobre IP que es la aplicación SKYPe

La distribución de ficheros vamos a ver cómo podemos comparar entre una arquitectura cliente servidor y una arquitectura peer tu
piel. Cuando queremos desde un servidor distribuir un fichero n-máquinas o host ¿Cuánto tiempo se tarda para distribuir este
fichero utilizando una arquitectura cliente servidor o utilizando una arquitectura peer-to-peer?

Veamos la comparativa en el escenario tenemos que el fichero tiene un tamaño F, que el servidor tiene una velocidad de subida del
fichero (el servidor no se lo va a descargar porque ya lo tiene entonces esa velocidad de descarga no nos interesa) la velocidad de
subida del enlace del servidor a la red es Us y luego cada uno de los host tiene una velocidad de subida U1, U2, Un...
y una velocidad de bajada (de download) D1, D2, Dn... por lo tanto veamos el tiempo de distribución en una arquitectura
cliente-servidor.

## Arquitectura Cliente-Servidor
En arquitectura Cliente-Servidor, cada uno de los clientes van a solicitar el fichero al servidor y se lo van a descargar, por lo
tanto el servidor tendrá que enviar secuencialmente n copias por ese enlace de subida, por lo tanto el tiempo que va a tardar va a
ser el tiempo que tarda de enviar esas n copias de tamaño F por un enlace Us.
Pero también podemos tener que otro factor limitante en el tiempo de distribución sea la velocidad de descarga del más lento de
los clientes.

	- Por lo tanto tendremos que mirar las velocidades de descarga de cada uno de los clientes y ver cuál de ellas es la
	  mínima pues la mínima de ellas será el cliente que más tarde en descargarse ese fichero por lo tanto tendremos que ver
	  qué es lo que tarda más sí el tiempo que tarda el servidor en distribuir las n copias o el tiempo que tarda el cliente
	  más lento en descargarse ese fichero. El máximo de estos dos valores será el tiempo de distribución en una arquitectura
	  cliente servidor.

Este valor vemos que incrementa con una n bastante grande linealmente con n el tiempo por lo tanto es un tiempo de
distribución bastante alto.
Si miramos la arquitectura peer-to-peer en este momento el servidor no hace falta que envíe las n copias a cada uno de los
clientes porque ahora el servidor (mal llamado servidor en una arquitectura abierta), el el par que tiene el fichero
originalmente va a enviar una única copia de ese fichero, lo va a subir y luego los demás peers, los demás padres se encargan de
distribuirse ese fichero que ha sido subido a una única vez. Por lo tanto, cada uno de estos clientes va a descargarselo a una
velocidad Dy y pasará igual que antes, tendremos que el mínimo (es decir, el cliente más lento en este caso el par más lento) va a
marcar uno de los límites del tiempo máximo de distribución, al igual que el tiempo de su vida de ese fichero pero ahora sin
multiplicar por N porque eso lo subimos una vez. Pero también tenemos que nos puede limitar el tiempo la velocidad de
subida de los demás pares que para subir las n copias ahora vamos a utilizar no sólo la velocidad de subida del servidor sino
también las velocidades de subida de los demás pares, por lo tanto, para subir las n copias vamos a ver que el tiempo que se tarda
en subir esas n copias ahora es, no solo con la velocidad subió al servidor sino también la suma de los enlaces de subida de los
demás pares. Por lo tanto este tiempo también podría ser limitante.
Calculamos el máximo de todos estos valores si se basa el tiempo de distribución de un fichero en una arquitectura peer-to-peer.

	- La Velocidad de servidor de subida es normalmente menor que las velocidades de descarga mayor que las pérdidas de subida
	  de los clientes y una velocidad de descarga de los clientes mayor normalmente que la velocidad de subida del servidor

Pues bien, la arquitectura peer-to-peer como se puede ver en la gráfica, los tiempos siempre estaban por debajo del incremento
lineal que supone una arquitectura Cliente-Servidor según el número de pares sobre la cual se quiera realizar la distribución.

## Bit Torrent
Esta popular aplicación debe su nombre a que cada vez que queramos compartir un archivo, por cada uno de ellos que se comparte se
va a generar un torrente, este torrente se puede definir como el grupo de peer que intercambian fragmentos de ese fichero
determinado.

## Historia
En los inicios este tipo de aplicaciones no era del todo distribuida, no era del todo peer-to-peer, sino que era mixta porque ya
existía una especie de servidor central (por ejemplo con napster que fue una de las primeras aplicaciones de este tipo) con una
base de datos centralizada. Este tracker, tiene registro de todos los peers que participan en el torrent. Por lo tanto cada vez
que un peer se une torrente es decir pide solicitar descargar por ejemplo un fichero o subir un fichero, se va a registrar en ese
tracker y periódicamente estará informándole que sigue en el torrente.
El tracker por tanto va a llevar una lista actualizada de los distintos peers que tienen un determinado fichero. Estos ficheros
para compartirlos se dividen en fragmentos de 256K hasta un mega, y esos fragmentos son los que se van a intercambiar. Cuando un
peer se une a un torrente él no va a tener fragmentos pero los tendrá según los vaya descargando y el tracker lo que le va a
asignar va a ser de forma aleatoria un conjunto de pares y le da las IPs de esos pares al nuevo peer que se acaba de conectar.
Este peer lo primero que va a hacer va a ser intentar conectarse con todos ellos pero normalmente no todos van a responderle sino
únicamente un subconjunto. Este subconjunto que le responde vamos a denominar vecinos de ese peer.

Mientras se descarga el fichero cada pie va a ir cargando fragmentos en otros pares, además algunos abandonarán otros aparecerán
nuevos y se van a ir estableciendo nuevas conexiones TCP para estas estas descargas y subidas de ficheros. Una vez que el peer
obtiene el fichero completo puede abandonar el torrente (eso se denomina una postura egoísta ser un leaching) o también puede
permanecer en el torrente y aunque ya tenga el fichero completo seguir compartiendo para que otros lo obtengan de forma altruista.

## ¿Cómo se actualizan los fragmentos?
Pues en cada instante cada peer tiene diferentes subconjuntos de fragmentos del fichero, entonces periódicamente el peer va a
pedir a sus vecinos la lista de los fragmentos que tiene, esto de forma periódica y va a mirar de esos fragmentos cuál es el que
no tiene, ¿cuál va a pedir primero de todos los que no tienen? El menos común evidentemente el más raro va a ser el primero que le
interese obtener, por si acaso ese peer que tiene ese fragmento se desconecta y nunca se vuelve a conectar por lo menos ya tiene
un fragmento de los raros. El envío de fragmentos sigue una filosofía que se denomina 'tit for tat' o 'toma y daca' que consiste
en que el peer envía fragmentos a los cuatro vecinos que actualmente le envían fragmentos a mayor velocidad, es decir, va a mirar
de todos los vecinos qué cuatro vecinos le envían fragmentos a mayor velocidad, y esos cuatro vecinos los va a ir reevaluando cada
diez segundos, cada diez segundos mira a ver qué cuatro vecinos le envían a mayor velocidad y a esos cuatro él le envía fragmentos
es decir, "si tú me compartes yo te comparto".  Esto quizás no daría cabida a vecinos nuevos, entonces para poder dar cabida a
nuevos vecinos cada 30 segundos se va a seleccionar de forma aleatoria un nuevo vecino y, a ese vecino se le va a enviar
fragmentos, y así ese vecino a lo mejor puede entrar en el top 4.

# ¿Cómo funciona?
Esto se denomina un "Optimstically unchoke". ¿Cómo funciona? Imaginaos que Alicia no está filtrando de esta forma optimista como
decíamos a Bob, entonces Alicia se convierte en uno de los primeros top 4 proveedores de Bob, le está dando le está dando
fragmentos y Bob la selecciona como uno de los cuatro más rápidas, entonces bob empieza en ese momento a enviarle datos también a
Alicia. Si la velocidad de voz es bastante buena también podría entrar en el top 4 de Alicia y por lo tanto establecer una
comunicación fluida. La consecuencia de esto es que los pies que suministran datos a velocidades similares tienden a emparejarse en
Bittorrent.

Como se dijo antes, Bittorrent no utiliza un tracker centralizado sino que utiliza lo que denominamos tablas hash distribuidas, es
decir, que son bases de datos por decirlo de alguna manera, que van a contener pares clave-valor, en nuestro caso van a contener
el nombre del fichero y la dirección IP que tiene y así, el tracker va a poder saber que un fichero determinado, lo tiene
tiene el ordenador 210.4.7.2. Entonces los pares van a consultar a esta base de datos distribuida consuntándole por este
esta clave y el valor e irá obteniendo los datos de cada torrente.

## Skype
En cuanto al caso de estudio de Skype, una aplicación de voz sobre IP, es una aplicación Peer-To-Peer.
Lo que sucede es que es una aplicación que tiene un protocolo propietario por lo que no podemos examinarlo en detalle.
Simplemente se observa que en skype se introduce un índice que va asignar los nombres de usuario de Skype a IPs, y este inicio
está distribuido en super-pares (probablemente con una tabla hash distribuida como en BitTorrent) pero no tendremos un servidor de
login y tendremos estos super-pares, por lo tanto es una arquitectura Peer-To-Peer mixta, con nodos distintos con más privilegios
que otros.

