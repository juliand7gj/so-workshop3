### Taller 3

**Universidad ICESI**  
**Curso:** Sistemas Operativos  
**Docente:** Daniel Barragán C.  
**Tema:** Llamadas al sistema (syscalls)  
**Correo:** daniel.barragan at correo.icesi.edu.co

**Nombre estudiante:** Julián David González Jiménez  
**Código estudiante:** A00315288

### Objetivos
* Conocer y explicar la función de las llamadas al sistema en un sistema operativo

### Prerrequisitos
* Virtualbox o WMWare
* Máquina virtual con sistema operativo CentOS7
* Aplicativo strace

### Descripción


### Actividades 

**1.** Empleando el aplicativo **strace** obtenga 5 llamadas al sistema para uno o varios comandos de linux. Explique por qué los comandos seleccionados emplean las llamadas al sistema encontradas, para ello debe emplear los manuales de Linux en Internet o del sistema operativo (comando **man**). Debe incluir la explicación de los parámetros que reciben las llamadas al sistema encontradas. Consigne capturas de pantalla donde muestre las llamadas al sistema obtenidas (sugerencia: emplear -etrace para filtrar los resultados)

### Solución 1

Utilizare el comando mkdir para obtener las llamadas al sistema. 

![][1] 

* statfs(const char path, struct statfs buf): devuelve información sobre un sistema de archivos. path es la ruta de acceso a cualquier archivo dentro del sistema de archivos. buf es un puntero a una estructura de statfs definida aproximadamente.

![][2] 

* stat(const char pathname, struct stat statbuf): devuelve información sobre un archivo, en el buffer apuntado por statbuf. stat() requiere el permiso de ejecución (búsqueda) en todos los directorios en la ruta que conducen al archivo. Recupera información sobre el archivo apuntado por pathname. 

![][3] 

* open(const char pathname, int flags): Dado un nombre de ruta para un archivo, open () devuelve un descriptor de archivo, un entero pequeño, no negativo para uso en llamadas de sistema posteriores (leer (2), escribir (2), lseek (2), fcntl (2), etc.) . El descriptor de archivo devuelto por una llamada correcta será el descriptor de archivo de número más bajo que no está abierto actualmente para el proceso. Los indicadores de argumento deben incluir uno de los siguientes modos de acceso: O_RDONLY, O_WRONLY o O_RDWR. Éstos solicitan abrir el archivo sólo lectura, sólo escritura o lectura / escritura, respectivamente.

![][4] 

* access(const char pathname, int mode): comprueba si el proceso que llama puede tener acceso al pathname del archivo. Si pathname es un enlace simbólico, se desreferencia. El modo especifica la o las comprobaciones de accesibilidad a realizar, y es el valor F_OK o una máscara que consiste en el OR binario de uno o más de R_OK, W_OK y X_OK. F_OK prueba la existencia del archivo. R_OK, W_OK y X_OK comprueban si el archivo existe y otorga permisos de lectura, escritura y ejecución, respectivamente.

![][5] 

* mmap(void addr, size_t length, int prot, int flags, int fd, off_t offset): crea una nueva asignación en el espacio de direcciones virtual del proceso de llamada. La dirección inicial para la nueva asignación se especifica en addr. El argumento length especifica la longitud de la asignación. Si addr es NULL, entonces el kernel elige la dirección en la cual crear la asignación; este es el método más portátil de crear una nueva asignación. Si addr no es NULL, entonces el kernel lo toma como una pista sobre dónde colocar el mapeo; en Linux, la asignación se creará en un límite de página cercano. La dirección de la nueva asignación se devuelve como resultado de la llamada. El argumento prot describe la protección de memoria deseada de la asignación (y no debe entrar en conflicto con el modo abierto del archivo). El argumento flags determina si las actualizaciones de la asignación son visibles para otros procesos que asignan la misma región y si se realizan actualizaciones al archivo subyacente. 

![][6] 

2. Realice la compilación del código fuente adjunto y su ejecución empleando el aplicativo **strace**. Identifique las llamadas al sistema encargadas de enviar y recibir datos a través de la red. A partir de los manuales de Linux en Internet o del sistema operativo explique las llamadas al sistema encontradas y sus parámetros.

**Nota:** Cuando compile programas tenga en cuenta que estos pueden necesitar la instalación de librerías para su compilación y ejecución.

**Debian**
```
# apt-get install libcurl4-openssl-dev
$ gcc -o curl curl.c -lcurl
```
**CentOS**
```
# yum install libcurl-devel
$ gcc -o curl curl.c -lcurl
```

### Solución 2

Se utiliza el comando "strace -c ./curl" para obtener las llamadas al sistema de este archivo. 

![][7] 

Las llamadas al sistema encargadas de enviar y recibir datos a través de la red son: "sendto" y "recvfrom". 

* sendto(int sockfd, const void buf, size_t len, int flags, const struct sockaddr dest_addr, socklen_t addrlen): se utiliza para transmitir un mensaje a otro socket. El argumento sockfd es el descriptor de archivo del socket de envío. El argumento flags es el OR bit a bit de cero o más de los siguientes indicadores. 

![][8] 

* recvfrom(int sockfd, void buf, size_t len, int flags, struct sockaddr src_addr, socklen_t addrlen): se utiliza para recibir mensajes desde un socket. Puede utilizarse para recibir datos en sockets sin conexión y orientados a la conexión. Coloca el mensaje recibido en el buffer buf. El llamador debe especificar el tamaño del búfer en len. Si src_addr no es NULL y el protocolo subyacente proporciona la dirección de origen del mensaje, esa dirección de origen se coloca en el búfer señalado por src_addr. En este caso, addrlen es un argumento de valor-resultado. Antes de la llamada, se debe inicializar al tamaño del búfer asociado con src_addr. Al volver, addrlen se actualiza para contener el tamaño real de la dirección de origen. La dirección devuelta se trunca si el búfer suministrado es demasiado pequeño; en este caso, addrlen devolverá un valor mayor que el proporcionado a la llamada. Si la persona que llama no está interesada en la dirección de origen, src_addr y addrlen deben especificarse como NULL.

![][9] 

### Nota

El informe debe ser entregado en formato README.md y debe ser subido a un repositorio de github. El repositorio de github debe ser un fork de https://github.com/ICESI-Training/so-workshop3 y para la entrega deberá hacer un Pull Request (PR) respetando la estructura definida. El código fuente y la url de github deben incluirse en el informe.  

## Referencias

* http://man7.org/linux/man-pages/man2/syscalls.2.html  
* https://jvns.ca/blog/2014/09/18/you-can-be-a-kernel-hacker/


[1]: 1.png
[2]: 2.png
[3]: 3.png
[4]: 4.png
[5]: 5.png
[6]: 6.png
[7]: 7.png
[8]: 8.png
[9]: 9.png
