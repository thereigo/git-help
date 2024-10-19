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

Script Injection
Script injection is when someone enters a script tag in a text box and the value is accepted and
saved in the database. When the data is displayed on the page, the script is triggered and performs a
particular action.
Here is a simple extension method that searches for and destroys malicious tags from HTML using regex:

public static class StringExtensions
{
    public static string Sanitize(this string content)
    {
        // Replace the malicious tags with nothing.
        var maliciousTagsPattern =
        @"<(applet|embed|frameset|head|noframes|noscript|object|
        form|select|option|script|style|title)(.*?)>"+
        "((.|\n)*?)"+
        "</(applet|embed|frameset|head|noframes|noscript|object|
        select|form|option|script|style|title)>";
        var options = RegexOptions.IgnoreCase | RegexOptions.
        Multiline;
        var regex = new Regex(maliciousTagsPattern, options);
        content = regex.Replace(content, @"");
        // Remove the Javascript function on the tags (i.e.
        OnChange="Javascript:<blah blah blah>")
        var inlinePattern = @"<[^>]*=""javascript:[^""]*""[^>]*>";74 Applying Security from the Start
        options = RegexOptions.IgnoreCase;
        var regex2 = new Regex(inlinePattern, options);
        return regex2.Replace(content, @"");
    }
}

While this .Sanitize() extension method removes any malicious tags from a string, if you are
passing in HTML-formatted text, it also removes any tag using any JavaScript events on tags (such as
onclick='alert("gotcha");'). It then returns the sanitized string for use.
Use this extension method like any other extension method with a string:

var sanitizedString = inputFromUser.Sanitize();

You could even extend the method further so that it includes other safeguards, such as encoding the
string before returning it.
Always validate, filter, and sanitize input from the user. No matter what.
