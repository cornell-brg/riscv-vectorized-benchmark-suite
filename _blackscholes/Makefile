#makefile
GCC_TOOLCHAIN_DIR := /newlib/
SYSROOT_DIR := $(GCC_TOOLCHAIN_DIR)/riscv64-unknown-elf

LLVM := /llvm/build/
SPIKE := spike
PK := pk

target = bin/rvv-test

serial:
	${GCC_RISCV}/bin/riscv64-unknown-elf-g++     -O2 -o $(target) src/blackscholes.cpp -lm

vector:
	${GCC_RISCV}/bin/riscv64-unknown-elf-g++        -O2 -o $(target)  src/blackscholes.cpp -lm

runspike :
	$(SPIKE) --isa=rv64gcv $(PK) $(target) 1 ./input/in_64K.input prices.txt

runqemu:
	$(QEMU) -cpu rv64,x-v=true,vlen=256,elen=64,vext_spec=v1.0 $(target) 1 $(shell pwd)/input/in_64K.input prices.txt