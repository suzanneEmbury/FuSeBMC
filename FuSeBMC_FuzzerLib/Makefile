#CLANG_BASE = /home/kaled/clang_base
#LLVM_BASE = /mnt/sda2/llvm-6.0.0
#CXX = $(CLANG_BASE)/bin/clang
$(info concurrency="$(concurrency)")
CXX= gcc
#
CXXFLAGS = -std=gnu11 -Wall -Wextra -W  -I./include
#CXXFLAGS += -I$(CLANG_BASE)/include -I$(LLVM_BASE)/include
CXXFLAGS += -Wno-unused-parameter
ifeq ($(concurrency), 1)
    CXXFLAGS += -DFUSEBMC_CONCURRENCY
    LIB32 = libFuSeBMC_FuzzerLib_c_32.a
    LIB64 = libFuSeBMC_FuzzerLib_c_64.a
else
    LIB32 = libFuSeBMC_FuzzerLib_32.a
    LIB64 = libFuSeBMC_FuzzerLib_64.a
endif
#LDFLAGS = -L$(CLANG_BASE)/lib -L$(LLVM_BASE)/lib
#LDLIBS = 

SRC_DIR = src
INCLUDE_DIR = include
OBJ_DIR32 = obj32
OBJ_DIR64 = obj64
HEADER = $(wildcard $(INCLUDE_DIR)/*.h)
SRC = $(wildcard $(SRC_DIR)/*.c)
OBJ32 = $(SRC:$(SRC_DIR)/%.c=$(OBJ_DIR32)/%.o)
OBJ64 = $(SRC:$(SRC_DIR)/%.c=$(OBJ_DIR64)/%.o)
OTHER_HEADERS = $(INCLUDE_DIR)/config.h

.PHONY: all clean debug32 release32 debug64 release64
debug32: CXXFLAGS += -g -o0 -DMYDEBUG -m32
debug64: CXXFLAGS += -g -o0 -DMYDEBUG -m64

debug32: $(LIB32)
debug64: $(LIB64)

release32: CXXFLAGS += -o3 -os -m32
release64: CXXFLAGS += -o3 -os -m64

release32: $(LIB32)
release64: $(LIB64)
all: release32 release64
$(OBJ_DIR32)/%.o: $(SRC_DIR)/%.c $(INCLUDE_DIR)/%.h $(OTHER_HEADERS)
	$(CXX) $(CXXFLAGS) $(CFLAGS) -DARCH32 -c $< -o $@
$(OBJ_DIR64)/%.o: $(SRC_DIR)/%.c $(INCLUDE_DIR)/%.h $(OTHER_HEADERS)
	$(CXX) $(CXXFLAGS) $(CFLAGS) -DARCH64 -c $< -o $@
$(LIB32): $(OBJ32)
	ar rcs $@ $^
$(LIB64): $(OBJ64)
	ar rcs $@ $^

clean:
	rm -f $(OBJ_DIR32)/*.o
	rm -f $(OBJ_DIR64)/*.o
	rm -f $(LIB32)
	rm -f $(LIB64)
