Passing arguments to a program from the command line

```fortran
program main
    integer :: argc
    character(256) :: arg

    call command_argument_count(argc)
    print *, 'Number of arguments passed on command line: ', argc

    do k = 1, argc
        call get_command_argument(k,arg)
        print *, k, '"'//trim(adjustl(arg))//'"'
    end do
end program
```

```cxx
#include <iostream>
int main(int argc, char* argv[]){
    std::cout << "Number of arguments passed on the command line: " << argc << "\n";

    for(int k = 1; k < argc; k++){
        std::cout << k << " \"" << argv[k] << "\"\n";
    }
}
```