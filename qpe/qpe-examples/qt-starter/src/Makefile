#############################################################################
# Makefile for building qt-starter
# Generated by tmake at 20:01, 2012/08/16
#     Project: qt-starter
#    Template: app
#############################################################################

####### Compiler, tools and options

CC	=	$(TARGET_CROSS)gcc
CXX	=	$(TARGET_CROSS)g++
CFLAGS	=	-pipe $(TARGET_CFLAGS) -Wall -W -O2 -DNO_DEBUG
CXXFLAGS=	-pipe $(TARGET_CFLAGS) -DQWS -fno-exceptions -fno-rtti -Wall -W -O2 -DNO_DEBUG
INCPATH	=	-I$(QTDIR)/include -I$(QPEDIR)/include
LINK	=	$(TARGET_CROSS)g++
LFLAGS	=	$(TARGET_LDFLAGS)
LIBS	=	$(SUBLIBS) -L$(QPEDIR)/lib -L$(QTDIR)/lib -lm $(TARGET_LIBS) -lqpe -lqte
MOC	=	$(QTDIR)/bin/moc
UIC	=	$(QTDIR)/bin/uic

TAR	=	tar -cf
GZIP	=	gzip -9f

####### Files

HEADERS =	
SOURCES =	main.cpp
OBJECTS =	main.o
INTERFACES =	
UICDECLS =	
UICIMPLS =	
SRCMOC	=	
OBJMOC	=	
DIST	=	
TARGET	=	qt-starter
INTERFACE_DECL_PATH = .

####### Implicit rules

.SUFFIXES: .cpp .cxx .cc .C .c

.cpp.o:
	$(CXX) -c $(CXXFLAGS) $(INCPATH) -o $@ $<

.cxx.o:
	$(CXX) -c $(CXXFLAGS) $(INCPATH) -o $@ $<

.cc.o:
	$(CXX) -c $(CXXFLAGS) $(INCPATH) -o $@ $<

.C.o:
	$(CXX) -c $(CXXFLAGS) $(INCPATH) -o $@ $<

.c.o:
	$(CC) -c $(CFLAGS) $(INCPATH) -o $@ $<

####### Build rules


all: $(TARGET)

$(TARGET): $(UICDECLS) $(OBJECTS) $(OBJMOC) 
	$(LINK) $(LFLAGS) -o $(TARGET) $(OBJECTS) $(OBJMOC) $(LIBS)

moc: $(SRCMOC)

tmake: Makefile

Makefile: qt-starter.pro
	tmake qt-starter.pro -o Makefile

dist:
	$(TAR) qt-starter.tar qt-starter.pro $(SOURCES) $(HEADERS) $(INTERFACES) $(DIST)
	$(GZIP) qt-starter.tar

clean:
	-rm -f $(OBJECTS) $(OBJMOC) $(SRCMOC) $(UICIMPLS) $(UICDECLS) $(TARGET)
	-rm -f *~ core

####### Sub-libraries


###### Combined headers


####### Compile

main.o: main.cpp

