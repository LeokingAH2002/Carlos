

Mensajes : 83
Fecha de inscripción : 15/03/2013

Tutorial tcpflow para Kali Linux Empty	
MensajeTema: Tutorial tcpflow para Kali Linux   Tutorial tcpflow para Kali Linux EmptyLun Sep 22, 2014 5:03 am	
¿Qué es el TCPflow?

TCPflow es un programa que captura los datos transmitidos como parte de las conexiones TCP (flujos), y almacena los datos en una forma eficaz para el análisis de protocolos o la depuración. Un programa como 'tcpdump' o 'wireshark' muestra un resumen de los paquetes vistos en el cable, pero por lo general no almacena los datos que en realidad está siendo transmitidos. En contraste, TCPflow reconstruye las corrientes de miles o millones de datos reales y almacena cada flujo en un archivo separado para el análisis posterior..

TCPflow interpreta números de secuencia y reconstruye correctamente los flujos de datos independientemente de las retransmisiones o fuera de la orden de entrega. Sin embargo, en la actualidad no puede con los  fragmentos IP, los flujos que contienen fragmentos IP no se grabarán correctamente, ni con las cabeceras 802.11

TCPflow se basa en la LBL Packet Capture Library y por lo tanto respalda las mismas expresiones de filtración que los programas como 'tcpdump' apoyan. Debe compilarse en versiones más populares de UNIX; consulte el archivo INSTALL para más detalles.

tcpflow almacena todos los datos capturados en los archivos que tienen los nombres de la siguiente forma

            128.129.130.131.02345-010.011.012.013.45103

donde el contenido del archivo de arriba serían los datos transmitidos desde el host origen 128.129.131.131 puerto 2345, hacia el host destino 10.11.12.13 puerto 45103.

¿De qué sirve?

Para capturar los datos que se envían por varios programas que utilizan los protocolos de red indocumentados y realizar ingeniería inversa a ellos. RealPlayer (y la mayoría de los reproductores de medios de streaming), ICQ y AOL IM son buenos ejemplos de este tipo de aplicación.

Para comprobar las cookies que mi navegador estaba enviando a varios sitios, mirar las cabeceras MIME de solicitudes HTTP que personas están enviando a mi servidor web, y verificar que varias conexiones a mi máquina, que se supone que debe ser cifradas, en realidad estaban encriptadas. La comprobación de cifrado de protocolo es particularmente útil para los sitios que llevan los datos de valor financiero en sus cargas útiles, tales como clientes de la banca.

Sintaxis

tcpflow [-chpsv] [-b max_bytes] [-d debug_level] [-f max_fds] [-i iface] [-r file] [expression]
         

       -b: número de máximo de octetos por flujo para guardar
       -c: solo imprime en consola ( no cea archivos)
       -C: solo imprime en consola, pero sin la visualización de cabecera fuente / destino
       -d: nivel de depuración; predeterminado es 1
       -e: salida de cada flujo en colores alternos
       -f: número máximo de descriptores de archivo a utilizar
       -h: imprimir este mensaje de ayuda
       -i: interfaz de red en el que escuchar
           (tipo "ifconfig" para obtener una lista de las interfaces)
       -p: no utilice el modo promiscuo
       -r: leer los paquetes de archivo de salida de tcpdump
       -s: quitar caracteres no imprimibles (cambiar a '.')
       -v: operación detallada equivalente a -d 10
expression: tcpdump-like expresión de filtro

Mirar la página del autor para información adicional.


Ejemplo

Filtrar solo la captura a los puertos HTTP (80), para la interface eth0 (-i) en modo no promiscuo (-p)

tcpflow -p -i eth0 tcp port http


Tutorial tcpflow para Kali Linux Tcpflo10


Los archivos de flujo en realidad contiene los datos extraídos de las sesiones TCP, por lo que no se ven los encabezados de paquetes, etc ...

Tutorial tcpflow para Kali Linux Tcpflo11


La herramienta no puede manejar los fragmentos IP, y en ese caso la reconstrucción de un flujo no será correcta.
