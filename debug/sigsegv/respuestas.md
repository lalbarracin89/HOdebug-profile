
# Al ejecutar small.x no devuelve ningun error 
# Al ejecutar big.x devuelve el siguiente error:
	segmentation fault  ./big.x
	
 Al ejecutar 'ulimit -s unlimited' en la terminal y al correrlo nuevamente
a big.c no devuelve ningun error, pero no devuelve el promtp.

 ulimit permite limitar todo el sistema de uso de los recursos.
 Con el flag -s unlimited, cambiará el valor de limits the core file size
a unlimited.

 Utilizando ulimit -a se observo que el tamaño del stack es de 8192 kbytes
Al ejecutar ulimit -s unlimited, el tamaño del stack se redefinio a unlimited

 Esta no es una buena solucion para debugging porque incrementa el 
tamañano del stack. Una solucion seria trabajar con vectores dinamicos, y de
esta forma evitamos la reserva estatica de memoria


