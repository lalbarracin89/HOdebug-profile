Primero intente crear los ejecutables de los 3 codigos, solo falla en test_fpe3.c

La compilacion fue realizada con el siguiente comando
gcc -g -O0 test_fpeX.c -o test_fpeX.e
siendo X el numero de cada archivo.

test_fpe3.c: In function ‘main’:
test_fpe3.c:19:9: warning: incompatible implicit declaration of built-in function ‘sqrt’ [enabled by default]
   tmp = sqrt(a / b);
         ^
/tmp/ccxxjSbQ.o: En la función 'main':
test_fpe3.c:(.text+0x5a): referencia a 'sqrt' sin definir
collect2: error: ld returned 1 exit status

La solución fue corregir la función sqrtf a sqrt y agregar el flag -lm 
(linkea a la libreria math) al momento de compilar
gcc -g -O0 test_fpe3.c -lm -o test_fpe3.e


Al probar con valores de entrada se obtiene:

En test_fpe1
 ENTRADA: 2 y 3
 SALIDA: 0.666667 ===> OK

 ENTRADA: 2.
 SALIDA: inf ===> ERROR


En test_fpe2
 ENTRADA: 2.0 y 3.0
 SALIDA: -1.000000 ===> OK

 ENTRADA: 2.0 y 0.0
 SALIDA: 1.000000 ===> ERROR  


En test_fpe3
 ENTRADA: 2.0 y 0.0
 SALIDA: inf ===> ERROR

Al agregar el flag -DTRAPFPE requiere agregar la función set_fpe_x87_sse_()
que se encuentra alojada en el directorio fpe_x87_sse/

En el directorio fpe_x87_sse/ creo el objeto de la funcion
gcc -c  -g -O0 fpe_x87_sse.c -o fpe_x87_sse.o


Vuelvo al directorio linkeo los objetos 
 gcc -c test_fpeX.c -DTRAPFPE -I./fpe_x87_sse/
 LINKEO CON LA LIBRERIA
 gcc -o test_fpeX.e test_fpeX.o ./fpe_x87_sse/fpe_x87_sse.o -lm
 siendo X el numero de cada archivo.


En test_fpe1
 Insert a, b 
 2
 0
 floating point exception  ./test_fpe1.e

En test_fpe2
 Insert a, b 
 2
 0
 floating point exception  ./test_fpe2.e

En test_fpe3
 Insert a, b 
 2
 0
 floating point exception  ./test_fpe3.e

Estos mensajes son correctos ya que se produce un overflow en el stack


