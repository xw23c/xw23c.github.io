---
layout: post
title: Unordered Associative Container
date: 2024-08-04 16:38:00
description: STL unordered containers
tags: C++ STL container
categories: tech
featured: true
---



## The containers
The order of elements is not defined and it changes over the time. Internally they are implemented by hash table (an array of linked list, a.k.a array of buckets, the grey boxes in bellow graph,  and the slots on the linked list also called entries).
> Fast and effective hash function can guarantee fast search time (constant)
{: .block-tip }


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/unorderedset.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/unorderedmap.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

<div class="caption">
    *Figure 1. Internal structure of unordered sets/multisets(left) and map/multimaps(right)* [^1]
</div>


| unordered accos. containers | header           | RA    | dup.   | feature               | Impl.      |
|-----------------------------|------------------|-------|--------|-----------------------|------------|
| unordered_set<T>            | <unordered_set>  | false | forbid | value cant be changed | hash table |
| unordered_multiset<T>       | <unordered_set>  | false | allow  | value cant be changed | hash table |
| unordered_map<T, T>         | <unordered_map>￼ | false | forbid | key cant be changed   | hash table |
| unordered_multimap<T, T>    | <unordered_map>￼ | false | allow  | key cant be changed   | hash table |

Note the change of value (value for sets, key for maps) will corrupt the data structure of hash table, thus it is forbidden by definition in nature.
> Unordered set/multiset: element value cannot be changed
> Unordered map/multimap: element key cannot be changed
{: .block-warning }

> Multimaps do not promise unique keys, therefore they don’t support subscript operator [-].
{: .block-warning }

Example code (unorder_set):
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <unordered_set>

using namespace std;



int main() {

    /*  unordered map
    */
    unordered_set<string> myset{"red", "green", "blue"};
    unordered_set<string>::const_iterator it = myset.find("red"); // O(1)

    if (it!=myset.end())     // check if element found
        cout << *it << endl;

    myset.insert("yellow"); // O(1)

    vector<string> vec{"purple", "pink"};
    myset.insert(vec.begin(), vec.end());


    /*  hash table APIs
    */


    cout << "load_factor(total num of elements / total num of buckets)"
        << myset.load_factor() << endl;

    // string x{"red"};
    for (string x: myset) {
        cout << x << " is in bucket : " << myset.bucket(x) << endl;
    }
    cout << "Total bucket count: " << myset.bucket_count() << endl;

    return 0;

}

```

Example code (unordered_map):
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <unordered_map>

using namespace std;


void foo(const unordered_map<char,  string>& m);

int main() {
    unordered_map<char, string> day = {{'S', "Sunday"}, {'M', "Monday"}};

    cout << day['S'] << endl; // No range check
    cout << day.at('S') << endl; // Has range check

    vector<int> vec{1, 2, 3};
    vec[5] = 5 ; // Compile Error, vec[5] not created yet

    day['W'] = "Wednesday";  // Inserting {'W', "wednesday"}
    day.insert(make_pair('F', "Friday"));

    day.insert(make_pair('M', "MONDAY")); // fail to modify. For unordered_map, cannot use insert() to modify elements
    day['M'] = "MONDAY";                  // succeed to modify. Subscript operator provide write access to the container


    foo(day);

    return 0;
}

void foo(const unordered_map<char,  string>& m) {

    /* line bellow fails to compile.
    Compiler sees the subscript operrator [-], which provide write access,
    and this vialate the const keyword protect
    */
    cout << m['S'] << endl;

    // to print element of S
    if (auto it = m.find('S'); it!=m.end())
        cout << "key: " << it->first << ", value: " << it->second << endl;
    else
        cout << "not found!" << endl;

}

```

## Performance [^2]

| *action*                       | *unordered associative containers*  |
|--------------------------------|-------------------------------------|
|                                | unordered set/multiset/map/multimap |
| insert/remove (.insert/.erase) | $$O(1)$$ - amortized constant       |
| search (.find)                 | $$O(1)$$ - amortized                |
| traverse                       |                                     |
They possess the **fastest** insert and search time at any place - $$O(1)$$. However, this is **amortized** constant time due to potential rare case of  hash collisions. (*Amortized time* here indicates “average time taken per operation given that the same operation repeats many times”[^3])

### Hash collision
This indicate many items are inserted at the same buckets, and majority of them then are only inserted into a few buckets. The performance of searching will degrade from constant time $$O(1)$$ to linear time $$O(n)$$

> Unordered map provides amortized constant time, but it may degrade to linear time, while ordered map can guarantee $$O(log(n)$$ time. This is important for real time system.
{: .block-tip }


### Reference

[^1]: Nicolai M. Josuttis, *The C++ Standard Library 2nd Edition* : p. 358
[^2]: Bo Qian, *[Introduction of STL \#4:  Unordered Containers](https://www.youtube.com/watch?v=NNLvY9O7ufU&list=PL5jc9xFGsL8G3y3ywuFSvOuNm3GjBwdkb&index=5)*
[^3]: [what-is-constant-amortized-time](https://stackoverflow.com/questions/200384/what-is-constant-amortized-time)
