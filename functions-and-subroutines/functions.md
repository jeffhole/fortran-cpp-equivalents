###

### Functions, pass-by-value

```fortran
integer function process(a, b)
    integer, value :: a, b
    process = a + b
end function
```

```cxx
int process(int a, int b){
    return a + b;
}
```

### Functions, pass-by-reference

```fortran
integer function process(a, b)
    integer, intent(inout) :: a, b
    process = a + b
end function
```

```cxx
int process(int &a, int &b){
    return a + b;
}
```

Notice that in both cases, the arguments can be modified. In Fortran, this is indicated by the `intent(inout)`. In C++, this is indicated by the lack of `const`.

### Functions, pass-by-reference, arguments are not intended to be changed

```fortran
integer function process(a, b)
    integer, intent(in) :: a, b
    process = a + b
end function
```

```cxx
int process(const int &a, const int &b){
    return a + b;
}
```