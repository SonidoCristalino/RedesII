# Introducción
Para el correcto abordaje sobre el protocolo RSTP será necesario abordar de forma conjunta el protocolo que se utiliza comúnmente
junto con este, el denominado Protocolo de Tiempo Real (Real Time Protocol) RTP y RTCP (Protocolo RTP de Control).

El protocolo de Transporte en Tiempo Real (por sus siglas en inglés,RTP) es un protocolo de Capa de Transporte para tráfico
multimedia comúnmente utilizado en Internet. RTP, detalla el formato estándar del paquete a utilizar para transmitir audio y video
sobre la red. Es utilizado en sistemas de transmisión de datos multimedia (en conjunto con el protocolo RTCP -
Protocolo RTP de Control), en conferencias de video, o en telefonía IP. Tanto RTP como RTCP, se utilizan sobre UDP para la
transferencia de datos.

Este protocolo puede ser utilizado tanto bajo demanda como por servicios interactivos, como por ejemplo la telefonía IP. El
estándar de RTP actualmente define un par de protocolos de forma simultánea: RTP propiamente dicho, y RTCP. RTP es utilizado para
el intercambio de datos multimedia (audio y video) mientras que RTCP es la parte encargada de controlar el fllujo de datos y que
de forma periódica obtiene información de control sobre la calidad de la transmisión asociada con ellos.

RTP en sí mismo no proporciona mecanismos de entrega en tiempo u otras garantías como Calidad de Servicio (denominadas por sus
siglas en inglés: QoS); para ello se ampara en servicios de bajo nivel (de otras capas) para su implementación.

RTP tampoco garantiza o previene una transmisión de datos en orden (lo que significa que los paquetes RTP pueden viajar
desordenados en todo el camino), y por lo tanto, la confiabilidad de la red subyacente se vuelve incierta. Pero, por otro lado, el
número de serie en RTP permite al receptor reorganizar la secuencia de paquetes en el lado del remitente, y también puede ser
utilizado para determinar la ubicación del paquete apropiado como por ejemplo, en la decodificación de video.

RTP consta de dos partes estrechamente vinculadas: RTP transmite datos con propiedades de tiempo real, y el Protocolo de Control
de RTP monitorea la calidad del servicio y se encarga de transmitir sobre lo que sucede entre los participantes de la sesión.

RTCP es un protocolo para el control de RTP. Este provee control "fuera de banda" para flujos multimedia. Cabe destacar que RTCP
no transmite datos por si mismos, pero colabora con RTP en empaquetar y enviar datos multimedia (audio o video).  RTCP transfiere
de forma períodica datos de control entre los participantes de una sesión multimedia.  El protocolo RTCP tiene como tarea el
proveer de información acerca de la calidad del servicio entregado por RTP y el de sincronizar el audio y el video.

Esto lo hace mediante la recolección de datos, como ser: número de bites transferidos, número de paquetes transmitidos, la
cantidad de paquetes perdidos, la latencia de la red, etc. Muchas aplicaciones utilizan la información suministrada por RPCT para
mejorar la calidad del servicio que brindan. Cabe resaltar que RTCP por sí solo no provee datos de encriptación o de
autenticación. Para esto se puede utilizar SRTP (Protocolo seguro de Transporte en Tiempo Real).


# RTP
RTC se emplea casi siempre bajo el protocolo de transporte UDP/IP y dependiendo de las circunstancias en la aplicación lo
requiera, también puede utilizarse TCP. Tanto RTP como RTCP usan puertos de la capa de aplicación cuando utiliza UDP. La figura a
continuación ilustra de forma esquemática el uso de RTP. El audio y video es digitalizado utilizando un codec particular; una vez
realizado esta codificación/decodificación, cada bloque de bits es encapsulado bajo un paquete RTP y luego bajo un paquete UDP.

El cuerpo de datos perteneciente a RTP provee soporte para aplicaciones con características de tiempo real para transmisión
continua de audio y video, lo que permite la reconstrucción temporal de paquetes, la detección de pérdidas, seguridad e
identificación de contenido. Pero hay que tener en cuenta que RTP no reserva ancho de banda para lo que comunmente se denomina
como "Calidad de Servicio" (QoS, por sus siglas en inglés).

Quien provee esto último es el protocolo RTCP, el cual proporciona soporte para conferencias en tiempo real en grupos de cualquier
tamaño en internet. Este protocolo ofrece información constante sobre la calidad de la transmisión, la cual obtiene de los
participantes a los que se les transmite los datos multimedia. RTCP también mantiene información de todos los que intervienen en
la transmisión.

RTP está diseñado para soportar una gran variedad de aplicaciones. Provee mecanismos flexibles los que permiten que las nuevas
aplicaciones puedan ser desarrolladas sin que tener que ajustarse al protocolo, sino todo lo contrario, es el protocolo quien se
adapta a las aplicaciones. Esto lo hace gracias a un mecanismo por el cual RTP define un perfil y uno o más formatos para la carga
últil del paquete. El perfil provee un rango de información que asegura el común acuerdo entre los campos de la cabecera de RTP
para esa clase de aplicación. La aplicación asimismo, puede definir extender o modificar el protocolo para que se adapte a una
clase especial de aplicaciones.





















