###

A subroutine in Fortran is a procedure that does not return a value like a function does. In Fortran, a subroutine is called by preceeding the
subroutine name with the keyword `call`. For example `call process()`.

```fortran
subroutine increment(i)
    integer, intent(inout) :: i
    i = i + 1
end subroutine
```

```cxx
void increment(int &i){
    i++;
}
```