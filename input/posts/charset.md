Title: Character Encoding, Unicode, UTF-8 and the BOM
Published: 10/10/2021
Tags:
    - Character
    - C#
    - Unicode
---

If you run the below code and open the output in text editor like notepad you can see bunch of unrecognizable text. check the default encoding if it choose UTF-16. But if you choose to open it with ANSI you can recongnise a lot of them

```csharp
using System.IO;

byte[] bytes = new byte[128];
for (byte i = 0; i < 128; i++)
{
    bytes[i] = i;
}
File.WriteAllBytes("123.txt",bytes);
```

Helpful links
*****************

1. [https://docs.microsoft.com/en-us/dotnet/core/tutorials/using-on-macos](https://docs.microsoft.com/en-us/dotnet/core/tutorials/using-on-macos)

2. [https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-new](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-new)