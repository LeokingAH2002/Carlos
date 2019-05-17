Cisco auditing tools en un script en Perl que analiza routers cisco para las vulnerabilidades comunes. Comprueba si hay contraseñas por defecto, los nombres de comunidad fáciles de adivinar, y la historia de errores IOS. Incluye soporte para plugins y escaneo múltiples hosts.

Sintaxis

cat [opciones] {host} [opciones]

Opciones

-h hostname (para escanear un solo host)
-f hostfile (para el análisis de múltiples hosts)
-p port # (Puerto, por defecto 23)
-w wordlist (lista de palabras para conjeturar el nombre comunidad)
-a passlist (lista de palabras para adivinar la contraseña)
-i [ioshist] (Busque el error de la historia de IOS)
-l logfile (para acceder a la pantalla por defecto del archivo)
-q quiet mode (no hay salida de pantalla)


Ejemplos

Trata de adivinar la contraseña

cat -h 192.168.1.50 -a ./lists/passwords
