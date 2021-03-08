.PHONY: clean all

ifeq ($(OS),Windows_NT)
    detected_OS := Windows
else
    detected_OS := $(shell sh -c 'uname 2>/dev/null || echo Unknown')
endif

CC=gcc
AR=ar

DEP_LIBS:=
SHARED_LIB_NAME:=mlx90641_driver_mcp2221
STATIC_LIB_NAME:=mlx90641_driver_mcp2221_static

STATIC_LIB_FILE:=lib$(STATIC_LIB_NAME).a
SHARED_LIB_FILE:=lib$(SHARED_LIB_NAME).so

LIB_RESULT_DIR:=../libs

ifeq ($(detected_OS),Windows)
MACHINE:=windows
SHARED_LIB_FILE:=$(SHARED_LIB_NAME).dll
DEP_LIBS+=-lhidapi
else
MACHINE:=$(shell uname -m)
DEP_LIBS+=-ludev
DEP_LIBS+=-lusb-1.0
DEP_LIBS+=-lhidapi-hidraw
endif

CFLAGS:=-Wall -Wextra -Wstrict-prototypes -Wunused-result -O3 -std=c99 -fmessage-length=0 -fPIC -I../libs -Iinc
CFLAGS+=$(CFLAGS_EXTRA)


all: $(SHARED_LIB_FILE) $(STATIC_LIB_FILE)

clean:
	rm -f main *.o
	rm -f src/*.o
	rm -f *.exe
	rm -f $(SHARED_LIB_FILE) $(STATIC_LIB_FILE) hidapi.dll

main.o: main.c
	$(CC) -c -o $@ $(CFLAGS) $^

src/mcp2221_hidapi.o: src/mcp2221_hidapi.c
	$(CC) -c -o $@ $(CFLAGS) $^

src/MLX90641_I2C_Driver_mcp2221.o: src/MLX90641_I2C_Driver_mcp2221.c
	$(CC) -c -o $@ $(CFLAGS) $^

main: main.o src/MLX90641_I2C_Driver_mcp2221.o src/mcp2221_hidapi.o
	$(CC) -o $@ $^ -lm -Wl,-rpath=$(LIB_RESULT_DIR)/$(MACHINE) -L$(LIB_RESULT_DIR)/$(MACHINE) $(DEP_LIBS)
	@cp -rvf $(LIB_RESULT_DIR)/windows/hidapi.dll .


$(SHARED_LIB_FILE): src/MLX90641_I2C_Driver_mcp2221.o src/mcp2221_hidapi.o
	$(CC) -shared -o $@ $^ -L$(LIB_RESULT_DIR)/$(MACHINE) -L. $(DEP_LIBS) -Wl,-rpath=$(LIB_RESULT_DIR)/$(MACHINE) -Wl,-rpath=.
	@mkdir -p $(LIB_RESULT_DIR)/$(MACHINE)
	find .. -type f
	@cp -fv $@ $(LIB_RESULT_DIR)/$(MACHINE)

$(STATIC_LIB_FILE): src/MLX90641_I2C_Driver_mcp2221.o src/mcp2221_hidapi.o
	$(AR) rcu $@ $<
	@mkdir -p $(LIB_RESULT_DIR)/$(MACHINE)
	@cp -fv $@ $(LIB_RESULT_DIR)/$(MACHINE)
