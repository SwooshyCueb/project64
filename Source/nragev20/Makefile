## -*- Makefile -*-

ifeq ($(BUILD_ARCH),)
BUILD_ARCH = x86
endif

#### Compiler and tool definitions shared by all build targets #####
CC = gcc
CXX = g++
AR = ar
WINDRES = windres
BASICOPTS = -O3
CXXFLAGS = $(BASICOPTS) -Wno-write-strings
LDFLAGS = -static-libstdc++ -static-libgcc


# Define the target directories.

OBJDIR=obj
BULDDIR=build
DXLIBDIR32=../3rd\ Party/directx/lib
DXLIBDIR64=../3rd\ Party/directx/lib64
ifeq ($(BUILD_ARCH),x86_64)
DXLIBDIR=$(DXLIBDIR64)
else
DXLIBDIR=$(DXLIBDIR32)
endif

all: $(BULDDIR)/PJ64_NRage.dll

release: all

debug: debugvar $(OBJDIR)/Debug.o all

debugvar:
	$(eval BASICOPTS := -g -D_DEBUG)
	$(eval DEBUGOBJS := $(OBJDIR)/Debug.o)

## Target: PJ64_NRage.dll.exe
OBJS =  \
	$(OBJDIR)/mingw.o \
	$(DEBUGOBJS) \
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
#USERLIBS = $(SYSLIBS_SUAinst.exe) 
#DEPLIBS =  
#LDLIBS = $(USERLIBS_SUAinst.exe)

# Link or archive
$(BULDDIR)/PJ64_NRage.dll: $(BULDDIR) $(OBJDIR) $(OBJS) $(DEPLIBS)
	$(CXX) -shared $(CXXFLAGS) $(CPPFLAGS) -o $@ $(OBJS) $(LDLIBS) $(LDFLAGS)


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
	$(WINDRES) NRagePluginV2.rc $@


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
		$(BULDDIR)/PJ64_NRage.dll \
		$(OBJS)
	rm -f -r \
		$(BULDDIR) \
		$(OBJDIR)


# Create the target directory (if needed)
$(BULDDIR):
	mkdir -p $(BULDDIR)

$(OBJDIR):
	mkdir -p $(OBJDIR)

# Enable dependency checking
.KEEP_STATE:
.KEEP_STATE_FILE:.make.state
