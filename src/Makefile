DESTDIR ?=
PREFIX ?= /usr/local
LIBDIR ?= $(PREFIX)/lib
BINDIR ?= $(PREFIX)/bin
ifdef DEB_HOST_MULTIARCH
LIBDIR := $(LIBDIR)/$(DEB_HOST_MULTIARCH)
endif

CROSS_COMPILE =
CC = $(CROSS_COMPILE)gcc
AR = $(CROSS_COMPILE)ar
LD = $(CROSS_COMPILE)gcc

WARNINGS = -Wall -Wextra -Werror -Wfatal-errors
INCLUDE_DIRS = -I ../include
override CFLAGS := $(WARNINGS) $(INCLUDE_DIRS) -fPIC $(CFLAGS)

VPATH = ../include/tinyalsa
OBJECTS = mixer.o pcm.o

.PHONY: all
all: libtinyalsa.a libtinyalsa.so

pcm.o: pcm.c pcm.h

mixer.o: mixer.c mixer.h

libtinyalsa.a: $(OBJECTS)
	$(AR) $(ARFLAGS) $@ $^

libtinyalsa.so: libtinyalsa.so.1
	ln -sf $< $@

libtinyalsa.so.1: libtinyalsa.so.1.1.0
	ln -sf $< $@

libtinyalsa.so.1.1.0: $(OBJECTS)
	$(LD) $(LDFLAGS) -shared -Wl,-soname,libtinyalsa.so.1 $^ -o $@

.PHONY: clean
clean:
	rm -f libtinyalsa.a
	rm -f libtinyalsa.so
	rm -f libtinyalsa.so.1
	rm -f libtinyalsa.so.1.1.0
	rm -f $(OBJECTS)

.PHONY: install
install: libtinyalsa.a libtinyalsa.so.1
	install -d $(DESTDIR)$(LIBDIR)/
	install libtinyalsa.a $(DESTDIR)$(LIBDIR)/
	install libtinyalsa.so.1.1.0 $(DESTDIR)$(LIBDIR)/
	ln -sf libtinyalsa.so.1.1.0 $(DESTDIR)$(LIBDIR)/libtinyalsa.so.1
	ln -sf libtinyalsa.so.1 $(DESTDIR)$(LIBDIR)/libtinyalsa.so

