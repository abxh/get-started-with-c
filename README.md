# get-started-with-c

There are advice out there for which resouces to choose, what to learn, and etc. You can research that online,
but I will put up some personal recommendations. However in particular for C, knowing your tools can make your
learning experience so much more productive and less mentally taxing.

I will assume you have setup [WSL for windows](https://learn.microsoft.com/en-us/windows/wsl/install),
[homebrew for macos](https://brew.sh/) or have a package manager in a linux system for installing the named software.

## Tools
### Compiler Warnings

Yes. This is not a tool. But it's a thing I set up later, but which I could have benifitted from doing it from the start.

With `gcc`, compile your program with at least following options:
```bash
gcc -Wall -Wextra -pedantic
```

With `clang`, compile your program with at least following options:
```bash
clang -Wall -Wextra -pedantic
```

Clang even has a option to turn every warning on. Some of these warnings are not harmful; some are meant to be used in c++.
But could be educational to use, so try it out : ).
```bash
clang -Weverything
```

You can turn off particular warnings like so (with a `Wno-` prefix):
```
clang -Weverything -Wno-unsafe-buffer-usage
```

With other compilers, I encourage you to look up common warnings and turn them on.

### Memory sanitizers

These catch unsafe memory usage. They are a part of your program runtime, so turn them off for release build. But I have
found them to often catch memory errors, which the warnings doesn't tell me much about -- and `segfaults` -- yeah they are
not particularly helpful.

You can turn them on like so:
```
gcc -fsanitize=undefined -fsanitize=address
clang -fsanitize=undefined -fsanitize=address
```
You may need to install extra packages for them to work, I found. In particular look for packages with keywords `ubsan` and `libsan`.

For other compilers, you are on your own. Sanitizers aren't neccessary, but useful.

### Stack tracing and debugging

In particular get to know about tools like `gdb`, and tools like `strace` and `ltrace`.

`gdb` could be a little intimidating to use, so here is a helpful guide by comment @AlphaOmegaProgrammer wrote moons ago:
```https://godbolt.org/
Try compiling with -g to enable debug symbols, then run it under gdb with gdb ./program followed by run.
Then when it crashes, type bt to get a backtrace, and you can use frame 0 or whatever number to change what
frame of the backtrace you're looking at. gdb is super annoying at first, but once you learn it, it will help
a ton with learning C
```

```bash
run              # run the program
bt               # Shows a backtrace
frame            # Change the current frame (level in the backtrace)
info threads     # Shows all of the threads your program has
thread           # Change to thread #
print <symbol>   # Prints the value of the symbol. You can use C type casting here
x                # Examine memory at the given memory address. Not used much,
                 # but can be helpful when print isn't working and you don't know why
```

Get started by running `gdb <your-program>`. Make sure to compile your c program with following flags:
```bash
gcc -g -ggdb3
clang -g -ggdb3
```

`strace` and `ltrace` are pretty simple to use. You just type `strace <your-program>`, or `ltrace <your-program>`.

If you want, you can setup `gdb` with neovim. Easier to use, imo. Or, use `VS Studio` debugger. Just mentioning
a few more visual alternatives.

### valgrind

This is to test memory leaks. Yes, eventually you will come to a point where you may have forgotten to deallocate
memory --- in your own code, while using library code, etc.

To get started, simply type `valgrind <your-program>`. No more. : ). You will know the details, once you run it.

### static analyzers

These should be a part of your lsp in your editor of choice. If not, please do set up your lsp... At least get
syntax highlighting working.

### assembly

TLDR. Use following command to dump your executable file's converted assembly.

```
objdump -drwC -S -M intel
```

Get to know `objdump`. Get to know assembly. Use [https://godbolt.org/](https://godbolt.org/) to check the assembly
line-by-line.

### formatter

Yes. Formatting matters. So far, I only used `clangd-format` set up with neovim. But do look into this. And preferably
set up automatic formatting.

### build system

This is not neccessary, when you are starting. But if you doing a larger project, do look into build systems. They will
help you when you have multiple C files. The simple build system is `make` -- which does have some magic syntax...
like C's prepressors, ; ). You will have to grok that or make use of other build systems.

## Misc

### Left-aligned / Right-aligned pointers

When you are starting to learn C, this aspect may confuse you. If you view pointers as `pointer types`, do use left-aligned
pointers. The compiler doesn't care.
```c
int a = 1;
int* p = &a;
*p = 2;
```
With one caveat. If you try to declare multiple pointers in the same line, do note:
```c
int* a, b; // a is a int pointer. b is not.
```

And if you view pointers as `pointer to <type>`, then you can use right-aligned pointers. Pointers store an address and don't really work
like usual "types" in C, even if they are helpful to be think that way. If you do pointer arithmatic, the compiler does work with different
offsets depending on what the points point to, but all pointers stores addresses, regardless of which "type" they point to.
```c
int a = 1;
int *p = &a;
*p = 2;
```
And it's clear what is meant here:
```c
int *a, *b; // a is a pointer to int, b is pointer to int.
```

I myself use left-aligned pointers. I may be biased in my explanation. Do as you wish, and use the most helpful convention.
And stick to it. You may use formatters to make youself consistent.

And yes, I found myself learning pointers much easier, when I grokked the C syntax in this manner. Hence, I put it here.

## Stuff to do

- Read K & R - the c programming language: The OG book. One which everyone recommmends.
- Read Effective C: A more modern book. Few exercises. Nice to read. Latest best practices.
- [Turing complete](https://store.steampowered.com/app/1444480/Turing_Complete/): A game to have fun and get a good idea of
how a basic CPU works. Worth it.
- Data structures in C. Stacks, queues, dynamic arrays, hashmaps. Linked version and array versions.
- Use the aformentioned data structures in DSA problems / projects in C.
