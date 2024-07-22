
[5.1] Const

- Making a function parameter constant enlists the compiler’s help to ensure that the parameter’s
value is not changed inside the function. However, in modern C++ we don’t make value parameters
const because we generally don’t care if the function changes the value of the parameter
(since it’s just a copy that will be destroyed at the end of the function anyway).
The const keyword also adds a small amount of unnecessary clutter to the function prototype.

- Don’t use const when passing by value / returning by value
- Prefer constant variables over object-like macros with substitution text.



[14.1-.3] Class intro, member variables/functions


- class solving the issue of invariant violation from source
- class are naturally modualar
- member variables and functions can be defined in any order !
- class member functions can be overloaded !
- class without member data is an overkill, and using namespace would be a bette choice.
    This makes it clear no member data is being managed and does not requre object
    initiation to call the funtion



Q: what is the difference between class and struct?
A: In modern C++, it is fine for structs to have member functions. This excludes constructors,
    which are a special type of member function that we cover in upcoming lesson 14.9
    -- Introduction to constructors. A class type with a constructor is no longer an aggregate,
    and we want our structs to remain aggregates.
    -- keyword: **aggreagte**


[14.9] Class constructor!

Def (keyw: "setup"):
A constructor is a special member function that is automatically called after a non-aggregate
    class type object is created. It prevents the creation if no existent matching creator.
* Constructors must have the same name as the class (with the same capitalization).
    For template classes, this name excludes the template parameters.
* Constructors have no return type (not even void).
* Because constructors are typically part of the interface for your class, they are usually public.

- initialize members variables, and do any other set up tasks required to ensure objects of the class are ready for use.
    (legally, compulsively, sufficiently, keep class invariant)
- enable member emcapsulated (private)



Q: aggreagte vs non-aggreagte class type
A: 1. aggreagte take no private member (invalid), so non-aggreagte (class), no longer able to use
    aggreagte-initialization.

[??.?] *this*

[??.?] *new*

[??.?] -foreach-


[15.4] Distructor!

Def (keyw: "cleanup"):

To generalize this issue, classes that use a resource (most often memory, but sometimes files,
databases, network connections, etc…) often need to be explicitly sent or closed before the
class object using them is destroyed. In other cases, we may want to do some record-keeping
prior to the destruction of the object, such as writing information to a log file, or sending
a piece of telemetry to a server. The term “clean up” is often used to refer to any set of tasks
that a class must perform before an object of the class is destroyed in order to behave as expected.
If we have to rely on the user of such a class to ensure that the function that performs clean
up is called prior to the object being destroyed, we are likely to run into errors somewhere.

Distructor is called automatically when an object of a non-aggregate class type is destroyed,
designed to allow a class to do any necessary clean up before an object of the class is destroyed.

* The destructor must have the same name as the class, preceded by a tilde (~).
* The destructor can not take arguments.
* The destructor has no return type.

- Unhandled exceptions will also cause the program to terminate, and may not unwind the stack before doing so.
If stack unwinding does not happen, destructors will not be called prior to the termination of the program.
