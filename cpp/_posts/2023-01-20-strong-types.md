---
layout: post
title:  "Strong Types ftw :fire:"
author: Brad Fix
tag: c++
category: cpp
---

Strong typing via inheritance, with the ability to inherit 'skills'/operations.

A canonical example: math functions that take *angle* as a float.  
Is it meant to be degrees or radians? Probably radians...?  
It's common for data driven angles (e.g. supplied from script/json) to use degrees as they're easier to visualize.

#### Common legacy function signature
```c++
void apply_some_kind_of_rotation( float angle ); // please pass me radians
apply_some_kind_of_rotation( 90.0 ); // whoops
```

#### Simple implementation of strong angle types
```c++
inline constexpr auto c_pi = 3.1415926536;

struct degree;
struct radian : public roam::strong_type< radian, double, roam::st_cmp, roam::st_math >
{
    using strong_type::strong_type;
    operator degree() const; // implicit conversion to degree
};

struct degree : public roam::strong_type< degree, double, roam::st_cmp, roam::st_math >
{
    using strong_type::strong_type;
    operator radian() const // implicit conversion to radian
    {
        return radian{ this->get() / 180 * c_pi };
    }
};

inline radian::operator degree() const
{
    return degree{ this->get() / c_pi * 180 };
}
```

#### Strong interface with compile-time guarantees
```c++
void apply_some_kind_of_rotation( radian angle );

double my_angle = 90.0;
apply_some_kind_of_rotation( my_angle ); // awesome, doesn't compile!

radian my_angle{ c_pi / 2 };
apply_some_kind_of_rotation( my_angle ); // works as expected

degree my_angle{ 90.0 };
apply_some_kind_of_rotation( my_angle ); // nice, implicitly converts to radian
```
