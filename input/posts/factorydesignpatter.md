Title: Factory Design Patter
Published: 02/18/2020
Tags:
    - Design Pattern
    - Factory
    - C#
---

new console app
*******************

```
    dotnet new console -o myapp
```

new solution
*******************
run the dotnet new command to create a new solution golden.sln inside a new folder named golden

```
    dotnet new sln -o golden
```

Now cd into golden and add library project with the following command

```
    dotnet new classlib -o library
```

Execute the dotnet sln command to add the newly created library.csproj project to the solution

```
    dotnet sln add library/library.csproj
```

Helpful links
*****************

1. [https://docs.microsoft.com/en-us/dotnet/core/tutorials/using-on-macos](https://docs.microsoft.com/en-us/dotnet/core/tutorials/using-on-macos)

2. [https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-new](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-new)