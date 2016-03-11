Este ejercicio es muy libre. Compilen los códigos de C o FORTRAN
con distintas opciones de optimización (`-O0`, `-O1`, `-O3`) y ejecútenlo con distintas
formas de hacer un profiling: desde `time ./programa.e` hasta `perf`,
pasando por `gprof` y cualquier otra cosa que se les ocurra. Escriban
un pequeño reporte en `reporte.md` respecto de qué cosas hicieron y
cómo mejoraron/empeoraron los tiempos. 

profile_me_1.c

Fue necesario ejecutar el comando ulimit -s unlimited porque el fichero 
contenia matrices que excedian el stack de memoria.

Una forma de solucionar este bug sería utilizar matrices dinamicas (No resuelto).


$ gcc -g -O0 profile_me_1.c -o profile_me_1_O0.e

	TIME
	
	time ./profile_me_1_O1.e
	real	0m0.632s
	user	0m0.591s
	sys		0m0.040s
	gprof
	
	GPROF

	gcc -pg -O0 profile_me_1.c -o profile_me_1_pO0.e
	gprof profile_me_1.e gmon.out > profile_1_O0.info
	Each sample counts as 0.01 seconds.
	%   cumulative   self              self     total           
	time   seconds   seconds    calls  ns/call  ns/call  name    
	40.75      0.32     0.32 25000000    12.88    12.88  first_assign
	29.93      0.56     0.24 25000000     9.46     9.46  second_assign
	29.29      0.79     0.23                             main

	PERF
	
	perf record ./profile_me_1_O0.e
	perf report -i perf.data
	[ perf record: Woken up 1 times to write data ]
	[ perf record: Captured and wrote 0.166 MB perf.data (~7271 samples) ]
	[kernel.kallsyms] with build id dba2ede7a119af631d2587d4b7bcf51282419cb8 not found, continuing without symbols


$ gcc -g -O1 profile_me_1.c -o profile_me_1_O1.e

	TIME
	
	time ./profile_me_1_O1.e
	real	0m0.731s
	user	0m0.675s
	sys		0m0.048s

	GPROF

$ gcc -pg -O1 profile_me_1.c -o profile_me_1_pO1.e

	gprof profile_me_1_pgO1.e gmon.out > profile_1_O1.info
	
	Each sample counts as 0.01 seconds.
	%   cumulative   self              self     total           
	time   seconds   seconds    calls  ns/call  ns/call  name    
	62.11      0.24     0.24 25000000     9.69     9.69  first_assign
	31.06      0.36     0.12                             main
	7.76      0.39     0.03 25000000     1.21     1.21  second_assign

	PERF
	
	perf record ./profile_me_1_O1.e
	perf report -i perf.data
	[ perf record: Woken up 1 times to write data ]
	[ perf record: Captured and wrote 0.112 MB perf.data (~4895 samples) ]
	[kernel.kallsyms] with build id dba2ede7a119af631d2587d4b7bcf51282419cb8 not found, continuing without symbols

$ gcc -g -O3 profile_me_1.c -o profile_me_1_O3.e

	TIME
	time ./profile_me_1_O3.e
	real	0m0.009s
	user	0m0.000s
	sys		0m0.002s

	GPROF

$ gcc -pg -O3 profile_me_1.c -o profile_me_1_pO3.e

	gprof profile_me_1_pgO3.e gmon.out > profile_1_O3.info
	
	Each sample counts as 0.01 seconds.
	no time accumulated

	PERF
	
	perf record ./profile_me_1_O3.e
	perf report -i perf.data
	[ perf record: Woken up 1 times to write data ]
	[ perf record: Captured and wrote 0.001 MB perf.data (~60 samples) ]
	[kernel.kallsyms] with build id dba2ede7a119af631d2587d4b7bcf51282419cb8 not found, continuing without symbols
dsf

CONCLUSION
Con -O3 se produjo una importante optimizacion porque el tiempo de ejecucion
disminuyó considerablemente. Esto se debe a que se produjo una vectorizacion 
en las matrices
	
	
	

