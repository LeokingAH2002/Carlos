Hashcat es la herramienta de recuperación de contraseñas más rápida del mundo basada en CPU. No tan rápida como sus contrapartes Oclhashcat-plus,oclhashcat-lite, largas listas pueden ser reducidas a la mitad con un buen diccionario y con un poco de familiaridad con los vectores de ataque. El objetivo de esta sección es presentarle todo lo que tiene que saber para poder para comenzar a correr de manera exitosa hashcat.

Usando las herramientas suministradas en hashcat hagamos una prueba rápida:

Tutorial hashcat para Kali Linux Hashca10

Aquí le hemos dicho a hashcat cargar la lista de hashes A0.M500.hash y usar el diccionario de ejemplos A0.M500.word. Esto debería de darnos un 100% de éxito ya que los hashes UNIX fueron encriptados de las cadenas de texto en el diccionario de ejemplo. Si usted está usando Windows asegúrese de correr hashcat-cli(32/64).exe y omita él “./”.

Tutorial hashcat para Kali Linux Hashca11

Éxito total: D. Todos los hashes recuperados!!! Sencillo no? Pues se pondrá mejor.........


OPCIONES

Hashcatcli tiene una gran cantidad de opciones que puede usar para afinar sus ataques.

Tutorial hashcat para Kali Linux Hashca12

Si, lo sabemos el número de opciones puede parecer intimidante al inicio, pero esto es solo impacto visual, una vez que usted se acostumbre, serán fáciles de recordar. Muchas de las opciones se explican por sí mismas, así que tómese el tiempo de leer cada una.

Explicaremos las opciones vitales para poder correr hashcat y el resto las explicaremos mas adelante con más detalle.

--hash-type=NUM O –m: El tipo de algoritmo a usar que puede ser MD5, SHA-1 etc.

--hash-type=0 (MD5) O –m 0

--attack-mode=NUM O -a: El tipo de ataque a usar en contra de un hash. Usando los diferentes vectores de ataque aumentaran las probabilidades de recuperar la contraseña. Los modos son los siguientes:

 0 = Straight- simplemente corre todas las palabras del diccionario en contra de la lista de hashes, teniendo un buen diccionario aumentara las probabilidades de recuperar tu hash.
 1 = Combination – combina las palabras del diccionario dado.
 2 = Toggle-Case –Cambia todas las letras minúsculas a mayúsculas y viceversa. Dígitos y caracteres especiales son ignorados.
 3 = Brute-force – Fuerza bruta debe ser usado como último recurso, no es efectivo contra contraseñas largas y puede consumir mucho tiempo.
 4 = Permutation – Toma las letras de una palabra y las reordena. Ejemplo abc se vuelve abc, acb, bca, bac.
 5 = Table-Lookup –Rompe una cadena en caracteres individuales y aplica una regla a cada uno que coincida con la regla en la tabla.
Por defecto el modo 0.

--attack-mode=0 O –a 0 para un simple ataque de diccionario.

--seperator=CHAR O -p: Algunas listas de hashes se pueden dar junto con el usuario y su pass encriptado user:33c2a20e201110df4cc723c0a994b4ff en este caso el : es nuestro separador, pueden usarse otros pero por defecto se usa siempre “:”.

--output-file O –o: especifica donde los hashes rotos serán escritos. Esto debe ser usado si usted planea conservar estos hashes o no desea copiar/pegar desde la terminal. Trabaje inteligente y no más complicado. Por defecto no usado.

--output-file=/user/Desktop/hashes.txt O –o /user/Desktop/hashes.txt

--output-format=NUM : NUM puede ser 0, 1 o 2. Generalmente no se necesita pero si en el texto plano hay caracteres en hexadecimal esto necesita ser especificado para prevenir “malos” textos planos. Por defecto Modo: 0.

--output-format=0

--remove : Con esta opción se borrara el hash de la lista una vez que sea crackeado. Esto le ayudara a prevenirle de atacar el mismo hash dos veces. Por defecto no usado.

--stdout : En lugar de tratar de recuperar las contraseñas hashcat simplemente les dará salida en la terminal.

--disable-potfile : Previene que hashcat escriba los hashes rotos en hashcat.pot . Por defecto no usado.

--debug-file=FILE : especifica el archivo en donde la información de depurado será escrita. Por defecto no usado.

--debug-file=/user/Desktop/debug.txt

--debug-mode=NUM : Escribe bien la norma de búsqueda, la palabra original, o la palabra mutada que fue exitosa en contra del hash usado en –debug-file=. Por defecto no usado.

--salt-file=FILE O –e: Especifica una lista de “salts” pre-generados para ser usados en una sesión. Esto se usa cuando en un hash con “salt” el “salt” esta ausente. Por defecto no usado.

--segment-size=NUM O –c : Especifica la cantidad de memoria en MB que se permitirá en el almacenamiento en cache para el diccionario. Si usted trabaja con una cantidad limitada de memoria deberá ser usado para no interferir con los otros servicios. Con la siguiente opción solo se permitirán 10MB de palabras en cache. Por defecto 32 MB

-c 10

--threads O –n : Para uso en procesadores “multi-threaded”. Casi todos los procesadores contienen múltiples núcleos. Si usted tiene un procesador “quad” o de cuatro núcleos entonces, ajuste a –n 4 o –n 6 para uno de seis núcleos. Si usted corre un sistema multi-procesadores, ajuste –n al número de núcleos * numero de procesadores físicos. Por ejemplo un sistema Dual hexacore seria 6 (núcleos) * 2 (procesadores)= 12 (numero de threads) por defecto: 8.

-n 12

--words-skip=NUM O –s : Salta el numero provisto de palabras cuando se resume una sesión detenida. Esto previene de correr palabras contra una lista de hashes de nuevo con lo cual se incrementaría la cantidad de tiempo en un instancia podría tomar. La siguiente opción brincara las primeras 100000 palabras. Por defecto no usado.

-s 100000

--words-limit=NUM O –l : especifica el numero de palabras que serán procesadas. Esto es útil cuando se recupera la misma lista de hashes en diferentes computadoras así la misma computadora no corre palabras que están siendo procesadas por otra. La siguiente opción solo usara las primeras 20000 palabras. Por defecto no usado.

-l 20000

--rules-file=FILE O –r : Especifica el directorio donde se encuentran los archivo de reglas(más adelante se explicara a detalle)

-r /user/Desktop/1.rule

--generate-rules=NUM O –g : Le dice a hashcat que genere un numero de reglas para aplicarse en cada intento, la opción –g 50 le dirá a hashcat crear aleatoriamente 50 reglas sobre la marcha para ser usadas en esa sesión. Esto puede eliminar la necesidad de largos archivos de reglas, aunque un buen archivo de reglas bien creado puede aumentar las probabilidades de recuperar la contraseña. Por defecto no usado.

-g 50

--generate-rules-func-min/max=NUM : Especifica el numero de funciones que deben ser usadas. Este número puede ser ilimitado pero largas cantidades no se recomiendan. Cuando se usa en conjunto con –g, cualquier regla fuera de este ajuste será ignorada. Por ejemplo –g 50 genera 1 r, 1^f y sa@ todas estas son reglas validas sin embargo 1^f sa@ r $3 será ignorado porque contiene 5 funciones. Por defecto min=1 max=4.

--custom-charset1,2,3,4=CS O -1,-2,-3,-4 : Este es un mapa de caracteres a medida, todos lo derivados de hashcat tienen 4 para crear tu mapa de caracteres a medida. Estos nos sirven en el modo 3 o bruteforce. Si no queremos pasar por todo el mapa de caracteres a-z podemos ajustarlo así -1 abcdef -2 12345 de tal modo al hacer nuestra mascara ?1?1?1?1?2?2 solo abarcara los caracteres indicados y no todo el abecedario y números del 0 al 9.

?1?1?1?1?2?2 – aaaa11- ffff55

--toggle-min/max=NUM : ajustando esta opción le diremos a hashcat esperar un mínimo/máximo de X y Y en el plano. Se puede reducir el tiempo de ejecución al no tomar en cuenta los valores que se encuentran fuera de los requisitos de la contraseña. Por defecto min=1 max=16.

--toggle-min=3 --toggle-max=5

--pw-min/max=NUM :Esto especificara el mínimo y máximo numero de espacio de caracteres cuando se hace el Brute-force o Mass-attack. El siguiente comando intentara todas las combinaciones de caracteres desde 1 a 8 espacios. Solo los espacios comenzando con min y terminando con max serán tomados en cuenta. Por defecto min=1 max=16.

--pw-min =1 –pw-max=8

--table-file=FILE : Especifica que tabla usar con –a 5. Las tablas pueden ser ubicadas en la carpeta Tables o puede crear unas por sí mismo. Por defecto no usado.

--table-min/max=NUM: Cualquier palabra fuera del rango especificado sera ignorada. Por defecto 10.

--perm-min/max=NUM : Cualquier palabra fuera del rango definido sera ignorada. Por defecto min=2 y max=10.


VECTORES DE ATAQUE

Straight. (Directo).Este ataque simplemente corre una lista de palabras y prueba todas las cadenas contra cada hash. Este es un vector extremadamente efectivo para una primera pasada, asumiendo que usted tiene un buen diccionario como el famoso Rockyou Very Happy

Combination. Junta dos palabras del diccionario dado e intenta con ellas. Puede ser útil en contra de contraseñas largas, la gente trata de añadir complejidad a sus contraseñas escribiéndolas dos veces o personas que usan su primer y segundo apellido. Por ejemplo atom usa su apellido derp para hacer una contraseña, si atom y derp existen en el diccionario, atomderp será usado y también derpatom.

Toggle-case. Simplemente cambia las letras minúsculas por mayúsculas y viceversa. PasS se volvería pASs.

Brute-force. Extremadamente util en GPU, Fuerza bruta con CPU puede tardar para siempre. Este vector tratara cada combinación --pw-min/max. Esencialmente trata cada combinación por incrementos hasta que encuentra el texto plano requerido. Altamente poco inteligente y usado como último recurso, excepto en maquinas con un alto poder en GPU.

Permutation. Este vector reordena todas las letras suministradas en el diccionario que coinciden en min/max. Por ejemplo min=1 y max=3 tomara las palabras con una longitud de 1 a 3 y las reacomodara en todas las posiciones posibles. Puede ser efectivo contra generadores aleatorios de contraseñas. por defecto min=2 y max=10.

Table-lookup. Esta función a cambiado un poco con respecto a su antecesor hashcat 0.40 pero aun así es un altamente avanzado vector de ataque que rompe una cadena en caracteres individuales y aplica una regla definida en table-file=FILE a cada uno. Por ejemplo password es roto en cada carácter: p a s s w o r d. Entonces hashcat mira en la tabla por las reglas que deben ser aplicadas en cada carácter. En este caso nuestra tabla tendría

a=a
a=A
p=p
p=P
o=o
o=O
o=0 (cero)

Ahora cada caracter que coincida será cambiado y probado. Así que por cada a, a y A será probada, por cada p, p y P será probado y por cada o, o, O y 0 será probado. Para aquellos que están familiarizado con las mascaras sería algo así -1 pP -2 aA -3 oO0 ?1?2ssw?3rd. Luce algo extraño quizás para usted pero o se preocupe esto será explicado en oclhashcat donde es altamente usado.


EJEMPLOS

STRAIGHT

Supongamos que tenemos una lista de aproximadamente unos 10000 hashes D: parece algo imposible. Pero nuestro amigo hashcat nos ayudara en esta difícil mission

Tutorial hashcat para Kali Linux Hashca13

El comando en la terminal fue ./hashcat-cli32.bin -a 0 -m 0 /root/Desktop/hash.txt /root/Desktop/dicejemplo.txt --output-file=/root/Desktop/founded.txt --remove

Lo que le dijimos a hashcat fue que usara el modo –a 0 de ataque directo con diccionario con el tipo de algoritmo –m 0 que corresponde a MD5 usando la lista de hashes y el diccionario de ejemplo en los directorios especificados y reescribiendo los hashes recuperados en el archivo indicado. Ahora veamos el resultado.

Un éxito casi total Very Happy el ataque directo recupero 9878 hashes. Esto depende totalmente del diccionario si la palabra no se encuentra el hash no podrá ser recuperado. Como vemos en el diccionario solo hay 9910 palabras y teníamos 10682 hashes. Como haremos para recuperar el resto? Con la demás opciones podremos mutar las palabras y ver si eso nos trae algún resultado.

Tutorial hashcat para Kali Linux Hashca14

COMBINATION

Probemos ahora con la lista restante el ataque de –a 1 o de combinación, como antes explicamos si hay contraseñas que coincidan o que sean palabras duplicadas, las encontrara

Tutorial hashcat para Kali Linux Hashca15

En general lo único que cambio en el comando fue –a 1 lo demás quedo intacto, como ves manejar hashcat no resulta difícil después de todo, el comando escrito queda de la sig. Manera : ./hashcat-cli32.bin -a 1 -m 0 /root/Desktop/hash.txt /root/Desktop/dicejemplo.txt --output-file=/root/Desktop/founded.txt --remove

Ejecutaremos de nuevo hashcat con los nuevos ajustes y veremos qué resultados tendremos

Tutorial hashcat para Kali Linux Hashca16

Como podemos ver recuperamos 368 hashes mas y eso con un mismo diccionario, pero aun nos quedan más hashes por recuperar, intentemos otro tipo de ataque con el mismo diccionario y esperemos tener algo de suerte.

TOGGLE-CASE

Usemos ahora el modo –a 2 en el cual las minúsculas son cambiada a mayúsculas y viceversa los parámetros son iguales solo cambiaremos el modo de ataque.

El comando es básicamente el mismo solo con –a 2 de diferencia: ./hashcat-cli32.bin -a 2 -m 0 /root/Desktop/hash.txt /root/Desktop/dicejemplo.txt --output-file=/root/Desktop/founded.txt --remove

Tutorial hashcat para Kali Linux Hashca17

Ejecutamos de nuevo hashcat y vemos que nos traerá a la salida.

Tutorial hashcat para Kali Linux Hashca18

Ahora podemos observar que recuperamos cada vez menos contraseñas, en total 91, esto es algo común, las contraseñas seguras son las que quedan a lo último, o en el peor de los casos, no pueden ser recuperadas XD. Algo más que podemos diferenciar es la cantidad de palabras en el diccionario como no especificamos un máximo --toggle-max hashcat tomo por defecto 16, entonces las palabras mayores de 16 no fueron tomadas en cuenta, en este caso fueron 10 palabras excluidas.

Aun quedaron contraseñas sin recuperar, intentaremos con los vectores de ataque restantes.

BRUTE-FORCE

Como se menciono antes la fuerza bruta en cpu es lo último a lo que debemos recurrir a menos que sepamos que la lista tiene un patrón, este puede ser una mayúscula al inicio, tres o dos números al final etc. En este caso no sabemos nada acerca de la lista y probaremos con algo sencillo ya que largas cadenas pueden tomar horas.

Tutorial hashcat para Kali Linux Hashca19

Podemos apreciar la diferencia de que ahora no estamos usando el diccionario sino una “mascara” que emulara al diccionario e intentara en un espacio de 5 todas las letras del abecedario. De otra manera la máscara ?l?l?l?l?l hará combinaciones desde aaaaa hasta zzzzz.

El comando en la terminal fue ./hashcat-cli32.bin -a 3 -m 0 /root/Desktop/hash.txt ?l?l?l?l?l --output-file=/root/Desktop/founded.txt –remove

Tutorial hashcat para Kali Linux Hashca20

Como resultado solo recuperamos dos contraseñas no fue mucho, ya que si la cadena de texto es menos de 5 o más 5 no podrá ser recuperada. Es una de las limitantes de usar fuerza bruta. Podríamos aumentar la complejidad añadiendo caracteres a medida como -1 ?d?l?s para probar todas las combinaciones de minúsculas, mayúsculas y números pero eso podría tardar una eternidad usando CPU.

PERMUTATION

Este vector lo que hará ser reacomodar las palabras en nuestro diccionario, si tenemos por ejemplo ABC lo convertirá en:
ABC
ACB
BAC
BCA
CAB
CBA

Tutorial hashcat para Kali Linux Hashca21

Regresamos a nuestro primer diccionario y usamos el ataque –a 4. El comando escrito fue ./hashcat-cli32.bin -a 4 -m 0 /root/Desktop/hash.txt '/root/Desktop/dicejemplo.txt' --output-file=/root/Desktop/founded.txt –remove

Veamos el resultado más abajo.

Tutorial hashcat para Kali Linux Hashca22

Comparado con los demás ataques que realizamos este tomo un poco más de tiempo, pero recuperamos mas hashes que usando el ataque de fuerza bruta, en total 26. Vemos también que la cantidad de palabras usadas fue de 9247 menos que la cantidad total ya que no especificamos el largo máximo, por defecto hashcat toma la cantidad de max=10.

TABLE-LOOKUP

Probemos el ultimo de nuestros vectores de ataque, para este ataque usamos el modo –a 5 y añadimos la opción --table-file='/root/Desktop/tablas.table' donde se encuentran nuestras reglas para la tabla.

a=a
a=A
a=4
@=a
@=@
o=o
o=O
o=0
i=1
i=I
s=S
s=5
s=s

Usamos algo sencillo ya que esto en ocasiones puede llegar a tomar un par horas, usted puede configurar sus reglas de la tabla como más le guste, en este caso usamos algo que la gente usa para escribir sus contraseñas. Sustituir letras por números, mayúsculas y caracteres especiales.

Tutorial hashcat para Kali Linux Hashca23

El comando escrito fue ./hashcat-cli32.bin -a 5 -m 0 /root/Desktop/hash.txt '/root/Desktop/dicejemplo.txt' --output-file=/root/Desktop/founded.txt --remove --table-file=/root/Desktop/tablas.table --table-min=1 --table-max=20

Añadimos las opciones --table-min=1 --table-max=20 para aumentar nuestras probabilidades, ya que no sabemos el largo de todas las palabras en nuestro diccionario y hashcat toma por defecto max=10

Ejecutemos y veamos.

Tutorial hashcat para Kali Linux Hashca24

Como podemos ver recuperamos una buena cantidad, en total 32 más. Pero observamos que las contraseñas bien pensadas y seguras son las restantes, podrían ser rotas usando otro diccionario y aplicando los vectores anteriores o usando reglas pero eso lo veremos en oclhashcat. En la cantidad de palabras vemos que siguen siendo menor a la cantidad original del diccionario, por distintas razones, hay palabras de más de 30 caracteres, o algunas no encajaron con las reglas que escribimos en la tabla.

En total quedaron 285 hashes sin romper, pero de una cantidad de 10682 a 285 es una buena cantidad

Demos una mirada a nuestro archivo de contraseñas recuperadas.

Tutorial hashcat para Kali Linux Hashca25

Si hacemos matemáticas 10682-285=10397.


ESPECIFICOS HASHCAT

Hascat es especialmente bueno para reducir largar listas de hashes usando métodos rápidos como el ataque de diccionario (–a 0) O el toggle-case( –a 2) y el resto. Aunque es la herramienta de recuperación basada en CPU mas rápida del mundo, a hashcat le tomaría años romper un hash con una larga cadena de texto usando Brute-force (-a 3). Aqui es donde oclhashcat entra en juego. Los usuarios con una tarjeta grafica con posibilidades GPGPU (General-Purpose Computing on Graphics Processing Units) pueden además usas hashcat.


OCLHASHCAT-PLUS(CUDAHASHCAT-PLUS)

La ultima versión de hashcat está disponible para su descarga desde http://hashcat.net/oclhashcat-plus/ . 7zip es un requerido para descomprimir el archivo. Oclhashcat es la herramienta de recuperación de contraseñas basada en GPU más rápida del mundo, codeada por Atom para ambas plataformas GUN/Linux y Windows de 32 y 64 bits. Este es un gran avance desde las soluciones basadas en CPU debido a la gran cantidad de dato que se pueden procesar usando las plataformas de CUDA (NVIDIA), Stream (AMD) y OpenCL.

Lo que tomaría horas en un CPU ahora puede tomar minutos en un GPU y aquí tenemos como hacerlo

El proceso de instalación es el mismo, si tienen dudas dirígete de nuevo a la sección 1.1

Unas ves descomprimidas veremos unos archivos y unas carpetas, daremos una explicación rápida y después iremos de lleno a oclhashcat.

Tutorial hashcat para Kali Linux Hashca26

Una nota rápida: Atom ha divido oclhashcat en 2 ejecutables, el primero es oclhashcat para usuarios con tarjetas AMD(ATI). Esto requerirá que tengan los drivers y el SDK instalado. Aquí no veremos cómo instalar los drivers daremos hecho que ya lo tienen instalados. Para instalarlos les daremos los links de referencia:

http://www.backtrack-linux.org/wiki/index.php/Install_OpenCL
http://ati.amd.com

Los otros dos principales ejecutables son cudahascat que es para lo usuarios con NVIDIA. Para NVIDIA solo necesitamos el forceware driver.

http://www.backtrack-linux.org/wiki/index.php/CUDA_On_BackTrack

Por simplicidad nos referiremos a ambos ejecutables como oclhashcat y los ejemplos serán corridos todos usando oclhashcat. Para los usuarios Nvidia, tienen que correr cudahashcat o tendrán un error. Tampoco hay diferencia entre los ejemplos corridos en oclhashcat y cuda hashcat así que no tienen de que preocuparse.

oclHashcat(32/64).(bin/exe)- es el ejecutable principal

oclExample.(sh/cmd)- es un archivo de ejemplo para actuar como un inicio rápido para oclhashcat, será usado como ejemplo, pero después de leer un poco mas no lo necesitara.

example.dict – un diccionario de ejemplo de hashcat

example.hash – son las cadenas de texto encriptadas de el diccionario de ejemplo

doc/ - son los documentos que pertenecen a oclhashcat

kernels/ - es el directorio donde los kernel del hardware son almacenados. Esto no debe ser tocado a menos que seas un miembro autorizado de la comunidad hashcat, de otro modo, puedes dañar el ejecutable.

Usando los ejemplos suministrados corramos rápidamente hashcat.

Tutorial hashcat para Kali Linux Hashca27

Como vemos, tenemos muchos datos a la salida, lo importante aquí esta marcado con el punto blanco que es el hash recuperado, en este caso es solo uno, pero se imaginan una lista de unos 1000 y apareciendo todos en la terminal? Sería algo fastidioso, por eso en los ejemplos, preferimos no mostrarlos a la salida.


OPCIONES

Hascat viene con un gran repertorio de opciones veamos que tiene:

Para eso en la terminal tecleamos ./oclHashcat-plus32.bin --help

Oclhashcat funciona un poco diferente a hashcat, oclhashcat usa dos mascaras, izquierda y derecha, lo mismo sucede en el ataque de combinación usa dos diccionarios diferentes, uno derecho y uno izquierdo.

Hay ciertas opciones que son idénticas en hashcat y oclhashcat así que no las repetiremos todas.

./oclHashcat64.bin hashlist dictizquierdo dictderecho
Es algo confuso quizás pero no te preocupes más adelante en los vectores de ataque lo explicaremos mas a detalle.

Ahora pasemos a las opciones que tenemos disponibles.

--quiet : suprime la salida hacia el “STDOUT” es decir, en tu terminal de Linux, no serás floodeado con texto, hashes recuperados y errores de programa. Por defecto no usado.

--remove : Igual que en hashcat, una vez que el hash es recuperado, es borrado de la lista de hashes, así previniendo que oclhashcat lo ataque de nuevo. Por defecto no usado.

--output-file=FILE O –o :Especifica donde los hashes serán escritos. Esto debe ser usado si usted planea preservar los hashes, o no desea copiarlos y pegarlos desde la terminal. Por defecto no usado.

--output-file=/user/Desktop/founded.txt O –o /user/Desktop/founded.txt

--output-format=NUM : NUM puede ser 0, 1 o 2. Normalmente no se necesita pero si un texto plano contiene caracteres en hexadecimal esto necesita ser especificado para evitar textos planos erróneos. Por defecto modo 0.

--output-format=0

--salt-file=FILE O –e : Suministra los “salt” para los hashes que lo tienen ausente. útil para los hashes vbull o xtcommerce . Por defecto no usado.

--gpu-devices=STR O –d : Esto es usado para especificar el número de GPU (en orden) para usar, si usted tiene una instalación multi-GPU. Esto es útil si usted trabaja con ventanas, como Windows, Gnome o KDE. Debido a la gran cantidad de datos que se procesan, el escritorio tendrá algo de lag. Usted puede omitir su primer GPU para asegurar una buena operación de su escritorio mientras recupera los hashes. Por defecto todas las tarjetas son usadas.

-d 2,3,4

--gpu-accel=NUM O –n: Define el ajuste de la carga de trabajo. Mientras más alto es el valor la tarjeta grafica trabajara más. Valores altos pueden ser útiles en ataques de fuerza bruta mientras que valores bajos pueden ser útiles con ataques de diccionario. No hay un valor que se pueda decir

-n 400

--runtime=NUM : este operador, solo le da un límite de tiempo de ejecución a hashcat. Por defecto no usado.

--runtime=60 (60 segundos)

--hex-salt : Asume que el “salt” esta dado en hexadecimal. Por defecto no usado.

--hex-charset : Asume que el mapa de caracteres esta dado en hexadecimal. Por defecto no usado

--username: Ignora los nombres de usuario en las listas de hashes, si tenemos user:hash, para evitar la molestia de editar largas listas, usamos este operador, para ahorrarnos tiempo. Por defecto no usado.

--force: Ignora las advertencias que arroje hashcat, en ciertos algoritmos como sha-1 te pedirá usar oclhashcat-lite si es un solo hash. Con este operador ignorara la advertencia. Por defecto no usado.

--gpu-async: Es ayuda a los Gpus de baja Gama, Solo para Nvidia.

--cpu-affinity=STR :Especifica el número de unidades cpu que se usaran, si su procesador de multi-nucleo. Por defecto no usado.

--cpu-affinity=2,3,4

--gpu-loops=NUM: esto es usado para ajustar mas la carga de trabajo. Mas específicamente, el numero de palabras por unidad de trabajo. Provee más trabajo a la máscara de la izquierda. Por defecto 128.

--gpu-loops=1024

--gpu-temp-disable : Deshabilita las lecturas de los disparadores temperatura y el ventilador. Úselo bajo su propio riesgo. Por defecto no usado.

--gpu-temp-abort=NUM: Establece la temperatura del GPU con la que oclhashcat abortara la sesión para evitar daños al GPU. Por defecto 90 grados Celsius.

--gpu-temp-abort=90

--gpu-temp-retain=NUM : establece la temperatura que oclhashcat tratara de mantener para evitar un sobre calentamiento del GPU. Por defecto 80 grados Celsius.

--gpu-temp-retain=80

--rule-left=RULE O –j and –rule-right=RULE O –k: Son reglas que se aplican a los diccionarios de la derecha o la izquierda cuando se usa el ataque de combinación y/o hibrido. La teoría puede ser un poco confusa, pueden visitar el siguiente sitio http://ob-security.info/?p=31%20. Y mas adelante en los vectores de ataque explicaremos un poco más. Por defecto no usado.

--increment O –i : incrementa la máscara con +1 en cada posición. Por ejemplo ?l?l?l?l?l al inicio correrá
?1
?1?1
?1?1?1
?1?1?1?1
?1?1?1?1?1

Por defecto no usado.

--increment-min=NUM : comienza a incrementar posiciones en la posición dada si tenemos una máscara de ?l?l?l?l?l?l?l y le decimos a oclhashcat que comience en 3.
?1?1?1
?1?1?1?1
?1?1?1?1?1
?1?1?1?1?1?1
?1?1?1?1?1?1?1

Esto se hace para ahorrar tiempo, si uno ya sabe que la contraseña no es de 3 caracteres. Se puede usar cualquier mascara o carácter a medida que uno especifique. Por defecto no usado

--increment-max=NUM : Lo mismo que el anterior solo que este le dará el limite a oclhashcat de detenerse en la posición que le especifiquemos. Por defecto no usado.

increment-max=12

Los operadores Markov son una nueva mejora a la versión 0.9 de oclhashcat, la teoría es algo confusa trataremos de explicarla

El ataque markov es un ataque parecido al de fuerza bruta pero basado en estadísticas, en lugar de especificar una máscara o un mapa de caracteres, especificamos un archivo.

Una vez que el archivo es creado en un paso previo este contiene información estadística, que lleva a cabo un análisis automatizado de un diccionario dado.

Para hacer este análisis usamos una herramienta nueva llamada "hcstatgen" la cual es parte de las nuevas utilerías hashcat “hashcat-utils package”.

http://www.hashcat.net/files/hashcat-utils-0.9-32.7z
http://www.hashcat.net/files/hashcat-utils-0.9-64.7z

La utilería hcstatgen genera un archivo .hcstat, el cual es procesado con otra de las nuevas utilerías "statprocessor" que genera las palabras basadas en un orden estadístico del archivo .hcstat .

En versiones anteriores esto no estaba en las funciones de oclhashcat en la versión 0.9 viene integrado, y se usa automáticamente al usar cualquier mascara tales como ?d?d?lu . el archivo por defecto es hashcat.hcstat el cual fue hecho usando la lista rockyou.txt

Como funciona?

Trataremos de explicarlo un poco mas a fondo.

En el ataque Brute-force o Mass-attack nosotros podemos especificar un limite del “keyspace” estableciendo un mapa de caracteres menor para reducir el tiempo de ataque. En el ataque markov tenemos algo parecido llamado "threshold". Todo lo que se tiene que hacer es especificar un número. Mientras mas alto el número más alto será el "threshold" para sumar otro enlace entre dos caracteres en la tabla de dos niveles en la que se basa el ataque markov.

No es tan necesario saberlo todo a fondo según las notas de Atom, solo tenemos que recordar que mientras más alto el valor mas alto será el “keyspace” así que el ataque será mas profundo y tardara mas. Si usamos un --markov-threshold=0 estaremos usando ataque puro de fuerza bruta pero con cadenas markov.

Más adelante les explicaremos como crear el .hcstat ahora les explicaremos los operadores markov

Markov:

--markov-hcstat: especifica el directorio donde se encuentra el archivo .hcstat . Por defecto es hashcat.hcstat

--markov-hcstat /user/Desktop/markov.hcstat

--markov-disable : Deshabilita el uso de cadenas markov y hacemos un ataque puro de fuerza bruta

--markov-classic: habilita el uso clásico de cadenas markov, no hay un realce por cada posición, según notas de Atom, el modo classic es un 29 % menos eficiente. Úselo según le convenga.

--markov-threshold=NUM O –t: especificamos cuando oclhashcat deja de aceptar más cadenas markov.

Pongámoslo así

Un threshold de 3 y un largo de 6 caracteres = =729

Un threshold de 10 y un largo de 8 caracteres = =100000000

A menos que especifiquemos lo contrario con –t 0 o –t 10, al usar una máscara cómo ?l?l?l?l?l hashcat intentara con todo el keyspace, puesto de otra manera (# de caracteres)^(largo).

--attack-mode=NUM O –a: El tipo de ataque a usar en contra de un hash. Usando los diferentes vectores de ataque aumentaran las probabilidades de recuperar la contraseña. Los modos son los siguientes:

 0 = Straight- simplemente corre todas las palabras del diccionario en contra de la lista de hashes, teniendo un buen diccionario aumentara las probabilidades de recuperar tu hash.
 1 = Combination – combina las palabras de los diccionarios dados, recuerda que Oclhashcat usa un diccionario izquierdo y uno derecho.
 3 = Brute-force – Fuerza bruta o también conocido como Mask Attack. A diferencia de hashcatcli que es usado como un paso desesperado, con oclhashcat puede ser realizado en minutos u horas con la ayuda del GPU, y usando el ataque markov.
 6 = Hybrid dict + mask – es un ataque hibrido, fácil especificamos en la izquierda un diccionario y en la derecha una mascara como si fuésemos a usar un mass attack. Por ejemplo /user/Desktop/diccionario.txt ?d?d?d lo que hará Oclhashcat será añadir la mascara en la parte del frente de la palabra del diccionario. Si tenemos en el diccionario “aaaaa” la palabra seria “aaaaa000 hasta aaaaa999 “
 7 = Hybrid mask + dict – lo mismo descrito anteriormente pero al revés, tendríamos de “000aaaaa hasta 999aaaaa”.
Nota: para los modos 0, 1, 6, 7 el uso de reglas puede ser usado, lo explicaremos más adelante.

VECTORES DE ATAQUE

Straight. (Directo).Este ataque simplemente corre una lista de palabras y prueba todas las cadenas contra cada hash. Este vector ya lo hemos estudiado en la versión cpu de hascat. La versión GPU tiene una manera particular de cargar los diccionarios, cada palabra del diccionario será puesta en un buffer especial por el largo de la cadena de caracteres, es decir el largo de cada palabra tiene un buffer único, las palabras de 8 letras serán almacenadas en el buffer numero 8 y así sucesivamente, pero como sucede normalmente los diccionarios no están ordenados, así que para optimizar la carga del diccionario te sugerimos, si usas Linux usar http://hashcat.net/wiki/doku.php?id=hashcat_utils la herramienta splitlen junto con el comando “sort”

Sort –u dicc.txt > diccordenado.txt

Esta es una de las razones por la que es difícil en Oclhashcat restaurar la sesión como su contraparte basada en CPU.

Combination. Junta dos palabras del diccionario dado e intenta con ellas. Puede ser útil en contra de contraseñas largas, la gente trata de añadir complejidad a sus contraseñas escribiéndolas dos veces o personas que usan su primer y segundo apellido. Por ejemplo Atom usa su apellido derp para hacer una contraseña, si Atom y derp existen en el diccionario, atomderp será usado y también derpatom. Lo especial de este ataque en oclhascat es que podemos aplicar reglas a los diccionarios de izquierda y derecha.

Brute force (Mask attack)- con este ataque probamos todas las combinaciones de un espacio dado pero más especifico, igual que en un ataque de fuerza bruta. La razón de esto es para no apegarse al tradicional ataque de fuerza bruta es para reducir los candidatos de contraseñas. Convirtiéndolo en un ataque más especifico y eficiente. Por cada posición tenemos que configurar un marcador, si nuestra contraseña a crackear tiene 8 espacios usaremos 8 marcadores.

 ?l = abcdefghijklmnopqrstuvwxyz
 ?u = ABCDEFGHIJKLMNOPQRSTUVWXYZ
 ?d = 0123456789
 ?s = !”#$%&'()*+,-./:;⇔?@[\]^_`{|}~
 ?h = 8 bit characters from 0xc0 - 0xff
 ?D = 8 bit characters from german alphabet
 ?F = 8 bit characters from french alphabet
 ?R = 8 bit characters from russian alphabet

Si queremos solo letras minúsculas usaríamos ?l?l?l?l?l?l?l?l así nuestra mascara probara desde” aaaaaaaa” hasta “zzzzzzzz”

Pero y si queremos solo numero y letras? Qué tal si el pass es geyepn123 ? pues fácil, en Oclhashcat tenemos los caracteres a medida entonces configuraríamos así:

--custom-charset1=?l?d O de manera análoga -1 ?l?d

Nuestra mascara seria ?1?1?1?1?1?1?d?d?d

Ahora calculemos el tiempo, digamos que es un algoritmo rápido como el MD5 y nuestro GPU tiene una velocidad con el MD5 de 100 millones de contraseñas por segundo o 100M/s

36*36*36*36*36*36*10*10*10= 366 *103 =2176782336000/100000000=21767.82 seg.
21767.82/60=362.79 minutos
362.79/60= 6.04 horas

Parecerá mucho tiempo, pero comparado con CPU esto podría tomar años. Hay GPU más poderosos donde podemos llegar hasta a 1000M/s y con multi-gpu unos 8000M/s

Hybrid. “ dict + mask” y “mask + dict” básicamente es solo un ataque de combinación, de un lado tienes un diccionario y del otro simplemente un ataque de fuerza bruta, en otras palabras los caracteres de fuerza bruta son usados como prefijo o sufijo en cada una de las palabras del diccionario, es por eso que se llama Hibrido. Como alternativa se puede usar el modo 3 o mask attack o el ataque basado en reglas.

RULE-BASED ATTACK

El ataque basado en reglas es uno de los ataques más complicados que existen en la familia hashcat. La razón de esto es muy simple, el ataque basado en reglas es como un lenguaje de programación, diseñado para generación de candidatos de contraseñas. Tiene funciones para modificar, cortar o extender palabras y tiene operadores condicionales etc. Esto lo hace un ataque más flexible, seguro y eficiente.

El motor de las reglas fue hecho paraqué pueda ser compatible con otros programas de recuperación de contraseñas como John the ripper y Passwordspro, que no veremos en este manual.

Lo más importante al escribir reglas es sabiendo que es lo que queremos escribir, esto significa que tendría que analizar docenas de contraseñas en texto plano para poder definir un patrón en ellas. Por ejemplo sabemos que comúnmente las personas añaden un digito a sus contraseñas, con esto ya tenemos dos parámetros.

*queremos añadir algo
* El valor que queremos añadir es un digito

Las siguiente funciones son 100 % compatibles con John the ripper y passwordspro.

Tutorial hashcat para Kali Linux Hascat10
Tutorial hashcat para Kali Linux Hascat11
* indica que N inicia en 0. Para posiciones de caracteres diferentes que 0-9 use A-Z (A=11)
+ indica que esta regla es implementada en hashcat únicamente.

Las siguientes funciones no están disponibles en John the ripper y Passwordspro

Tutorial hashcat para Kali Linux Hascat12

* indica que N inicia en 0. Para posiciones de caracteres diferentes que 0-9 use A-Z (A=11)

Bueno y dirá, quizás, wtfck? Que son esas tablas? Pues fácil son los comandos que usaremos en las reglas, usaremos lo que está en la segunda columna donde dice “Function”, no nos molestaremos en traducir estas tablas a español, así que si no sabe ingles pues use google para traducir XD.

Estas reglas las podemos guardar en un archivo .txt o si quiere cambiarle la extensión a .rule para no confundirse no hay problema, esto no afecta en nada.

Supongamos que ya creamos nuestro archivo donde guardaremos nuestras reglas

Como mencionamos antes si queremos añadir una cifra a nuestras palabras del diccionario en el archivo escribiremos

Así que si tenemos en nuestro diccionario:

putoepn

A la palabra le será añadida la cifra 1 y tendremos

putoepn1

Si lo que queremos es añadir todos los números del 0 al 9 para eso tenemos el ataque hibrido.

Si queremos hacer más complejo el ataque podemos añadir

:

r

ru

Con eso le dijimos a hashcat que primero no haga nada, solo comparar la palabra, en la segunda línea le dijimos que invierta la palabra y en la tercera línea le dijimos que invierta la palabra y que además la palabra la pase toda a mayúsculas.

Otra característica especial de hashcat es que con hashcat puedes generar reglar aleatorias sobre la marcha en una sesión. Esto es de mucha utilidad cuando estas totalmente sin ideas.

--generate-rules=NUM – con esto le decimos a hashcat que genere el numero de reglas indicadas a aplicar en cada intento.

--generate-rules-func-min=NUM
--generate-rules-func-max=NUM

Estos operadores especifican el número de funciones que deben ser usadas. Si ponemos como mínimo 1 y máximo 3 el resto de las funciones serán ignoradas. Por ejemplo digamos que hashcat genero las funciones

:
]]]]
ru

lo que pasara entonces es que la segunda función “]]]]’ será ignorada.
