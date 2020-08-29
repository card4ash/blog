Title: Parameter passing in C# Passing by reference vs. passing by value
Published: 06/16/2019
Tags:
    - C#
    - Parameter
    - Introduction
---

Parameter passing in C# Passing by reference vs. passing by value
===================================================================

there is a nice article about it by John skeet.A [complementary article](http://www.leerichardson.com/2007/01/parameter-passing-in-c.html) to this one. [Here](https://jonskeet.uk/csharp/parameters.html) is the John skeet article. Some parts of it is as follows


Sidenote: what is the difference between passing a value object by reference and a reference object by value?
You may have noticed that the last example, passing a struct by reference, had the same effect in this code as passing a class by value. This doesn't mean that they're the same thing, however. Consider the following code:

```csharp
void Foo (??? IntHolder x)
{
    x = new IntHolder();
}

...

IntHolder y = new IntHolder();
y.i=5;
Foo (??? y);
```
Consider the case where IntHolder is a struct (i.e. a value type) and the parameter is a reference parameter (i.e. replace ??? with ref above). After the call to Foo(ref y), the value of y is a new IntHolder value - i.e. y.i is 0.

In the case where IntHolder is a class (i.e. a reference type) and the parameter is a value parameter (i.e. remove ??? above), the value of y isn't changed - it's a reference to the same object it was before the function member call. This difference is absolutely crucial to understanding parameter passing in C#, and is why I believe it is highly confusing to say that objects are passed by reference by default instead of the correct statement that object references are passed by value by default.