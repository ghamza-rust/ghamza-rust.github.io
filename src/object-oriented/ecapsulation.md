# Ecapsulation

Encapsulation is cited as part of the definition of OO because OO Languages provide easy and effective encapsulation of data and functions.

This idea is certainly not unique to OO. Indeed, we had perfect encapsulation in C. Consider this simple C program:

```c
// point.h
typedef struct point_t point_t;
point_t* new_point(double x, double y);
double distance(point_t *p1, point_t *p2);
```

```c
// point.c
#include "point.h"
#include <stdlib.h>
#include <math.h>

struct point_t {
    double x;
    double y;
};

point_t* new_point(double x, double y) {
    point_t* p = malloc(sizeof(point_t));
    p->x = x;
    p->y = y;
    return p;
}

double distance(point_t* p1, point_t* p2) {
    double dx = p1->x - p2->x;
    double dy = p1->y - p2->y;
    return sqrt(dx * dx + dy * dy);
}
```

We can compile `point.c`, get an object file. The `point.h` header client will know nothing about the inner structure of the point or the implementation of the functions.

```bash
gcc -c point.c
```

```c
// main.c
#include "point.h"
#include <stdio.h>

int main(int argc, char** argv) {
    point_t* origin = new_point(0.0, 0.0);
    point_t* upperRight = new_point(1.0, 1.0);
    printf("distance=%f\n", distance(origin, upperRight));
}
```

```bash
gcc main.c -o main point.o
./main
# distance=1.414214
```

But then came OO in the form of C++, and the perfect encapsulation of C was broken.

The C++ compiler needs to know the size of the instances of each class, so it needs the member variables of a class to be declared in the header file of that class. So our `Point` program changed to look like this:

```cpp
// point.h
class Point {
public:
    Point(double x, double y);
    double distance(const Point& p) const;

private:
    double x;
    double y;
};
```

```cpp
#include "point.h"
#include <math.h>

Point::Point(double x, double y) : x(x), y(y) { }

double Point::distance(const Point& p) const {
    double dx = x - p.x;
    double dy = x - p.y;
    return sqrt(dx * dx + dy * dy);
}
```

Clients of the header file `point.h` know about the member variables x and y!
The compiler will prevent access to them, but the client still knows they exist.
For example, if those member names are changed, the `point.cc` file must be recompiled! Encapsulation has been broken.

Indeed, encapsulation is partially repaired by introducing the `public`, `private`, and `protected` keywords into the language. This, however, was a _hack_ necessitated by the technical need for the compiler to see those variables in the header file.

Java and C# completely abolished the header/implementation split, thereby weakening encapsulation even more. In these languages, it is impossible to separate the declaration and definition of a class.

For these reasons, it isn't easy to accept that OO depends on solid encapsulation. Indeed, many OO languages have little or no enforced encapsulation.

OO certainly does depend on the idea that programmers are well-behaved enough not to circumvent encapsulated data. Even so, the languages that claim to provide OO have only weakened the once perfect encapsulation we enjoyed with C.
