PA_HEADERS?=../3rdparty/pulseaudio-headers
PREFIX?=/usr/local

LIBS=glib-2.0
CFLAGS+=$(shell pkg-config --cflags $(LIBS))
LDFLAGS+=$(shell pkg-config --libs $(LIBS))

.PHONY: libs pcs install clean all
all: libs

libs: libpulse.so libpulse-simple.so libpulse-mainloop-glib.so

lib%.so: %.c
	@$(CC) -shared -fPIC -I$(PA_HEADERS) $(CFLAGS) $(LDFLAGS) -o $@.0 $<
	@ln -s $@.0 $@

pcs: libpulse.pc libpulse-simple.pc libpulse-mainloop-glib.pc

lib%.pc: %.pc.in
	@sed "s%@PREFIX@%$(PREFIX)%g" $< > $@

install: libs pcs
	@mkdir -p $(PREFIX)/include
	@cp -a $(PA_HEADERS)/pulse $(PREFIX)/include
	@mkdir -p $(PREFIX)/lib
	@cp -a *.so *.so.0 $(PREFIX)/lib
	@mkdir -p $(PREFIX)/lib/pkgconfig
	@cp -a *.pc $(PREFIX)/lib/pkgconfig

clean:
	rm -f *.so *.so.0