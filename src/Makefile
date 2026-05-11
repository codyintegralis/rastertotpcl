
EXEC=rastertotpcl
CFLAGS=-Wall -Wextra -O2 -D_DEFAULT_SOURCE
LIBS=-lcupsimage
EXECPATH=/usr/lib/cups/filter

all: rastertotpcl

.PHONY: clean install uninstall

$(EXEC): rastertotpcl.c
	gcc $(CFLAGS) -o $(EXEC) rastertotpcl.c $(LIBS)

install: $(EXEC)
	cp $(EXEC) $(EXECPATH)/

uninstall:
	rm -f $(EXECPATH)/$(EXEC)

clean:
	rm -f rastertotpcl

