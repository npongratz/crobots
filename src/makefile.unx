# makefile for crobots

OBJS =  main.o grammar.o lexanal.o compiler.o cpu.o intrins.o display.o screen.o motion.o

SOURCES = crobots.h compiler.h tokens.h main.c grammar.y grammar.c lexanal.l lexanal.c compiler.c cpu.c intrins.c display.c screen.c screend.c motion.c 

DOS  = main.c grammar.c lexanal.c compiler.c cpu.c intrins.c display.c screend.c motion.c 
DOSA = main.c grammar.c lexanal.c compiler.c cpu.c intrins.c display.c screena.c motion.c 

# Xenix $(CC) flags: optimize(O), omit stack checking(K), separate text and data(i)
CC    = /usr/bin/cc
EGREP = /usr/bin/egrep
LEX   = /usr/bin/lex
# TODO: Find lint for darwin/OS X
LINT  = true
LPR   = /usr/bin/lpr
MV    = /bin/mv
PR    = /usr/bin/pr
RM    = /bin/rm
YACC  = /usr/bin/yacc

CFLAGS = -O -K -i -DUNIX
LIBS = -lm -lcurses -ltermlib

# note - grammar.c and lexanal.c: both yacc (grammar.y) and lex (lexanal.l)
#  place '# line' and/or '# ifdef' that sometimes choke the PC-DOS compiler,
#  Lattice C, ver 2.14.  The egrep filters are designed to remove 
#  the troublesome spots.
#  Lattice does not accept '# line nn' or '#' (null) statements, and gets
#  confused when '#ifdef symbol .... #endif' are nested.

crobots: 	$(OBJS)
		$(CC) -s $(CFLAGS) $(OBJS) -o crobots $(LIBS)

main.o: 	crobots.h main.c
		$(CC) $(CFLAGS) -c main.c

grammar.o: 	crobots.h compiler.h grammar.y
		$(YACC) -d grammar.y
		$(EGREP) -v '# line|^#$$' y.tab.c >grammar.c
		$(RM) y.tab.c
		$(MV) y.tab.h tokens.h
		$(CC) $(CFLAGS) -c grammar.c

lexanal.o: 	crobots.h compiler.h lexanal.l grammar.y
		$(LEX) lexanal.l
		$(EGREP) -v '# ifdef|# endif' lex.yy.c >lexanal.c
		$(RM) lex.yy.c
		$(CC) $(CFLAGS) -c lexanal.c

compiler.o: 	crobots.h compiler.h tokens.h compiler.c
		$(CC) $(CFLAGS) -c compiler.c
	
cpu.o: 		crobots.h grammar.y cpu.c
		$(CC) $(CFLAGS) -c cpu.c
	
intrins.o:	crobots.h intrins.c
		$(CC) $(CFLAGS) -c intrins.c
	
display.o:	crobots.h display.c
		$(CC) $(CFLAGS) -c display.c
	
screen.o:	crobots.h screen.c
		$(CC) $(CFLAGS) -c screen.c
	
motion.o:	crobots.h motion.c
		$(CC) $(CFLAGS) -c motion.c
	
dos:
		$(CC) -s -dos -K -i -O -DDOS $(DOS) -o crobotsx.exe -lm
		$(RM) *.o

dosa:
		$(CC) -s -dos -K -i -O -DDOS $(DOSA) -o crobotsa.exe -lm
		$(RM) *.o
print:
		$(PR) -t $(SOURCES) | $(LPR)

lint:
		@echo 'TODO: this does nothing at the moment. Must find lint for darwin/OS X'
		$(LINT) -abc *.c | tee crobots.lint
		@echo 'lint output kept in crobots.lint'

