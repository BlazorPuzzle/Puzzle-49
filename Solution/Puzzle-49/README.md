# Blazor Puzzle #49

## I Feel Invalidated!

YouTube Video: https://youtu.be/Yw0lXvK_DLQ

Blazor Puzzle Home Page: https://blazorpuzzle.com

### The Challenge:

This is a simple Blazor Web App with Global Server Interactivity.

We have an edit form for a simple model with two data annotations for validation.

What if we want to do some custom validation: for example, we want to **not allow** submitting if the name contains the string "Java" with case-insensitivity.

How can we do that?

### The Solution:

We created a custom validation class based on `ValidationAttribute` where you can specify a string that can not be entered. It can easily be modified to accept an array of strings.

```c#
public class Person
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    [ExcludeStringValidator(ExcludeString="Java", 
        CaseSensitive=false, 
        ErrorMessage="Name cannot contain 'Java'.")]
    public string Name { get; set; }
}

public class ExcludeStringValidator : ValidationAttribute
{
    public string ExcludeString { get; set; } = string.Empty;
    public bool CaseSensitive { get; set; }
    public override bool IsValid(object value)
    {
        var data = value as string;
        if (string.IsNullOrWhiteSpace(data))
            return true;
        if (CaseSensitive)
            return !data.Contains(ExcludeString);
        else
            return !data.Contains(ExcludeString, StringComparison.InvariantCultureIgnoreCase);
    }
}
```

Boom!