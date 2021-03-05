#makefile

start:
	mkdir -p bin; \
	rm bin/*.*
	
serial:
	riscv64-unknown-elf-g++  -Wall -flax-vector-conversions  -O2 -c -o src/jacobi-2d.o src/jacobi-2d.cpp; \
	riscv64-unknown-elf-g++  -Wall -flax-vector-conversions   -march=rv64g -O2 -o bin/jacobi-2d_serial.exe src/*.o -lm; \
	rm src/*.o; \
	riscv64-unknown-elf-objdump  -dC -ds  bin/jacobi-2d_serial.exe > bin/jacobi-2d_serial.dump

vector:
	riscv64-unknown-elf-g++  -Wall -flax-vector-conversions  -march=rv64g -O2 -c -o  src/jacobi-2d.o src/jacobi-2d.cpp; \
	riscv64-unknown-elf-g++  -Wall -flax-vector-conversions  -march=rv64g -O2 -o bin/jacobi-2d_vector.exe src/*.o -lm ; \
	rm src/*.o ; \
	riscv64-unknown-elf-objdump  -dC -ds   bin/jacobi-2d_vector.exe > bin/jacobi-2d_vector.dump ; \

host:
	g++ -O2 -g -Wall -flax-vector-conversions -std=c99 -o -c src/jacobi-2d.o  src/jacobi-2d.cpp ; \
	g++ -O2 -g -Wall -flax-vector-conversions -std=c99 -o  bin/jacobi-2d_host.exe  src/*.o  -lm; \
	rm src/*.o; \
	objdump -dC bin/jacobi-2d_host.exe > bin/jacobi-2d_host.dump ;

copy:
	rm -r ../../Test/_jacobi-2d/bin/*.exe; \
	cp -r bin/*exe ../../Test/_jacobi-2d/bin/;

clean:
	rm bin/*.dump; \
	rm bin/*.exe;