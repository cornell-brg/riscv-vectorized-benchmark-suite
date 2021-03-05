#makefile

GCC_TOOLCHAIN_DIR := /newlib/
SYSROOT_DIR := $(GCC_TOOLCHAIN_DIR)/riscv64-unknown-elf

LLVM := /llvm/build/
SPIKE := spike
PK := pk

QEMU := qemu-riscv64

target = bin/rvv-test

start:
	mkdir bin; \
	rm bin/*.*

serial:
	for entry in src/*.cpp ; do \
		riscv64-unknown-elf-g++   -Wall -flax-vector-conversions  -c  -O2 -o   $$entry.o  $$entry ; \
	done
	riscv64-unknown-elf-g++   -Wall -flax-vector-conversions       -O2  -o $(target)  src/*.cpp.o -lm;	\
	rm src/*.o; \
	riscv64-unknown-elf-objdump -D --mattr=+experimental-v ./bin/rvv-test > ./bin/dump_serial


vector:
	riscv64-unknown-elf-g++  -Wall -flax-vector-conversions   -c   -O2 -o  src/CumNormalInv.o src/CumNormalInv.cpp
	riscv64-unknown-elf-g++  -Wall -flax-vector-conversions   -c   -O2 -o  src/MaxFunction.o src/MaxFunction.cpp	
	riscv64-unknown-elf-g++  -Wall -flax-vector-conversions   -c  -O2 -o  src/RanUnif.o src/RanUnif.cpp
	riscv64-unknown-elf-g++  -Wall -flax-vector-conversions   -c   -O2 -o  src/nr_routines.o src/nr_routines.cpp
	riscv64-unknown-elf-g++  -Wall -flax-vector-conversions   -c  -O2 -o  src/icdf.o src/icdf.cpp
	riscv64-unknown-elf-g++  -Wall -flax-vector-conversions   -c   -O2 -o  src/HJM_SimPath_Forward_Blocking.o src/HJM_SimPath_Forward_Blocking.cpp
	riscv64-unknown-elf-g++  -Wall -flax-vector-conversions   -c  -O2 -o  src/HJM.o src/HJM.cpp
	riscv64-unknown-elf-g++  -Wall -flax-vector-conversions   -c  -O2 -o  src/HJM_Swaption_Blocking.o src/HJM_Swaption_Blocking.cpp
	riscv64-unknown-elf-g++  -Wall -flax-vector-conversions   -c  -O2 -o  src/HJM_Securities.o src/HJM_Securities.cpp
	riscv64-unknown-elf-g++  -Wall -flax-vector-conversions     -O2   -o $(target)  src/*.o -lm
	rm src/*.o
	riscv64-unknown-elf-objdump -dC ./bin/rvv-test > ./bin/dump_vector

runspike :
	$(SPIKE) --isa=rv64gcv $(PK) $(target) -ns 8 -sm 512 -nt 1

runqemu:
	$(QEMU) -cpu rv64,x-v=true,vlen=256,elen=64,vext_spec=v1.0 $(target) -ns 8 -sm 512 -nt 1

clean:
	rm bin/*
