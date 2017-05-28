#S9XDEBUGGER=1
#S9XNETPLAY=1
#S9XZIP=1
S9XJMA=1
#SYSTEM_ZIP=1

# Fairly good and special-char-safe descriptor of the os being built on.
OS         = `uname -s -r -m|sed \"s/ /-/g\"|tr \"[A-Z]\" \"[a-z]\"|tr \"/()\" \"___\"`
BUILDDIR   = .

OBJECTS    = ../apu/apu.o ../apu/bapu/dsp/sdsp.o ../apu/bapu/dsp/SPC_DSP.o ../apu/bapu/smp/smp.o ../apu/bapu/smp/smp_state.o ../bsx.o ../c4.o ../c4emu.o ../cheats.o ../cheats2.o ../clip.o ../conffile.o ../controls.o ../cpu.o ../cpuexec.o ../cpuops.o ../crosshairs.o ../dma.o ../dsp.o ../dsp1.o ../dsp2.o ../dsp3.o ../dsp4.o ../fxinst.o ../fxemu.o ../gfx.o ../globals.o ../logger.o ../memmap.o ../msu1.o ../movie.o ../obc1.o ../ppu.o ../stream.o ../sa1.o ../sa1cpu.o ../screenshot.o ../sdd1.o ../sdd1emu.o ../seta.o ../seta010.o ../seta011.o ../seta018.o ../snapshot.o ../snes9x.o ../spc7110.o ../srtc.o ../tile.o ../filter/2xsai.o ../filter/blit.o ../filter/epx.o ../filter/hq2x.o ../filter/snes_ntsc.o ../statemanager.o unix.o x11.o
DEFS       = -DMITSHM

ifdef S9XDEBUGGER
OBJECTS   += ../debug.o ../fxdbg.o
endif

ifdef S9XNETPLAY
OBJECTS   += ../netplay.o ../server.o
endif

ifdef S9XZIP
OBJECTS   += ../loadzip.o
ifndef SYSTEM_ZIP
OBJECTS   += ../unzip/ioapi.o ../unzip/unzip.o
INCLUDES   = -I../unzip/
endif
endif

ifdef S9XJMA
OBJECTS   += ../jma/7zlzma.o ../jma/crc32.o ../jma/iiostrm.o ../jma/inbyte.o ../jma/jma.o ../jma/lzma.o ../jma/lzmadec.o ../jma/s9x-jma.o ../jma/winout.o
endif

CCC        = em++
CC         = emcc
GASM       = em++
INCLUDES   += -I. -I.. -I../apu/ -I../apu/bapu -I../jma/ -I../filter/ -I/home/matt/.emscripten_ports/zlib/zlib-version_1/ -I/home/matt/.emscripten_ports/sdl2-image/SDL2_image-version_3/external/libpng-1.6.2/

#CCFLAGS    = -g4 -O0 -fomit-frame-pointer -fno-exceptions -fno-rtti -pedantic -Wall -W -Wno-unused-parameter  -DJOYSTICK_SUPPORT -DZLIB -DJMA_SUPPORT -DHAVE_LIBPNG -DHAVE_MKSTEMP -DHAVE_STRINGS_H -DHAVE_SYS_IOCTL_H -DHAVE_STDINT_H -DRIGHTSHIFT_IS_SAR -DUSE_XVIDEO -DUSE_THREADS $(DEFS)
# Removed flags: -DJOYSTICK_SUPPORT -DUSE_XVIDEO -DUSE_THREADS -fno-rtti
# Added flags: -DNOSOUND
CCFLAGS    = -fomit-frame-pointer -fno-exceptions -pedantic -Wall -W -Wno-unused-parameter -DZLIB -DJMA_SUPPORT -DHAVE_LIBPNG -DHAVE_MKSTEMP -DHAVE_STRINGS_H -DHAVE_SYS_IOCTL_H -DHAVE_STDINT_H -DRIGHTSHIFT_IS_SAR -DNOSOUND $(DEFS)
CFLAGS     = -std=c11 $(CCFLAGS)

.SUFFIXES: .o .cpp .c .cc .h .m .i .s .obj

all: Makefile index.js
	mv index.js static/index.js

index.js: $(OBJECTS)
	#-lm -lz -lpng -lSM -lICE -lX11 -lXext -lXv
	$(CCC) $(LDFLAGS) $(INCLUDES) -o $@ $(OBJECTS) -g4 -O0 --bind -lpthread -s WASM=1 -s USE_SDL=2 -s USE_ZLIB=1 -s USE_LIBPNG=1 -s FORCE_FILESYSTEM=1 -s ASSERTIONS=1 -s ALLOW_MEMORY_GROWTH=1 -s ABORTING_MALLOC=0

../jma/s9x-jma.o: ../jma/s9x-jma.cpp
	$(CCC) $(INCLUDES) -c $(CCFLAGS) -fexceptions $*.cpp -o $@
../jma/7zlzma.o: ../jma/7zlzma.cpp
	$(CCC) $(INCLUDES) -c $(CCFLAGS) -fexceptions $*.cpp -o $@
../jma/crc32.o: ../jma/crc32.cpp
	$(CCC) $(INCLUDES) -c $(CCFLAGS) -fexceptions $*.cpp -o $@
../jma/iiostrm.o: ../jma/iiostrm.cpp
	$(CCC) $(INCLUDES) -c $(CCFLAGS) -fexceptions $*.cpp -o $@
../jma/inbyte.o: ../jma/inbyte.cpp
	$(CCC) $(INCLUDES) -c $(CCFLAGS) -fexceptions $*.cpp -o $@
../jma/jma.o: ../jma/jma.cpp
	$(CCC) $(INCLUDES) -c $(CCFLAGS) -fexceptions $*.cpp -o $@
../jma/lzma.o: ../jma/lzma.cpp
	$(CCC) $(INCLUDES) -c $(CCFLAGS) -fexceptions $*.cpp -o $@
../jma/lzmadec.o: ../jma/lzmadec.cpp
	$(CCC) $(INCLUDES) -c $(CCFLAGS) -fexceptions $*.cpp -o $@
../jma/winout.o: ../jma/winout.cpp
	$(CCC) $(INCLUDES) -c $(CCFLAGS) -fexceptions $*.cpp -o $@

.cpp.o:
	$(CCC) $(INCLUDES) -c $(CCFLAGS) $*.cpp -o $@

.c.o:
	$(CC) $(INCLUDES) -c $(CCFLAGS) $*.c -o $@

.cpp.S:
	$(GASM) $(INCLUDES) -S $(CCFLAGS) $*.cpp -o $@

.cpp.i:
	$(GASM) $(INCLUDES) -E $(CCFLAGS) $*.cpp -o $@

.S.o:
	$(GASM) $(INCLUDES) -c $(CCFLAGS) $*.S -o $@

.S.i:
	$(GASM) $(INCLUDES) -c -E $(CCFLAGS) $*.S -o $@

.s.o:
	@echo Compiling $*.s
	sh-elf-as -little $*.s -o $@

.obj.o:
	cp $*.obj $*.o

clean:
	rm -f static/index.js
	rm -f index.js.map
	rm -f index.wasm
	rm -f index.wast
	rm -f $(OBJECTS)
