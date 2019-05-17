Cisco torch es una herramienta algo rápida para descubrir hosts remotos que ejecutan Cisco Telnet, SSH, Web, NTP y servicios SNMP y lanzar ataques de diccionario contra los servicios descubiertos.

Sintaxis

cisco-torch.pl <options> <IP,hostname,network>

cisco-torch.pl <options> -F <hostlist>


Opciones

-O <archivo de salida>
-A	Todos tipos de análisis combinados de huellas dactilares
-t	Exploración de Telnetd de Cisco
-s	Exploración de SSHd de Cisco
-u	Exploración SNMP de Cisco
-g	Descarga de archivos de configuración o tftp Cisco
-n	Análisis de huellas dactilares de NTP
-j	Análisis de huellas dactilares de TFTP
-l <type> nivel de registro
c crítica (por defecto)
v detallado
d depuración
-w	Exploración del servidor Web de Cisco
-z	Cisco IOS HTTP autorización vulnerabilidad Scan
-c	Servidor Web de Cisco con la exploración de soporte SSL
-b	Ataque de Diccionario de contraseña (Utilice con -s, -u, -c, -w , -j o -t only)
-V	Salida y versión de la herramienta de impresión


Ejemplos

cisco-torch.pl -A 10.10.0.0/16
cisco-torch.pl -s -b -F sshtocheck.txt
cisco-torch.pl -w -z 10.10.0.0/16
cisco-torch.pl -j -b -g -F tftptocheck.txt
