#	$Id: Makefile.in,v 1.49 2016/03/08 18:42:13 ragge Exp $
#
# Makefile.in for ccom
#

srcdir=.
top_srcdir=../..
builddir=.
top_builddir=../..
EXEEXT = 
BINPREFIX = 
CC_FOR_BUILD = gcc
CFLAGS += -ffunction-sections -fdata-sections
CPPFLAGS =   -D_BSD_SOURCE -DGCC_COMPAT -DNATIVE_FLOATING_POINT -D_ISOC99_SOURCE \
	-Dos_$(TARGOS) -Dmach_$(TARGMACH) \
	-I$(srcdir) -I$(builddir) -I$(top_builddir) -I$(MIPDIR) -I$(MDIR) \
	-I$(top_srcdir)/os/$(TARGOS) -I$(COMMONDIR)
LIBS = -lc -lgcc
LDFLAGS += -Wl,--gc-sections -s
LEX = flex
LFLAGS =
LEX_OUTPUT_ROOT = lex.yy
YACC = byacc
YFLAGS = 
TARGOS = litebsd
TARGOSVER = 0
TARGMACH = mips
prefix = /usr
exec_prefix = ${prefix}
libexecdir = ${exec_prefix}/libexec
datarootdir = ${prefix}/share
mandir = ${datarootdir}/man
INSTALL = /usr/bin/install -c
INSTALL_PROGRAM = ${INSTALL}
INSTALL_DATA = ${INSTALL} -m 644
CCOM=$(BINPREFIX)ccom$(EXEEXT)
CC0=$(BINPREFIX)cc0$(EXEEXT)
CC1=$(BINPREFIX)cc1$(EXEEXT)
CF0=
CF1=

MDIR=$(top_srcdir)/arch/$(TARGMACH)
MIPDIR=$(top_srcdir)/mip
COMMONDIR=$(top_srcdir)/common

DEST=$(BINPREFIX)ccom$(EXEEXT)
MANPAGE=$(BINPREFIX)ccom
MKEXT=mkext$(EXEEXT)

all: $(DEST)

OBJS=	builtins.o cgram.o code.o common.o compat.o dwarf.o external.o	\
	gcc_compat.o init.o inline.o local.o local2.o main.o		\
	match.o optim.o optim2.o order.o pftn.o reader.o		\
	regs.o scan.o stabs.o symtabs.o table.o trees.o unicode.o

OBJS0=  builtins.o cgram.o code.o common.o compat.o dwarf.o external.o	\
	gcc_compat.o init.o inline.o local.o main.o             	\
	optim.o pftn.o 							\
	scan.o stabs.o symtabs.o trees.o unicode.o

OBJS1=  common2.o compat.o external.o           			\
	local2.o main2.o                				\
	match.o optim2.o order.o reader.o      				\
	regs.o table.o


LOBJS=	mkext.lo common.lo table.lo

HDRS=	$(srcdir)/pass1.h $(MIPDIR)/pass2.h $(MIPDIR)/manifest.h	\
	$(MDIR)/macdefs.h $(MIPDIR)/node.h $(COMMONDIR)/compat.h	\
	$(COMMONDIR)/unicode.h

#
# round 1: generate external.[ch], cgram.[ch] & scan.c
#

$(LOBJS): $(HDRS)

mkext.lo: $(MIPDIR)/mkext.c
	$(CC_FOR_BUILD) $(CPPFLAGS) -DMKEXT -c -o $@ $(MIPDIR)/mkext.c

common.lo: $(MIPDIR)/common.c
	$(CC_FOR_BUILD) $(CPPFLAGS) -DMKEXT -c -o $@ $(MIPDIR)/common.c

table.lo: $(MDIR)/table.c
	$(CC_FOR_BUILD) $(CPPFLAGS) -DMKEXT -c -o $@ $(MDIR)/table.c

$(MKEXT): $(LOBJS)
	$(CC_FOR_BUILD) $(LOBJS) -o $@

external.c: $(MKEXT)
	$(builddir)/$(MKEXT)

cgram.c: $(srcdir)/cgram.y
	$(YACC) $(YFLAGS) -d $(srcdir)/cgram.y
	mv -f y.tab.c cgram.c
	mv -f y.tab.h cgram.h

scan.c: $(srcdir)/scan.l
	$(LEX) $(LFLAGS) $(srcdir)/scan.l
	mv -f $(LEX_OUTPUT_ROOT).c scan.c

#
# round 2: compile $(OBJS)
#

$(OBJS): $(HDRS) external.c cgram.c

builtins.o: $(srcdir)/builtins.c
	$(CC) $(CF0) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(srcdir)/builtins.c

cgram.o: cgram.c
	$(CC) $(CF0) $(CFLAGS) $(CPPFLAGS) -c -o $@ cgram.c

code.o: $(MDIR)/code.c
	$(CC) $(CF0) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(MDIR)/code.c

common.o: $(MIPDIR)/common.c
	$(CC) $(CF0) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(MIPDIR)/common.c

common2.o: $(MIPDIR)/common.c
	$(CC) $(CF1) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(MIPDIR)/common.c

compat.o: $(COMMONDIR)/compat.c
	$(CC) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(COMMONDIR)/compat.c

dwarf.o: $(srcdir)/dwarf.c
	$(CC) $(CF0) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(srcdir)/dwarf.c

external.o: external.c
	$(CC) $(CFLAGS) $(CPPFLAGS) -c -o $@ external.c

gcc_compat.o: $(srcdir)/gcc_compat.c
	$(CC) $(CF0) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(srcdir)/gcc_compat.c

init.o: $(srcdir)/init.c
	$(CC) $(CF0) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(srcdir)/init.c

inline.o: $(srcdir)/inline.c
	$(CC) $(CF0) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(srcdir)/inline.c

local.o: $(MDIR)/local.c
	$(CC) $(CF0) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(MDIR)/local.c

local2.o: $(MDIR)/local2.c
	$(CC) $(CF1) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(MDIR)/local2.c

main.o: $(srcdir)/main.c
	$(CC) $(CF0) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(srcdir)/main.c

main2.o: $(srcdir)/main.c
	$(CC) $(CF1) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(srcdir)/main.c

match.o: $(MIPDIR)/match.c
	$(CC) $(CF1) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(MIPDIR)/match.c

optim.o: $(srcdir)/optim.c
	$(CC) $(CF0) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(srcdir)/optim.c

optim2.o: $(MIPDIR)/optim2.c
	$(CC) $(CF1) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(MIPDIR)/optim2.c

order.o: $(MDIR)/order.c
	$(CC) $(CF1) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(MDIR)/order.c

pftn.o: $(srcdir)/pftn.c
	$(CC) $(CF0) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(srcdir)/pftn.c

reader.o: $(MIPDIR)/reader.c
	$(CC) $(CF1) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(MIPDIR)/reader.c

regs.o: $(MIPDIR)/regs.c
	$(CC) $(CF1) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(MIPDIR)/regs.c

scan.o: scan.c
	$(CC) $(CF0) $(CFLAGS) $(CPPFLAGS) -c -o $@ scan.c

stabs.o: $(srcdir)/stabs.c
	$(CC) $(CF0) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(srcdir)/stabs.c

symtabs.o: $(srcdir)/symtabs.c
	$(CC) $(CF0) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(srcdir)/symtabs.c

table.o: $(MDIR)/table.c
	$(CC) $(CF1) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(MDIR)/table.c

trees.o: $(srcdir)/trees.c
	$(CC) $(CF0) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(srcdir)/trees.c

unicode.o: $(COMMONDIR)/unicode.c
	$(CC) $(CF0) $(CFLAGS) $(CPPFLAGS) -c -o $@ $(COMMONDIR)/unicode.c

#
# round 3: build $(DEST)
#

$(CCOM): $(OBJS)
	$(CC) $(LDFLAGS) $(OBJS) -o $@ $(LIBS)

$(CC0):	$(OBJS0)
	$(CC) $(LDFLAGS) $(OBJS0) -o $@ $(LIBS)

$(CC1):	$(OBJS1)
	$(CC) $(LDFLAGS) $(OBJS1) -o $@ $(LIBS)

install: $(DEST)
	test -z "$(DESTDIR)$(libexecdir)" || mkdir -p "$(DESTDIR)$(libexecdir)"
	$(INSTALL_PROGRAM) $(DEST) $(DESTDIR)$(libexecdir)
	test -z "$(DESTDIR)$(mandir)/cat1" || mkdir -p "$(DESTDIR)$(mandir)/cat1"
	$(INSTALL_DATA) $(srcdir)/ccom.1 $(DESTDIR)$(mandir)/cat1/$(MANPAGE).1

clean:
	rm -f $(DEST) $(OBJS0) $(OBJS1) $(MKEXT) $(LOBJS) \
	$(LEX_OUTPUT_ROOT).c scan.c y.tab.[ch] cgram.[ch] external.[ch]

distclean: clean
	rm -f Makefile
