# Polymorphism

Did we have polymorphic behaviour before OO languages? Of course we did.
Consider this simple C `copy` program.

```c
#include <stdio.h>

void copy() {
    int c;
    while((c = getchar()) != EOF)
        putchar(c);
}
```

The function `getchar()` reads from `STDIN`. But which device is `STDIN`? The `putchar()` function writes to `STDOUT`. But which device is that? These functions are polymorphic--their behavior depends on the type of the `STDIN` and `STDOUT`.

It's as though `STDIN` and `STDOUT` are Java-style interfaces that have implementation for each device. Of course, there are not interfaces in the example C program--so how does the call getchar() actually get delivered to the device drive that reads the character?

The answer to that question is pretty straightforward. The UNIX operating system requires that every IO device driver provide five standard function. `open`, `close`, `read`, `write`, and `seek`. The signatures of those functions must be idential for every IO driver.

The FILE data structure contains five pointers to functions. In our example, it might look like this:

```c
struct FILE {
    void (*open)(char* name, int mode);
    void (*close)();
    int (*read)();
    void (*write)(char);
    void (*seek)(long index, int mode);
}
```

The IO driver for the console will define those functions and load up a FILE data structure with their addresses--something like this:

```c
#include "file.h"

void open(char* name, int mode) { /* ... */ }
void close() { /* ... */ }
int read() {int c; /* ... */ return c;}
void write(char c) { /* ... */ }
void seek(long index, int mode) { /* ... */ }

struct FILE console = {open, close, read, write, seek};
```

Now if `STDIN` is defined as a `FILE`, and if it points to the console data structure, then `getchar()` might be implemented this way:

```c
extern struct FILE* STDIN;

int getchar() {
    return STDIN->read();
}
```

In other words, `getchar()` simply calls the funciton pointed to by the `read` pointer of the `FILE` data structure pointed to by `STDIN`.

This simple trick is the basis of all polymorphism in OO. In C++, for example, every virtual function within a class has a pointer in a table called `vtable`, and the calls to virtual function go throught that table.
Constructors of derivatives simply load their versions of those functionas in the `vtable` of the object being created.

The bottom line is that polymorphism is an application of pointers to functions. Programmers have been using pointers to funtions to acheive polymorphic behavior since Von Neumann architecture were first impleemnted in the late 1940s. In other words, OO has provided nothing new.

Ah, but that's not quite correct. OO languages may not have given us polymorphism, but have made it much safer and much mroe convenient.

The problem with explicitly using pointers to functions to create polymorphic behavior is that pointers to functions are _dangerous_.Such use is driven by set of manual conventions. You have to remember to follow the convention to initialize those pointers. You have to remember to follow the convention to call all your functions through those pointers. If any programmer fails to remember these conventions, the resulting bug can be devilishly hard to track down and eliminate.

OO languages eliminate these conventions and, therefore, these dangers.
Using an OO language makes polymorphism trivial. That fact provides an enormous power that old C progammers could only dream of. On this basis, we can conclude that OO imposes discipline on indirect transfer of control.
