# Basic makefile
	
```Makefile
PREFIX ?= /usr

all:
	@mkdir -p build
	@cp -p program.sh build/program

install:
	@mkdir -p $(DESTDIR)$(PREFIX)/bin
	@cp -p build/program $(DESTDIR)$(PREFIX)/bin/program
	@chmod 755 $(DESTDIR)$(PREFIX)/bin/promptless

uninstall:
	@rm -rf $(DESTDIR)$(PREFIX)/bin/program
```

