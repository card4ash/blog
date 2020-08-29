Title: Image Upload Preview
Published: 06/11/2019
Tags: 
    - .Net
    - Asp.net MVC
    - Introduction
---


Image Upload Preview
=========================

HTML Code:
```html
    <form id="form1" runat="server">
        <input type="file" accept="image/*" onchange="loadFile(event)" id="LogoImageFile" name="LogoImageFile" />
        <img id="output"/>
    </form>
```

Javascript Code:

```javascript
    var loadFile = function(event) {
        var output = document.getElementById('output');
        output.src = URL.createObjectURL(event.target.files[0]);
    };
```

Backend controller code:

```c-sharp
    if (model.LogoImageFile != null)
    {
        using (var memoryStream = new MemoryStream())
        {
            await model.LogoImageFile.CopyToAsync(memoryStream);
            model.Person.LogoImage = memoryStream.ToArray();
        }
    }
```