

[Statement] Archive notes series reading through LearnCpp.Com, an insightful source, very worth reading
            and freely accessible to everyone anonymously. Thanks to Alex.




[5.1] Const

- Making a function parameter constant enlists the compiler’s help to ensure that the parameter’s
value is not changed inside the function. However, in modern C++ we don’t make value parameters
const because we generally don’t care if the function changes the value of the parameter
(since it’s just a copy that will be destroyed at the end of the function anyway).
The const keyword also adds a small amount of unnecessary clutter to the function prototype.

- Don’t use const when passing by value / returning by value
- Prefer constant variables over object-like macros with substitution text.



*const object and this* (15.1) : const member function ?
