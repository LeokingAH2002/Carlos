darkstat es un sniffer de paquetes que se ejecuta como un proceso en segundo plano, reuniendo todo tipo de estadísticas sobre el uso de la red, y sirve a través de HTTP.

Opciones de arranque

Tal como se mencionó previamente, en "darkstat" hay varias opciones, que se puede proporcionar al inicio. Estos parámetros son:

Con la opción "-i" se puede especificar el interface a monotorizar.

darkstat-i eth1

Si "darkstat" se ejecuta sin parámetros iniciales, el programa abre automaticamente el puerto 667. Se puede cambiar este puerto mediante el parámetro "-p":

darkstat -p 8080

Para ocultar un puerto especifico para un dispositivo concreto, puedes usar la opción "-b". En el siguiente ejemplo , ocultamos la dirección local de "loopback":

darkstat-b 127.0.0.1

Con el parámetro "-n" se puede prevenir el uso de la resolución DNS. Esta opción es útil para gente sin una red de tasa de flujo constante o linea dedicada .

darkstat-n

La opción " -P "Previene que" darkstat "" interfaz Tengo el modo promiscuo ". De todos modos, se recomiendan los aspectos más destacados, ya que " darkstat "Solo capturar los paquetes se dirigen a la dirección MAC de la interfaz de red que esta monotorizado siende. Es otros paquetes son rechazados.

darkstat-P

El parametro "-l" activa correctamente "SNAT"- en la red local. "SNAT" simboliza "Source Network Address Translation" y significa que tu router enmascara la dirección IP local con su dirección pública. Des esta manera el programa envia sus requerimientos , en lugar de los requerimientos originales del cliente.

darkstat-l 192.168.1.0/255.255.255.0

Con el parámetro "-e" se puede ejecutar la expresión para filtra paquetes.

"puerto no 22" darkstat-e

A partir de la versión 2.5 "darkstat" puede funcionar separado del terminal de inicio. Así trabaja como un "daemon".

darkstat --detach

Con el parámetro "-d" se le especifica el directorio donde "darkstat" crea su base de datos.

darkstat -d directorio /

La opción " -v "" activa el modo detallado ":

darkstat -v

Si estas interesado en el número de versión de "darkstat" o en su completo uso y sintaxis, prueba con el parámetro "-h".

darkstat-h

Ejemplo

Lanzamos darkstat con la interface eth0(-i) en el puerto por defecto (667)

darkstat -i eth0

Manual de darkstat para Kali Linux Darkst10

Se accede atraves del navegador por el puerto 667

127.0.0.1:667
