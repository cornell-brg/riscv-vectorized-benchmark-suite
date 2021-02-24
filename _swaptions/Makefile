#makefile

GCC_TOOLCHAIN_DIR := /newlib/
SYSROOT_DIR := $(GCC_TOOLCHAIN_DIR)/riscv64-unknown-elf

LLVM := /llvm/build/
SPIKE := spike
PK := pk

target = bin/rvv-test

start:
	mkdir bin; \
	rm bin/*.*

serial:
	for entry in src/*.cpp ; do \
		${GCC_RISCV}/bin/riscv64-unknown-elf-g++   -Wall -flax-vector-conversions  -c  -O2 -o   $$entry.o  $$entry ; \
	done
	${GCC_RISCV}/bin/riscv64-unknown-elf-g++   -Wall -flax-vector-conversions       -O2  -o $(target)  src/*.cpp.o -lm;	\
	rm src/*.o; \
	${GCC_RISCV}/bin/riscv64-unknown-elf-objdump -D --mattr=+experimental-v ./bin/rvv-test > ./bin/dump_serial


vector:
	${GCC_RISCV}/bin/riscv64-unknown-elf-g++  -Wall -flax-vector-conversions   -c   -O2 -o  src/CumNormalInv.o src/CumNormalInv.cpp
	${GCC_RISCV}/bin/riscv64-unknown-elf-g++  -Wall -flax-vector-conversions   -c   -O2 -o  src/MaxFunction.o src/MaxFunction.cpp	
	${GCC_RISCV}/bin/riscv64-unknown-elf-g++  -Wall -flax-vector-conversions   -c  -O2 -o  src/RanUnif.o src/RanUnif.cpp
	${GCC_RISCV}/bin/riscv64-unknown-elf-g++  -Wall -flax-vector-conversions   -c   -O2 -o  src/nr_routines.o src/nr_routines.cpp
	${GCC_RISCV}/bin/riscv64-unknown-elf-g++  -Wall -flax-vector-conversions   -c  -O2 -o  src/icdf.o src/icdf.cpp
	${GCC_RISCV}/bin/riscv64-unknown-elf-g++  -Wall -flax-vector-conversions   -c   -O2 -o  src/HJM_SimPath_Forward_Blocking.o src/HJM_SimPath_Forward_Blocking.cpp
	${GCC_RISCV}/bin/riscv64-unknown-elf-g++  -Wall -flax-vector-conversions   -c  -O2 -o  src/HJM.o src/HJM.cpp
	${GCC_RISCV}/bin/riscv64-unknown-elf-g++  -Wall -flax-vector-conversions   -c  -O2 -o  src/HJM_Swaption_Blocking.o src/HJM_Swaption_Blocking.cpp
	${GCC_RISCV}/bin/riscv64-unknown-elf-g++  -Wall -flax-vector-conversions   -c  -O2 -o  src/HJM_Securities.o src/HJM_Securities.cpp
	${GCC_RISCV}/bin/riscv64-unknown-elf-g++  -Wall -flax-vector-conversions     -O2   -o $(target)  src/*.o -lm
	rm src/*.o
	${GCC_RISCV}/bin/riscv64-unknown-elf-objdump -dC ./bin/rvv-test > ./bin/dump_vector

runspike :
	$(SPIKE) --isa=rv64gcv $(PK) $(target) -ns 8 -sm 512 -nt 1

runqemu:
	$(QEMU) -cpu rv64,x-v=true,vlen=256,elen=64,vext_spec=v1.0 $(target) -ns 8 -sm 512 -nt 1

clean:
	rm bin/*
