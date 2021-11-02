Title: Serializable object to/from file
Published: 11/02/2021
Tags:
    - Important
    - C#
    - Mist
---

 save/restore serializable object to/from file

```csharp
public static void WriteToBinaryFile<T>(string filePath, T objectToWrite, bool append = false)
{
    using (Stream stream = File.Open(filePath, append ? FileMode.Append : FileMode.Create))
    {
        var binaryFormatter = new System.Runtime.Serialization.Formatters.Binary.BinaryFormatter();
        binaryFormatter.Serialize(stream, objectToWrite);
    }
}
```


```csharp
public static T ReadFromBinaryFile<T>(string filePath)
{
    using (Stream stream = File.Open(filePath, FileMode.Open))
    {
        var binaryFormatter = new System.Runtime.Serialization.Formatters.Binary.BinaryFormatter();
        return (T)binaryFormatter.Deserialize(stream);
    }
}
```

Helpful links
*****************

1. [https://docs.microsoft.com/en-us/dotnet/core/tutorials/using-on-macos](https://docs.microsoft.com/en-us/dotnet/core/tutorials/using-on-macos)

2. [https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-new](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-new)