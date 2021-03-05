#makefile
GCC_TOOLCHAIN_DIR := /newlib/
SYSROOT_DIR := $(GCC_TOOLCHAIN_DIR)/riscv64-unknown-elf

LLVM := /llvm/build/
SPIKE := spike
PK := pk

QEMU := qemu-riscv64

target = bin/rvv-test

serial:
	riscv64-unknown-elf-g++     -O2 -o $(target) src/streamcluster.cpp -lm
	riscv64-unknown-elf-objdump -D ./bin/rvv-test > ./bin/dump_serial

vector:
	riscv64-unknown-elf-g++        -O2 -o $(target)  src/streamcluster.cpp -lm
	riscv64-unknown-elf-objdump -D ./bin/rvv-test > ./bin/dump_vector

runspike :
	echo >output.txt; \
	$(SPIKE) --isa=rv64gcv $(PK) $(target) 3 10 128 128 128 10 none output.txt 1

runqemu:
	echo >output.txt; \
	$(QEMU) -cpu rv64,x-v=true,vlen=256,elen=64,vext_spec=v1.0 $(target) 3 10 128 128 128 10 none output.txt 1
