## -*- Makefile -*-
## mingw Makefile for NRage Project64 Input Plugin
##
## Build can be configured to a small amount with environment variables.
## Set BUILD_ARCH to x86 for a 32 bit build or x86_64 for a 64 bit build.
## If BUILD_ARCH is not set, we will attempt to detect the architecture.
## Set BUILD_DEBUG to YES for a debug build. Anything else for a release build.


#### Compiler and tool definitions shared by all build targets #####
CC = gcc
CXX = g++
AR = ar
WINDRES = windres
CXXFLAGS = $(BASICOPTS) -Wno-write-strings
LDFLAGS = -static-libstdc++ -static-libgcc


# Cfg
ifeq ($(BUILD_ARCH),)
GCC_ARCH = $(shell gcc -dumpmachine | sed s/-.*//)
ifeq ($(BUILD_ARCH),x86_64)
BUILD_ARCH = x86_64
else
BUILD_ARCH = x86
endif
endif

ifneq ($(BUILD_DEBUG),YES)
BUILD_DEBUG = NO
PLUGIN_FILE = PJ64_NRage_m.dll
RESFLAGS =
BASICOPTS = -O3
else
BUILD_DEBUG = YES
PLUGIN_FILE = PJ64_NRage_md.dll
RESFLAGS = -D_DEBUG
BASICOPTS = -g -D_DEBUG
endif

# Define the target directories.
#OBJDIR=obj
#BUILDDIR=build
DXLIBDIR32=../3rd\ Party/directx/lib
DXLIBDIR64=../3rd\ Party/directx/lib64
OBJDIR32=../../build/Release_mingw32/PJ64_NRage
OBJDIR64=../../build/Release_mingw64/PJ64_NRage
OBJDIR32DEBUG=../../build/Debug_mingw32/PJ64_NRage
OBJDIR64DEBUG=../../build/Debug_mingw64/PJ64_NRage
BUILDDIR32=../../Plugin/Input
BUILDDIR64=../../Plugin64/Input
ifeq ($(BUILD_ARCH),x86_64)
DXLIBDIR=$(DXLIBDIR64)
BUILDDIR=$(BUILDDIR64)
ifeq ($(BUILD_DEBUG),YES)
OBJDIR=$(OBJDIR64DEBUG)
else
OBJDIR=$(OBJDIR64)
endif
else
DXLIBDIR=$(DXLIBDIR32)
BUILDDIR=$(BUILDDIR32)
ifeq ($(BUILD_DEBUG),YES)
OBJDIR=$(OBJDIR32DEBUG)
else
OBJDIR=$(OBJDIR32)
endif
endif

all: $(BUILDDIR)/$(PLUGIN_FILE)

release: all

#debug: debugvar $(OBJDIR)/Debug.o $(BUILDDIR)/PJ64_NRage_d.dll

#debugvar:
#	$(eval BASICOPTS := -g -D_DEBUG)

## Target: PJ64_NRage_m.dll
OBJS =  \
	$(OBJDIR)/mingw.o \
	$(OBJDIR)/XInputController.o \
	$(OBJDIR)/PakIO.o \
	$(OBJDIR)/NRagePluginV2.o \
	$(OBJDIR)/International.o \
	$(OBJDIR)/Interface.o \
	$(OBJDIR)/GBCart.o \
	$(OBJDIR)/FileAccess.o \
	$(OBJDIR)/DirectInput.o \
	$(OBJDIR)/resource.o \
	$(OBJDIR)/dinput8.a

DEBUGOBJS =	\
	$(OBJDIR)/Debug.o

# Link or archive
$(BUILDDIR)/PJ64_NRage_m.dll: $(BUILDDIR) $(OBJDIR) $(OBJS) $(DEPLIBS)
	$(CXX) -shared $(CXXFLAGS) $(CPPFLAGS) -o $@ $(OBJS) $(LDLIBS) $(LDFLAGS)

$(BUILDDIR)/PJ64_NRage_md.dll: $(BUILDDIR) $(DEBUGOBJS) $(OBJDIR) $(OBJS) $(DEPLIBS)
	$(CXX) -shared $(CXXFLAGS) $(CPPFLAGS) -o $@ $(DEBUGOBJS) $(OBJS) $(LDLIBS) $(LDFLAGS)


# Compile source files into .o files

$(OBJDIR)/mingw.o: $(OBJDIR) mingw.cpp
	$(CXX) $(CXXFLAGS) $(CPPFLAGS) -o $@ -c mingw.cpp

$(OBJDIR)/XInputController.o: $(OBJDIR) XInputController.cpp
	$(CXX) $(CXXFLAGS) $(CPPFLAGS) -o $@ -c XInputController.cpp

$(OBJDIR)/PakIO.o: $(OBJDIR) PakIO.cpp
	$(CXX) $(CXXFLAGS) $(CPPFLAGS) -o $@ -c PakIO.cpp

$(OBJDIR)/NRagePluginV2.o: $(OBJDIR) NRagePluginV2.cpp
	$(CXX) $(CXXFLAGS) $(CPPFLAGS) -o $@ -c NRagePluginV2.cpp

$(OBJDIR)/International.o: $(OBJDIR) International.cpp
	$(CXX) $(CXXFLAGS) $(CPPFLAGS) -o $@ -c International.cpp

$(OBJDIR)/Interface.o: $(OBJDIR) Interface.cpp
	$(CXX) $(CXXFLAGS) $(CPPFLAGS) -o $@ -c Interface.cpp	

$(OBJDIR)/GBCart.o: $(OBJDIR) GBCart.cpp
	$(CXX) $(CXXFLAGS) $(CPPFLAGS) -o $@ -c GBCart.cpp

$(OBJDIR)/FileAccess.o: $(OBJDIR) FileAccess.cpp
	$(CXX) $(CXXFLAGS) $(CPPFLAGS) -o $@ -c FileAccess.cpp

$(OBJDIR)/DirectInput.o: $(OBJDIR) DirectInput.cpp
	$(CXX) $(CXXFLAGS) $(CPPFLAGS) -o $@ -c DirectInput.cpp

$(OBJDIR)/Debug.o: $(OBJDIR) Debug.cpp
	$(CXX) $(CXXFLAGS) $(CPPFLAGS) -o $@ -c Debug.cpp

$(OBJDIR)/resource.o: $(OBJDIR) NRagePluginV2.rc
	$(WINDRES) $(RESFLAGS) NRagePluginV2.rc $@


# dinput8 stuff

$(OBJDIR)/dinput8/dinput8.lib: $(OBJDIR)/dinput8 $(DXLIBDIR)/dinput8.lib
	cp $(DXLIBDIR)/dinput8.lib $(OBJDIR)/dinput8/dinput8.lib
	cd $(OBJDIR)/dinput8 ; touch empty ; $(AR) r dinput8.lib empty ; rm empty ; $(AR) d dinput8.lib empty

$(OBJDIR)/dinput8: $(OBJDIR)
	mkdir -p $(OBJDIR)/dinput8

$(OBJDIR)/dinput8/mouse.o: $(OBJDIR)/dinput8/dinput8.lib
	cd $(OBJDIR)/dinput8 ; $(AR) x dinput8.lib dilib1.obj ; mv dilib1.obj mouse.o

$(OBJDIR)/dinput8/mouse2.o: $(OBJDIR)/dinput8/dinput8.lib
	cd $(OBJDIR)/dinput8 ; $(AR) x dinput8.lib dilib5.obj ; mv dilib5.obj mouse2.o

$(OBJDIR)/dinput8/keyboard.o: $(OBJDIR)/dinput8/dinput8.lib
	cd $(OBJDIR)/dinput8 ; $(AR) x dinput8.lib dilib2.obj ; mv dilib2.obj keyboard.o

$(OBJDIR)/dinput8/joystick.o: $(OBJDIR)/dinput8/dinput8.lib
	cd $(OBJDIR)/dinput8 ; $(AR) x dinput8.lib dilib3.obj ; mv dilib3.obj joystick.o

$(OBJDIR)/dinput8/joystick2.o: $(OBJDIR)/dinput8/dinput8.lib
	cd $(OBJDIR)/dinput8 ; $(AR) x dinput8.lib dilib4.obj ; mv dilib4.obj joystick2.o

$(OBJDIR)/dinput8.a: $(OBJDIR)/dinput8/mouse.o $(OBJDIR)/dinput8/mouse2.o $(OBJDIR)/dinput8/keyboard.o $(OBJDIR)/dinput8/joystick.o $(OBJDIR)/dinput8/joystick2.o
	cd $(OBJDIR) ; $(AR) rcs dinput8.a dinput8/mouse.o dinput8/keyboard.o dinput8/joystick.o dinput8/joystick2.o dinput8/mouse2.o


#### Clean target deletes all generated files ####
clean:
	rm -f \
		$(BUILDDIR)/$(PLUGIN_FILE) \
		$(OBJS)
	rm -f -r \
		$(BUILDDIR) \
		$(OBJDIR)


# Create the target directory (if needed)
$(BUILDDIR):
	mkdir -p $(BUILDDIR)

$(OBJDIR):
	mkdir -p $(OBJDIR)

# Enable dependency checking
.KEEP_STATE:
.KEEP_STATE_FILE:.make.state
