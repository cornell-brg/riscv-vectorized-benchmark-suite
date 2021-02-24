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
	${GCC_RISCV}/bin/riscv64-unknown-elf-gcc  -Wall -flax-vector-conversions     -c  -O2 -o src/particlefilter_serial.o src/particlefilter.c
	${GCC_RISCV}/bin/riscv64-unknown-elf-gcc  -Wall -flax-vector-conversions      -O2 -o $(target) src/particlefilter_serial.o -lm
	rm src/*.o

vector:
	${GCC_RISCV}/bin/riscv64-unknown-elf-gcc  -Wall -flax-vector-conversions  -DMVL_$$mvl  -c     -O2 -o  src/particlefilter.o src/particlefilter.c ; \
	${GCC_RISCV}/bin/riscv64-unknown-elf-gcc  -Wall -flax-vector-conversions  -DMVL_$$mvl     -O2 -o $(target) src/*.o -lm ; \
	rm src/*.o 

runspike :
	$(SPIKE) --isa=rv64gcv $(PK) $(target) -x 128 -y 128 -z 2 -np 256

runqemu:
	$(QEMU) -cpu rv64,x-v=true,vlen=256,elen=64,vext_spec=v1.0 $(target) -x 128 -y 128 -z 2 -np 256

clean:
	rm bin/*

	
