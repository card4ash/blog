Title: Custom Model Validation
Published: 06/16/2019
Tags:
    - C#
    - Asp.net Core
    - Web API
    - Json Patch
    - HTTP Patch
    - Introduction
    - Incomplete
---

Custom Model Validation
=========================

```csharp
public class Result
{
	public Result(bool success, string message)
	{
		Success = success;
		Message = message;
	}

	public bool Success { get; private set; }

	public string Message { get; private set; }
}
```

```csharp
private Result CheckModel(string test,string test2)
{
	var errors = new List<string>();
	
	if (!test.Equals("TEST"))
	{
		errors.Add("Input sting must be TEST");
	}

	string test2 = sheet.Cell(1, 2).Value.ToString().Trim();
	if (!test2.Equals("MORE TEST"))
	{
		errors.Add("Input sting must be MORE TEST");
	}

	
	var success = !errors.Any();
	var message = success ? "Validation successful." : string.Join(" ", errors);
	return new Result(success, message);
}
```