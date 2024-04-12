# Classes in Fortran and C++

## Fortran intent(in) type-bound procedures
In Fortran, to define a type-bound procedure that does not modify the class (like a getter method):

```Fortran
type MyClass
contains
    procedure, public :: getId
end type

interface
    module function getId(this) result(id)
        class(MyClass), intent(in) :: this
        integer :: id
    end function
end interface
```

This is done in C++:
```cxx
class MyClass{
public:
    int getId() const;
}
```

The `const` indicator means that this class method does not modify the class instance.

## Abstract Types (Classes)

Abstract types in Fortran are defined by:

```fortran
type, abstract :: BaseClass
contains
    procedure(getId), deferred, public :: getId
end type

abstract interface
    function getId(this) result(id)
        class(BaseClass), intent(in) :: this
        integer :: id
    end function
end interface
```

This is done in C++ by:

```cxx
class BaseClass{
public:
    virtual int getId() = 0;
}
```

The combination of the `virtual` keyword and the curious `=0` syntax means that extensions of `BaseClass` MUST implement it.
The `virtual` keyword marks a method that can be overriden, but it can be implemented in the class that declares it. This
is a "non-pure virtual" method. Methods not marked as `virtual` cannot be overriden in class extensions.

If the `=0` is not used, it appears that the issue will only be caught at the linking stage. This is because the method is assumed
to be defined by the base class. By using `=0`, this says that this method is NOT DEFINED by the base class.

```cxx
class BaseClass{
public:
    virtual int getId();
};

#include <iostream>
int main(){
    BaseClass b;
    std::cout << b.getId();
}
```

```bash
>>> g++ test.cpp 
/opt/rh/gcc-toolset-10/root/usr/bin/ld: /tmp/ccYCHi2T.o: in function `main':
test.cpp:(.text+0x9): undefined reference to `vtable for BaseClass'
/opt/rh/gcc-toolset-10/root/usr/bin/ld: test.cpp:(.text+0x19): undefined reference to `BaseClass::getId()'
collect2: error: ld returned 1 exit status
```

If the `=0` is used, it appears to get caught in compile step with MUCH MORE HELPFUL ERROR MESSAGES:

```cxx
class BaseClass{
public:
                             //**************************
    virtual int getId() = 0; // ONLY DIFFERENCE IS `=0`
                             //**************************
};

#include <iostream>
int main(){
    BaseClass b;
    std::cout << b.getId();
}
```

```bash
>>> g++ test.cpp 
test.cpp: In function ‘int main()’:
test.cpp:11:15: error: cannot declare variable ‘b’ to be of abstract type ‘BaseClass’
   11 |     BaseClass b;
      |               ^
test.cpp:1:7: note:   because the following virtual functions are pure within ‘BaseClass’:
    1 | class BaseClass{
      |       ^~~~~~~~~
test.cpp:4:17: note:     ‘virtual int BaseClass::getId()’
    4 |     virtual int getId() = 0;
      |                 ^~~~~
```

Abstract types are implemented in Fortran by:

```fortran
type, extends(BaseClass) :: ExtClass
contains
    procedure, public :: getId => getId_ExtClass
end type
```

And in C++:

```cxx
class ExtClass : public BaseClass {
public:
    int getId() override;
}
```
