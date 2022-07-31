# Encapsulation

Because OO languages make it simple and efficient to encapsulate data and functions, encapsulation is included as part of the definition of OO.

This concept is not specific to OO. In C, we did, in fact, have complete encapsulation. Take a look at this basic C program:

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
    point_t *p = malloc(sizeof(point_t));
    p->x = x;
    p->y = y;
    return p;
}

double distance(point_t *p1, point_t *p2) {
    double dx = p1->x - p2->x;
    double dy = p1->y - p2->y;
    return sqrt(dx * dx + dy * dy);
}
```

We can compile `point.c` and get an object file. The `point.h` header client will know nothing about neither the interiors of the `point_t` nor the implementation of the functions.

```bash
gcc -c point.c
```

```c
// main.c
#include "point.h"
#include <stdio.h>

int main() {
    point_t *p1 = new_point(0.0, 0.0);
    point_t *p2 = new_point(1.0, 1.0);
    printf("distance=%f\n", distance(p1, p2));
}
```

```bash
gcc main.c -o main point.o
./main
# distance=1.414214
```

Encapsulation was weakened when OO in the form of C++ was introduced.

The member variables of a class must be stated in the header file of that class in order for the C++ compiler to know the size of the instances of each class. So our `Point` program changed to look like this:

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

Clients of the header file `point.h` are aware of the members of `Point`. Although the C++ compiler forbids direct access to them, the header client is nevertheless aware of their existence. And unlike C, where we just said that a struct called `point_t` existed, `point.cc` needs to be recompiled if those member names change.

By implementing a _hacky_ method and employing the access modifiers `public`, `private`, and `protected`, encapsulation was partially repaired.

Other OO languages like Java and C# completely abolished the header/implementation distinction, which decreased encapsulation. In these languages, a class' declaration and definition cannot be separated.

For these reasons, it isn't easy to accept that OO depends on solid encapsulation. Indeed, many OO languages have little or no enforced encapsulation.

OO certainly does depend on the idea that programmers are well-behaved enough not to circumvent encapsulated data.
