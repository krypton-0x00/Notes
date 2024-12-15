Function extensions, or **extension methods**, allow you to add new methods to existing types without modifying their code or creating a derived type

```cs
public static class StringExtensions
{
    // Adds an extension method to the string type
    public static bool IsPalindrome(this string str)
    {
        if (string.IsNullOrEmpty(str)) return false;

        string reversed = new string(str.Reverse().ToArray());
        return string.Equals(str, reversed, StringComparison.OrdinalIgnoreCase);
    }
}

// Usage
class Program
{
    static void Main()
    {
        string word = "level";
        Console.WriteLine(word.IsPalindrome()); // Output: True
    }
}

```
