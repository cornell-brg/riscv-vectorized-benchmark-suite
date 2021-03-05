#makefile
GCC_TOOLCHAIN_DIR := /newlib/
SYSROOT_DIR := $(GCC_TOOLCHAIN_DIR)/riscv64-unknown-elf

LLVM := /llvm/build/
SPIKE := spike
PK := pk

QEMU := qemu-riscv64

target = bin/rvv-test

serial:
	riscv64-unknown-elf-gcc   -Wall -flax-vector-conversions   -O2  -c  -o src/axpy.o src/axpy.c
	riscv64-unknown-elf-gcc   -Wall -flax-vector-conversions   -O2  -c -o src/main.o src/main.c
	riscv64-unknown-elf-gcc   -Wall -flax-vector-conversions    -O2  -c -o src/utils.o src/utils.c
	riscv64-unknown-elf-gcc   -Wall -flax-vector-conversions    -O2 -o $(target) src/*.o -lm

vector:
	riscv64-unknown-elf-gcc   -Wall -flax-vector-conversions    -O2  -c  -o src/utils.o src/utils.c
	riscv64-unknown-elf-gcc   -Wall -flax-vector-conversions   -O2   -c -o src/axpy.o src/axpy.c
	riscv64-unknown-elf-gcc   -Wall -flax-vector-conversions    -O2  -c -o src/main.o src/main.c
	riscv64-unknown-elf-gcc   -Wall -flax-vector-conversions   -O2    -o $(target) src/*.o -lm  
	
runspike :
	$(SPIKE) --isa=rv64gcv $(PK) $(target) 256

runqemu:
	$(QEMU) -cpu rv64,x-v=true,vlen=256,elen=64,vext_spec=v1.0 $(target)