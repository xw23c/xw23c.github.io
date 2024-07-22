
[Statement] Archive notes series reading through LearnCpp.Com, an insightful source, very worth reading
            and freely accessible to everyone anonymously. Thanks to Alex.





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
    -- keyword: **aggregate**


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

[15.1] Hidden pointer *this*

Def:
Inside every member function, the keyword this is a const pointer that holds the address of the current implicit object.

- All non-static member functions/variables have a *this* const pointer that holds the address of the implicit object.
- Because this is just a function parameter (and not a member), it does not make instances of your class larger memory-wise.
- Explicit usecase:
    1. (distinguash class member variable wrt function local variable) First, if you have a member function that has a parameter with the same name as a data member, you can disambiguate them by using this
    2. (method chaining) Second, it can sometimes be useful to have a member function return the implicit object as a return value. The primary reason to do this is to allow member functions to be “chained” together, so several member functions can be called on the same object in a single expression! This is called function chaining (or method chaining).
    3. (class reset) If your class has a default constructor, you may be interested in providing a way to return an existing object back to its default state.

- For non-const member functions, *this* is a const pointer to a non-const value (meaning this cannot be pointed at something else, but the object pointing to may be modified). With const member functions, *this* is a const pointer to a const value (meaning the pointer cannot be pointed at something else, nor may the object being pointed to be modified). Errors got generated from attempting to call a non-const member on a const object.

- *this* defined as a pointer, not a reference. In other more modern C++-like languages, such as Java and C#, this is implemented as a reference.


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
