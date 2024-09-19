using System;
using System.Collections.Generic;
using System.Text.RegularExpressions;
using System.Text.Json;

public class Program
{
    public static void Main()
    {
        // Sample list containing potential dangerous HTML content
        List<string> inputList = new List<string>
        {
            "<script>alert('dangerous code');</script>",
            "<b>Bold Text</b>",
            "<img src='image.jpg' onerror='alert(\"error\")'>",
            "Normal Text"
        };

        // Sanitize the list
        List<string> sanitizedList = SanitizeList(inputList);

        // Serialize the sanitized list to JSON
        string jsonResult = JsonSerializer.Serialize(sanitizedList);
        
        // Output the JSON
        Console.WriteLine(jsonResult);
    }

    // Method to sanitize a list of strings
    public static List<string> SanitizeList(List<string> inputList)
    {
        List<string> sanitizedList = new List<string>();
        
        foreach (var item in inputList)
        {
            // Sanitize each item and add it to the sanitized list
            sanitizedList.Add(SanitizeString(item));
        }

        return sanitizedList;
    }

    // Method to remove HTML and script tags
    public static string SanitizeString(string input)
    {
        if (string.IsNullOrWhiteSpace(input))
            return input;

        // Remove script tags
        string sanitized = Regex.Replace(input, "<script.*?>.*?</script>", "", RegexOptions.IgnoreCase | RegexOptions.Singleline);

        // Remove HTML tags
        sanitized = Regex.Replace(sanitized, "<.*?>", string.Empty);

        return sanitized;
    }
}
