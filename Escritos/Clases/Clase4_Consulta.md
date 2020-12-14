# Clase nº 4 : 16/05/2020

Para esta clase mejor ver las filminas, que son más "entendibles".

## DNS:

[Imagen] De las preguntas del cuestionario.
[Imagen2]: Es un gráfico sobre el que se hace pregunta 4 y 5 del cuestionario.

Domain Name Systema (Sistema de dominio).

Es el listado que se mantiene en servidores que machean direcciones con

¿Por qué conviene centralizar los servicios de DNS?
Porque tendríamos un único punto de falla del volumen hacia un sólo servidor sería muy lento y la problemática que conlleva.

[Se corta porque el profesor no tiene buena internet]

# Retomamos el tema a la clase siguiente: 23/05/2020
Servidor DNS:
	Pasaje de resolución de nombres a direcciones IP

Servidor Autoritativo:
	- magip.com.ar , para sacarlo público se tienen que publicar los servidores DNS que vayan a solucionar esas consultas.
	- Los .com, .ar los resuelven otros servidores.

Del local para arriba es iterativa la consulta. El profesor configuró su conexión para saltearse el router, que su router hace
como servidor DNS, configuró el servidor DNS de Google. Para que directamente le pide las resolución DNS y no pase por el servidor
de servicio de Internet.
Los DNS raíz o root son 13, y la mayoría lo reciben los yanquis. Asocian su IP con las búsquedas que realiza el usuario.

Utilizar OPEN DNS que están en otros lugares, y no espían.

Las consultas recursivas cargan mucho el Servidor Local. Las que se utilizan en internet son las iterativas, sólo los servidores
locales tienen configurado las recursivas para las consultas internas.
ICANN es la corporación que controla el dominio de cada una de las páginas.

# Registros DNS

A: hotname a una IP. Se utiliza para darle un nombre a una máquina, nombre canónico se dice. Si el servidor tiene 30 páginas
hospedadas,se le hacen un CNAME, que apunten al primero.
Se utiliza la A para no tener que cambiar IP, cada vez que hay que cambiar la IP o cambiar otra cosa, hay que hay que hacer 30
cambios de registros, en caso con CNAME, lo que permite es que haya sólo un cambio.

MX = Nos dicen qué nombre de servidor manejará determinado correo.

Los servidores Raíz devuelven la consulta el Name Server asociado a la consulta realizada.

nslookup (aplicación de Microsoft):
	Es una aplicación de consola interactivo.
	Informa el servidor predeterminado que nos va a contestar las consulta y su dirección. [imagen]
[imagen] Se ve en los registros tipo : txt
	Que se visualizan las direcciones IP por donde salen los correo, no sólo por dónde se reciben.

# Arquitectura P2P
Comparando P2P con Cliente-Servidor. Una vez que los archivo sse carga, lo tiene todos los clientes, el que lo descarga/carga más
lento, entonces ese es el que lidera el tiempo de distribución.
Cuando se descarga, lo que se hace es buscar entre 4 vecinos y ver quién es más rápido, y cada x tiempo se agrega otro y se vuelve
a sopesar el tiempo y así se queda con el más rápido.
Lo que se hace es bajar siempre el pedazo de archivo menos compartido.

## DHT
Ver en la filmina que está meor explicado.
DHT base distribuidas en los clientes. Datos que son en pareja clave-valor, busco con una clave y me devuelve un valor.

Saber la dirección IP de todos los host, es mucha información que se tenga que guardar, por eso es que se hace una Tabla hash para
poder mantenerlas todas.

# Skype
p2p porque la comunicación general se hace entre clientes.
Se introduce el supernodo, para evitar que se corten los firewalls cuando hay comunicación.
