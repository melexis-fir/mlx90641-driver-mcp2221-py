.PHONY: clean all

ifeq ($(OS),Windows_NT)
    detected_OS := Windows
else
    detected_OS := $(shell sh -c 'uname 2>/dev/null || echo Unknown')
endif


DEP_LIBS:=
SHARED_LIB_NAME:=mlx90641_driver_mcp2221
STATIC_LIB_NAME:=mlx90641_driver_mcp2221_static

STATIC_LIB_FILE:=lib$(STATIC_LIB_NAME).a
SHARED_LIB_FILE:=lib$(SHARED_LIB_NAME).so

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


all: $(SHARED_LIB_FILE) $(STATIC_LIB_FILE)

clean:
	rm -f main *.o
	rm -f src/*.o
	rm -f $(SHARED_LIB_FILE) $(STATIC_LIB_FILE) hidapi.dll

main.o: main.c
	gcc -c -o $@ $(CFLAGS) $^

src/mcp2221_hidapi.o: src/mcp2221_hidapi.c
	gcc -c -o $@ $(CFLAGS) $^

src/MLX90641_I2C_Driver_mcp2221.o: src/MLX90641_I2C_Driver_mcp2221.c
	gcc -c -o $@ $(CFLAGS) $^

main: main.o src/MLX90641_I2C_Driver_mcp2221.o src/mcp2221_hidapi.o
	gcc -o $@ $^ -lm -Wl,-rpath=../libs/$(MACHINE) -L../libs/$(MACHINE) $(DEP_LIBS)
	@cp -rvf ../libs/windows/hidapi.dll .


$(SHARED_LIB_FILE): src/MLX90641_I2C_Driver_mcp2221.o src/mcp2221_hidapi.o
	gcc -shared -o $@ $^ -L../libs/$(MACHINE) $(DEP_LIBS) -Wl,-rpath=../libs/$(MACHINE)
	@mkdir -p ../libs/$(MACHINE)
	@cp -fv $@ ../libs/$(MACHINE)

$(STATIC_LIB_FILE): src/MLX90641_I2C_Driver_mcp2221.o src/mcp2221_hidapi.o
	ar rcu $@ $<
	@mkdir -p ../libs/$(MACHINE)
	@cp -fv $@ ../libs/$(MACHINE)
