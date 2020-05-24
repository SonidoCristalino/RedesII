
# Protocolo a utilizar: RTSP

	- Documento RFC:
	  	https://tools.ietf.org/html/rfc2326

	- Definición de Wikipedia:
		https://en.wikipedia.org/wiki/Real_Time_Streaming_Protocol

	- Video explicativo de cómo funciona RTSP:
		https://www.youtube.com/watch?v=tW6MiByt0Mw

	- Video mucho más explicativo y más sencillo:
	  	https://www.youtube.com/watch?v=g99rAG--rIw

	- Explicación en inglés sobre RTP, cómo trabaja y por qué sobre UDP y luego RTCP:
		https://www.youtube.com/watch?v=9dRY2_NmAuc

	- Una introducción muy breve sobre RTSP se encuentra en la página 600 [611] del libro de Kurose.

	- Diferencia entre RTP y RTSP:
	  	https://stackoverflow.com/questions/4303439/what-is-the-difference-between-rtp-or-rtsp-in-a-streaming-server

	- Diferencia entre RTP,RTCP y RTSP [Tiene mucha info]:
	  	https://topic.alibabacloud.com/a/the-difference-between-rtmprtprtsprtcp_8_8_10245114.html

	- PDF con Información sobre RTP, RTCP y RTSP [La introducción se podría sacar de acá]
		https://www.cse.wustl.edu/~jain/books/ftp/rtp.pdf

	- Videos sobre cómo funciona cada uno de los protocolos nombrados anteriormente:
	  	https://www.streamingvideoprovider.com/blog/streaming_video_protocols.html

	- Cómo capturar paquetes en Wireshark sobre RTSP:
	  	https://wiki.wireshark.org/RTSP

	- Trabajo práctico pasado por Guillermo:
	  	http://profesores.elo.utfsm.cl/~agv/elo323/2s14/projects/reports/RiveraBaezCarvajal/Streaming%20RTSP.html
# Kurose:
Para que un usuario pueda controlar la reproducción, el reproductor multimedia y el servidor necesitan un proto­ colo que les
permita intercambiar la información de controi de la reproducción. El Protocolo de flujos en tiempo real (RTSP, Real-Time
Streaming Protocol), definido en RFC 2326, es dicho protocolo. Antes de abordar los detalles de RTSP, veamos primero qué no hace
RTSP.

Por tanto, si RTSP no hace nada de lo anterior, ¿qué hace? RTSP permite que un repro­ ductor multimedia controle la transmisión de
un flujo multimedia. Como ya hemos mencio­ nado, las acciones de control son: pausar/reanudar, reposicionar la reproducción,
avance rápido y rebobinado. RTSP es un protocolo fuera de banda. En particular, los mensajes RTSP se envían fuera de banda,
mientras que los flujos multimedia, cuya estructura de paquete no está definida por RTSP, se consideran “en banda”. Los mensajes
RTSP utilizan un número de puerto diferente, el 544, del empleado por los flujos multimedia. La especificación RTSP [RFC 2326]
permite que los mensajes RTSP sean enviados sobre TCP o sobre UDP.  Recuerde de la Sección 2.3 que el Protocolo de transferencia
de archivos (FTP) también

RTSP es un protocolo fuera de banda. En particular, los mensajes RTSP se envían fuera de banda, mientras que los flujos
multimedia, cuya estructura de paquete no está definida por RTSP, se consideran “en banda”. Los mensajes RTSP utilizan un número
de puerto diferente, el 544, del empleado por los flujos multimedia. La especificación RTSP [RFC 2326] permite que los mensajes
RTSP sean enviados sobre TCP o sobre UDP.
Se denomina *fuera de banda* a los datos que se transmiten por redes paralelas o por fuera de la comunicación establecida; los
datos por fuera de banda que transporta RTSP son aquellos que permiten generar la pausa, atraso o reanudación de los datos
multimedia; en cambio, los datos *en banda* están proporsionados por otros protocolos (como el RTP) que si van por dentro de la
conexión establecida [Quizás algún paralelo se puede establecer con el paralelismo que establece FTP donde por un canal van los
datos de acceso al servidor, y por otro las modificaciones que se les quieran realizar al/los archivos deseados].

[616]:
De hecho, se pueden tolerar perfectamente tasas de pérdida de paquetes de entre el 1 y el 20 por ciento, dependiendo de cómo se
codifique y transmita la voz y de cómo se oculten esas pérdidas en el receptor. Por ejemplo, Los mecanismos de corrección de
errores hacia adelante (FEC, Forward Error Correction) pueden ayudar a ocultar las pérdidas de paquetes. Veremos más adelante
que con las técnicas FEC se transmite información redundante junto con la información original, de modo que una parte de los datos
originales perdidos puede recuperarse a partir de esa información redundante. Sin embargo, si uno o más de los enlaces existentes
entre el emisor y el receptor está severamente congestionado y la tasa de pérdida de paquetes excede del 10 o 20 por ciento
(aunque estas tasas tan altas raramente se observan en las redes bien diseñadas), entonces no hay nada que podamos hacer para
conseguir una calidad de sonido aceptable. Claramente, el servicio de entrega de mejor esfuerzo

## Interworking with TCP
[página 574] Deficinión del protocolo *RTP* y el protocolo de control asociado *RTCP*.

## Tanenabum
[702] Habla sobre RTSP pero mucho más sobre *RTP*.
[551] Sobre *RTP* para el transporte de flujos de audio, video y texto entre otros.
[552] Explica cómo funciona *RTP* y cómo es su flujo de trabajo.

## The illustrated Network
[766] Explica sobre *RTP* (pero no tiene NADA de RTSP).


Lo que dice la respuesta a una pregunta por la diferencia entre RTP y RTSP:

I hear your pain. I'm going through this right now (years later). From what I've learned, you can think of RTSP as a "VCR
controller", the protocol allows you to specify which streams (presentations) you want to play, it will then send you a
description of the media, and then you can use RTSP to play, stop, pause, and record the remote stream. The media itself goes over
RTP. RTSP is normally implemented over a different socket or communication layer. Although it is simply a protocol, most often
it's implemented by a server over a socket. For live streams, the RTSP stream you request is simply a name of a stream. It doesn't
need to refer to a file on the server, the server's RTSP implementation can parse that stream, put together a live graph, and then
provide the SDP (description) for that stream name. But, this is of course specific to the way the RTSP server has been
implemented. For "live" streams, it's probably simpler to just use RTP, but you'll need a way to transfer the SDP from the RTP
server to the client that wants to play that stream.
