# Makefile cheat sheet

Mostly geared towards GNU make

I've used `->| ` to indicate a tab character, as it's clearer to read than `⇥`

* Set a target, its dependencies and the commands to execute in order
```
target: [dependencies]
->| <shell command>
->| <shell command>
    ...
```

* shell commands are not required. To just list dependencies for a target, use only the first line:
```
output.o: defs.h
```

* The first target is the default target

  This could be `all` for example, listed together with its dependencies:
  `all: $(TARGET)`
  
* Use `.PHONY <target>` at the start or end of a build step if <target> is not an actual file to be build:
```
.PHONY: clean
clean:
->| rm -rf *.o $(TARGET)
```
or
```
install:
->| @echo Installing
.PHONY: install
```
but before is preferred.

* Comments are preceeded with `#`, either at the start of the line, or as end-of-line comments

* Lines can be continued across line-breaks by ending a line with a backslash `\`. 
  Just be aware of accidental spaces after the backslash (simply always removing trailing whitespace).

* Set a variable with `VARNAME = value`
  Variable values can contains spaces: no need to escape them or quote them. Practical for a list of filenames or compiler flags

  Standard variable names:
```
# C/C++ compiler
CC = gcc  
CXX = g++
# Linker
LD = ld
# Libraries to link with (include '-l')
LIBS = -lm -lpthread
# Compiler and linker flags (warning levels, optimisation level, 
# include debugging symbols, add include search path, add library search path)
CFLAGS = -Wall -Wextra -O2 -g -I./src/include
CXXFLAGS = -Wall -Wextra -O2 -g -I./src/include
LDFLAGS = -L./src/libs
# Object files
OBJS = obj1.o obj2.o obj3.o
# Executable name
TARGET = fancyname
```

* Use a variable with `$(VARNAME)`: dollar sign and enclosed in parentheses.

  Single-letter variables can omit the parentheses. This is mainly done for the automatic variables, see below.
  
* prepend shell commands with '@' to suppress printing the command itself. Shell output is still visible.
  (The `@` character has to be at the beginning of the command, directly after the tab.)
  
  Useful for specific `echo` commands, e.g.
```
$(TARGET): $(OBJS)
->| @echo "Building target"
->| $(LD) $(LDFLAGS) $(LIBS) ($OBJS) -o $(TARGET)
```

* Parallel builds:
  `make -j` to use all available cores
  `make -j2` to use two cores

* Automatic variables

  * `$@`: name of the target

  * `$<`: first dependency
```
main.o: main.cpp defs.h
->| $(CXX) $(CXXFLAGS) $< -o $@
```

  * `$^`: dependencies (prerequisites), separatead by spaces (duplicates are removed)
```
$(TARGET): $(OBJS)
->| $(CC) $(LDFLAGS) $^ -o $@
```

  * `$+`: as `$^`, but with duplicates included
  
  * `$?`: all prerequisites that are newer than the target (with respect to their timestamp)
  
  Each of the above automatic variables has two variants: appended with a `D`, or appended with an `F`. 
  The former results in the directory name of the relevant variable, the latter in just the filename portion.
  Surround these with parentheses, since they are longer than 1 character, e.g.: `$(<F)`.
  
* functions

  GNU make has a bunch of functions that can be applied to variables. A few are listed here, more in the [GNU make manual](https://www.gnu.org/software/make/manual/html_node/File-Name-Functions.html#File-Name-Functions).
  
  * `$(dir file1.txt dir/file2.txt)` results in `./ dir/`.
  
  * `$(notdir file1.txt dir/file2.txt)` results in `file1.txt file2.txt`.
  
  The above two functions can be used instead of the `D` and `F` additions in case of automatic variables.
  
  * `$(suffix file1.txt)` yields `.txt`. Includes the last dot, and omits results (empty result) from files without an extension.
  
  * `$(wildcard *.txt)` results in a space-separated list with all `.txt` files.
  
* `$%` (GNU extension). 
  This is also an automatic variable, but can be used somewhat differently. It is used in patterns:
```
%.o: %.c
->| $(CC) -c $(CFLAGS) $< -o $@
```
  This will build an object file for each source file, with a matching filename as output.
  (Note that with most compilers, `-o $@` is not really necessary in this case, 
  since the compiler will automatically replace the extension with `.o`.)
  
  `%` is written between a prefix and postfix, either of which can be empty.

  The actual "stem" for each match is stored in the `$*` variable.
  
  If there is no (Unix-style) directory separator '/' in the target name, then only the filename part is matched on; directory names are removed before trying to match.
  This means that the stem does not contain the directory name, though the final target name and prerequisites do: it's only for the matching.
  
# Resources

## Blog posts

* [Afraid of Makefiles? Don't be!](https://matthias-endler.de/2017/makefiles/)


## Manuals

* [GNU make manual](https://www.gnu.org/software/make/manual/html_node/index.html)

* Clark Grubb's [style guide](http://clarkgrubb.com/makefile-style-guide)

## Examples

* http://osr507doc.sco.com/en/tools/make_example.html

## StackOverflow questions (and their answers)

* https://stackoverflow.com/questions/3220277/what-do-the-makefile-symbols-and-mean
* https://unix.stackexchange.com/questions/346322/what-does-symbol-in-makefile-mean