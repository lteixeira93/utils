CC=gcc
CFLAGS=-O0 -g -Wall -Wextra

SRC=src
OBJ=obj
BINDIR=bin

# Any .c in src directory will be in SRCS variable
SRCS=$(wildcard $(SRC)/*.c)

# Pattern substitution, creates matching list of .o files based on each .c file
OBJS=$(patsubst $(SRC)/%.c, $(OBJ)/%.o, $(SRCS))

BIN=$(BINDIR)/main
SUBMITNAME=project.zip

# Default rule when use make (Same as make all)
all:$(BIN)

release: CFLAGS=-O2 -g -Wall -Wextra -DNDEBUG
release: clean
release: $(BIN)

# BIN Depends on OBJS
$(BIN): $(OBJS)
	$(CC) $(CFLAGS) $(OBJS) -o $@

# OBJS Depends on SRC so create .o file based on .c file where % (Any text .extension) is a wildcard
# $@ -> Left hand side  (%.o)
# $< -> Right hand side (%.c)
$(OBJ)/%.o: $(SRC)/%.c 
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	$(RM) -rf $(BINDIR)/* $(OBJ)/* $(SUBMITNAME)

rmdirs:
	$(RM) -rf $(BINDIR)/ $(OBJ)/

dirs:
	mkdir $(BINDIR) $(OBJ)

submit:
	$(RM) $(SUBMITNAME)
	zip $(SUBMITNAME) $(BIN)