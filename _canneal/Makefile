#makefile
GCC_TOOLCHAIN_DIR := /newlib/
SYSROOT_DIR := $(GCC_TOOLCHAIN_DIR)/riscv64-unknown-elf

LLVM := /llvm/build/
SPIKE := spike
PK := pk

QEMU := qemu-riscv64

target = bin/rvv-test

start:
	mkdir bin ; \
	rm bin/*.*;

serial:
	for entry in src/*.cpp ; do \
		riscv64-unknown-elf-g++  -Wall -flax-vector-conversions    -c   -O2 -o   $$entry.o  $$entry ; \
	done
	riscv64-unknown-elf-g++ -Wall -flax-vector-conversions      -O2  -o $(target) src/*.cpp.o -lm
	rm src/*.o

vector:
	riscv64-unknown-elf-g++ -Wall -flax-vector-conversions  -c    -O2 -o  src/annealer_thread.o src/annealer_thread.cpp
	riscv64-unknown-elf-g++ -Wall -flax-vector-conversions  -c   -O2 -o  src/main.o src/main.cpp
	riscv64-unknown-elf-g++ -Wall -flax-vector-conversions  -c   -O2 -o  src/netlist.o src/netlist.cpp
	riscv64-unknown-elf-g++ -Wall -flax-vector-conversions  -c   -O2 -o  src/netlist_elem.o src/netlist_elem.cpp
	riscv64-unknown-elf-g++ -Wall -flax-vector-conversions  -c   -O2 -o  src/rng.o src/rng.cpp
	riscv64-unknown-elf-g++ -Wall -flax-vector-conversions     -O2  -o bin/canneal_vector.exe src/*.o -lm
	rm src/*.o

runspike :
	$(SPIKE) --isa=rv64gcv $(PK) $(target) 1 100 300 input/100.nets 8

runqemu:
	$(QEMU) -cpu rv64,x-v=true,vlen=256,elen=64,vext_spec=v1.0 $(target) 1 100 300 input/100.nets 8

clean:
	rm bin/*
