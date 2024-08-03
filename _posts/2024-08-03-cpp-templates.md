---
layout: post
title: C++ Templates
date: 2024-08-03 22:09:00
description: essentials of C++ templates
tags: C++ STL
categories: tech
featured: true
---



## Brief intro to templates [^1]

Template methodology suits for generic designing
1. function template
2. class template

The differences between these two types:
- function template can infer the data type from its parameter, so it need not explicit declaration of data type that passed in, but for class templates this need to be done explicitly


Example of templates:
```cpp
// with standard C++ 11 and above
#include <iostream> // std::cout
#include <iterator> // std::ostream_iterator
#include <algorithm> // std::copy

template<typename T>
T square(T x){
    return x*x;
}

template<typename T>
class MyVector {
    T arr[1000];
    int size;

public:
    MyVector() : size(0) {}

public:
    void push_back(T x) {arr[size] = x; size++; }
    T& operator[] (int) ;
    MyVector<T> operator* (MyVector<T>& ) const;
    void print() const;
};


template<typename T>
void MyVector<T>::print() const {
    std::cout << "Vector Size : " << size << ", Elements: " << std::endl;
    std::copy(
        std::begin(arr), std::begin(arr)+size,
        std::ostream_iterator<int>(std::cout, ","));
}

template<typename T>
T& MyVector<T>::operator[] (int i)  {
    return arr[i];
}

template<typename T>
MyVector<T> MyVector<T>::operator*  (MyVector<T>& v_other) const {
    MyVector<T> v_res;
    for (int i=0; i<this->size; i++){
        v_res.push_back(this->arr[i] * v_other[i]);
    }
    return v_res;
}


int main() {
    MyVector<int> vec;
    vec.push_back(2);
    vec.push_back(3);
    vec.push_back(7);

    std::cout<<"Print MyVector: \n";
    vec.print();

    std::cout<<"\nPrint square of MyVector: \n";
    square(vec).print();

}

```

## Code bloat [^2]

Def:
code bloat refer to the production of code is unnecessarily long, and hence results in software
Inefficiency, slow in execution and compiling.

Reasons behind include
1. Definition in header file would compile every time when it is used in any source code file separately. Even though any functionality feature (i.e function etc) from that header is never used, it will still be compiled. This slows the built time.
2. As downside from template usage, every instance of a template is a completely separate piece of code generate by compiler. Templates were compiled as many times as they were called for each instantiation as per specific used parameters.

Ways to overcome this issue
1. Define template functions in source file and explicitly instantiate them.
2. Use extern template declarations in header, combined with explicit template instantiations in source files itself.





## Reference
- - -
[^1]: [[\[Learn STL: Introduction of Templates\](https:\/\/www.youtube.com\/watch?v=Vc1RyqWFbiA\&list=PL5jc9xFGsL8G3y3ywuFSvOuNm3GjBwdkb\&index=2)]]
[^2]: [[\[Code Bloating in C++ with Examples\](https:\/\/www.geeksforgeeks.org\/code-bloating-in-c-with-examples\/)]]


