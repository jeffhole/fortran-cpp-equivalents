A ```type``` in Fortran is analogous to a ```struct``` in C/C++.

```fortran
type MyDataStructure
    integer :: a, b, c
    real(4) :: x, y, z
end type
```

```cxx
struct MyDataStructure {
    int a, b, c;
    float x, y, z;
};
```

C++ offers `class` keyword, which is virtually identical to `struct`, with the key difference being that `struct` visibility is public by default and `class` is private by default. You can specify which parts are public/private by using the keyword `private:` and `public:`:

```cxx
class MyClass{
public:
    MyClass();
    int compute(int);
private:
    int a, b, c;
    float x, y, z;
    double alpha, beta, gamma, delta;
}
```

Prefer `class` over `struct` for classes intended to have private members. Use `struct` sparingly for smaller concrete data types (e.g. a Position struct that holds `x`, `y`, and `z`).

### Using a type/structure

```fortran
program main
end program
```

```cxx
```
