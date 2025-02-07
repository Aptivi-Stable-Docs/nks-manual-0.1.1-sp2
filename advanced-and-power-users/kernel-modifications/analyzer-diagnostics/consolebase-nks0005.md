---
description: Use SetConsoleColor(Color, true) from KernelColorTools
icon: chart-mixed
---

# ConsoleBase - NKS0005

This analyzer provides the following strings:

<table><thead><tr><th width="174">Context</th><th>String</th></tr></thead><tbody><tr><td>Error List</td><td>Caller uses <code>Console.BackgroundColor</code> instead of <code>SetConsoleColor(Color, true)</code></td></tr><tr><td>Suggestion Box</td><td>Use <code>SetConsoleColor(Color, true)</code> instead of <code>Console.BackgroundColor</code></td></tr><tr><td>Description</td><td><code>SetConsoleColor(Color, true)</code> not only brings better color support provided by the appropriate VT sequences, but it can also use true color. <code>Console.BackgroundColor</code> only handles 16 colors.</td></tr></tbody></table>

### Extended Description

This code analyzer detects the usage of `BackgroundColor` from the standard `Console` class found in the `System` namespace. This property only supports up to 16 colors, and, in order to upgrade to 255 colors or true color, you need to have a console that supports it and a VT escape sequence to be able to set the background color of the console in any color you want.

As a result, `SetConsoleColor` simplifies things up by using the `Color` class provided by Terminaux to be able to set the color without being restricted to the domain of the 16 colors.

### Analysis Comparison

To get a brief insight about how this analyzer works, compare the two code blocks shown to you below:

#### Before the fix

<pre class="language-csharp" data-title="Somewhere in your mod code..." data-line-numbers><code class="lang-csharp">public static void MyFunction() =>
<strong>    Console.BackgroundColor = ConsoleColor.Green;
</strong></code></pre>

#### After the fix

<pre class="language-csharp" data-title="Somewhere in your mod code..." data-line-numbers><code class="lang-csharp">public static void MyFunction() =>
<strong>    ColorTools.SetConsoleColor(ConsoleColors.Green, true);
</strong></code></pre>

### Suppression

You can suppress this suggestion by including it in the appropriate place, whichever is convenient.

For more information about how to suppress any warning issued by the Nitrocid analyzer, visit the below page:

{% embed url="https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/suppress-warnings" %}

### Recommendation

We recommend that every caller which use this property use the recommended abovementioned method.
