clapack.tgz taken from http://www.netlib.org/clapack/clapack.tgz

## Setup

`cd F2CLIBS/libf2c && make f2c.h signal1.h sysdep1.h arith.h` has been to generate missing headers.

To statically link this into your program, you will need to compile the following files:
```
LAPACK_FILTER_OUT := $(LAPACK_PATH)/F2CLIBS/libf2c/arithchk.c
LAPACK_FILTER_OUT += $(LAPACK_PATH)/F2CLIBS/libf2c/main.c
LAPACK_FILTER_OUT += $(LAPACK_PATH)/F2CLIBS/libf2c/uninit.c
LAPACK_FILTER_OUT += $(LAPACK_PATH)/SRC/xerbla.c
LAPACK_FILTER_OUT += $(LAPACK_PATH)/SRC/xerbla_array.c

SRC += $(wildcard $(LAPACK_DIR)/BLAS/SRC/*.c)
SRC += $(filter-out $(LAPACK_FILTER_OUT), $(wildcard $(LAPACK_DIR)/SRC/*.c))
SRC += $(filter-out $(LAPACK_FILTER_OUT), $(wildcard $(LAPACK_DIR)/F2CLIBS/libf2c/*.c))
SRC += $(LAPACK_PATH)/INSTALL/dlamch.c
SRC += $(LAPACK_PATH)/INSTALL/dsecnd.c
SRC += $(LAPACK_PATH)/INSTALL/second.c
SRC += $(LAPACK_PATH)/INSTALL/slamch.c

CFLAGS += -I$(LAPACK_PATH)/INCLUDE
```

For a baremetal armv7e-m target, you can use the following config:
```make
CFLAGS += -DNO_LONG_LONG -DNO_TRUNCATE -DINTEGER_STAR_8

# optional: do not add f2c_ prefix to blas function names
CFLAGS += -DNO_BLAS_WRAP
```

Be sure to set up your compiler and linker to remove unused symbols/code, otherwise the binary will be huge. For gcc
```make
CFLAGS += -ffunction-sections -fdata-sections
LDFLAGS += -Wl,-gc-sections
```

Good luck with the compiler warnings...

For more options and infos, check documentation pdfs and readmes.
