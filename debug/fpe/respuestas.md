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
al momento de compilar
gcc -g -O0 test_fpe3.c -lm -o test_fpe3.e


Al probar con valores de entrada se obtiene:

En test_fpe1
 ENTRADA: 2 y 3
 SALIDA: 0.666667 ===> OK

En test_fpe2
 ENTRADA: 2.0 y 3.0
 SALIDA: -1.000000 ===> OK

 ENTRADA: 2.0 y 0.0
 SALIDA: 1.000000 ===> OK a pesar que el resultado es infinto, este es mayor a 2.
 
En test_fpe3
 ENTRADA: 2.0 y 0.0
 SALIDA: inf ===> ERROR

Al agregar el flag -DTRAPFPE requiere agregar la función set_fpe_x87_sse_()
que se encuentra alojada en el directorio fpe_x87_sse/

En el directorio fpe_x87_sse/ creo el objeto de la funcion
gcc -c  -g -O0 fpe_x87_sse.c -o fpe_x87_sse.o


Vuelvo al directorio e intento unir los objetos 

En test_fpe1

gcc test_fpe1.o -Ifpe_x87_sse fpe_x87_sse/fpe_x87_sse.o -DTRAPFPE -o test_fpe1.e
produce un error que antes no aparecía y se debe a que falta el linkeo 
con la libreria de matematicas.

Solucion:
gcc test_fpe1.o -Ifpe_x87_sse fpe_x87_sse/fpe_x87_sse.o -DTRAPFPE -lm -o test_fpe1.e


En test_fpe2
gcc test_fpe2.o -Ifpe_x87_sse fpe_x87_sse/fpe_x87_sse.o -DTRAPFPE -lm -o test_fpe2.e





gcc -c test_fpe1.c -DTRAPFPE -I./fpe_x87_sse/
LINKEO CON LA LIBRERIA
gcc -o test_fpe1.e test_fpe1.o ./fpe_x87_sse/fpe_x87_sse.o -lm










