---
layout: post
title:  "Object Equality in C#"
date:   2024-10-20
categories: c# programming
---
In ["Object Equality in C#" on Stack Overflow](https://stackoverflow.com/a/54892972), I explained the different kinds of equivalence in programming, and how they are implemented in C#.

To summarize, in programming, there are four kinds of object equivalence:

1. **Reference Equality, object.ReferenceEquals(a, b)**: The two variables point to the same exact object in RAM. (If this were C, both variables would have the same exact pointer.)
2. **Interchangeability, a == b**: The two variables refer to objects that are completely interchangeable. Thus, when a == b, Func(a,b) and Func(b,a) do the same thing.
3. **Semantic Equality, object.Equals(a, b)**: At this exact moment in time, the two objects mean the same thing.
4. **Entity equality, a.Id == b.Id**: The two objects refer to the same entity, such as a database row, but don’t have to have the same contents.
