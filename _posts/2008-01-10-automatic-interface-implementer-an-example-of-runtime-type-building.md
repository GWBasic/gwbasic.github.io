---
layout: post
title:  "Automatic Interface Implementer: An Example of Runtime Type Building"
date:   2008-01-10
categories: C# interfaces softwaredevelopment
---
I wanted to experiment with using .NET's TypeBuilder class to automatically generate classes at runtime. For my experiment, I decided to implement a function, that, given an interface, returns a fully functional object that implements the interface. The programmer does not have to create a class to implement the interface.

In this article, I will first describe how one uses the automatic interface implementer. Later, I will describe how I used reflection and .NET's TypeBuilder to, at runtime, dynamically create Types that implement interfaces.

[Automatic Interface Implementer: An Example of Runtime Type Building, on codeproject](https://www.codeproject.com/Articles/22832/Automatic-Interface-Implementer-An-Example-of-Runt)
