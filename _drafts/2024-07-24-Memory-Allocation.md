
[Statement] Archive notes series reading through LearnCpp.Com, an insightful source, very worth reading
            and freely accessible to everyone anonymously. Thanks to Alex.





[19.1]


Def: *Dynamic memory allocation*

Dynamic memory allocation is a way for running programs to request memory from the operating system when needed. This memory does not come from the program’s limited stack memory -- instead, it is allocated from a much larger pool of memory managed by the operating system called the heap. On modern machines, the heap can be gigabytes in size.





*new*

- int* value { new (std::nothrow) int }; // value will be set to a null pointer if the integer allocation fails
- the best practice is to check all memory requests to ensure they actually succeeded before using the allocated memory.
- Dynamically allocated memory has dynamic duration and will stay allocated until you deallocate it or the program terminates.



Def: *Dangling pointer*

C++ does not make any guarantees about what will happen to the contents of deallocated memory, or to the value of the pointer being deleted. In most cases, the memory returned to the operating system will contain the same values it had before it was returned, and the pointer will be left pointing to the now deallocated memory.

A pointer that is pointing to deallocated memory is called a dangling pointer.

- Dereferencing or deleting a dangling pointer will lead to undefined behavior.
- Set deleted pointers to nullptr unless they are going out of scope immediately afterward.


Def: *Memory Leak*

Dynamically allocated memory stays allocated until it is explicitly deallocated or until the program ends (and the operating system cleans it up, assuming your operating system does that). However, the pointers used to hold dynamically allocated memory addresses follow the normal scoping rules for local variables. This mismatch can create interesting problems.

Consider the following function:

void doSomething()
{
    int* ptr{ new int{} };
}

This function allocates an integer dynamically, but never frees it using delete. Because pointers variables are just normal variables, when the function ends, ptr will go out of scope. And because ptr is the only variable holding the address of the dynamically allocated integer, when ptr is destroyed there are no more references to the dynamically allocated memory. This means the program has now “lost” the address of the dynamically allocated memory. As a result, this dynamically allocated integer can not be deleted.

This is called a memory leak. Memory leaks happen when your program loses the address of some bit of dynamically allocated memory before giving it back to the operating system. When this happens, your program can’t delete the dynamically allocated memory, because it no longer knows where it is. The operating system also can’t use this memory, because that memory is considered to be still in use by your program.

Memory leaks eat up free memory while the program is running, making less memory available not only to this program, but to other programs as well. Programs with severe memory leak problems can eat all the available memory, causing the entire machine to run slowly or even crash. Only after your program terminates is the operating system able to clean up and “reclaim” all leaked memory.

Although memory leaks can result from a pointer going out of scope, there are other ways that memory leaks can result. For example, a memory leak can occur if a pointer holding the address of the dynamically allocated memory is assigned another value:

int value = 5;
int* ptr{ new int{} }; // allocate memory
ptr = &value; // old address lost, memory leak results
This can be fixed by deleting the pointer before reassigning it:

int value{ 5 };
int* ptr{ new int{} }; // allocate memory
delete ptr; // return memory back to operating system
ptr = &value; // reassign pointer to address of value
Relatedly, it is also possible to get a memory leak via double-allocation:

int* ptr{ new int{} };
ptr = new int{}; // old address lost, memory leak results



[19.2]

*Dynamically allocate arrays*

- Unlike a fixed array, where the array size must be fixed at compile time, dynamically allocating an array allows us to choose an array length at runtime (meaning our length does not need to be constexpr).


*delete[]*

- One often asked question of array delete[] is, “How does array delete know how much memory to delete?” The answer is that array new[] keeps track of how much memory was allocated to a variable, so that array delete[] can delete the proper amount. Unfortunately, this size/length isn’t accessible to the programmer.




