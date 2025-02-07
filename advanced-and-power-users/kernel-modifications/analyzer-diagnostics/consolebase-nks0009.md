---
description: Use ConsoleTools.ResetColors()
icon: chart-mixed
---

# ConsoleBase - NKS0009

This analyzer provides the following strings:

<table><thead><tr><th width="174">Context</th><th>String</th></tr></thead><tbody><tr><td>Error List</td><td>Caller uses <code>Console.ResetColor</code> instead of <code>ConsoleExtensions.ResetColors()</code></td></tr><tr><td>Suggestion Box</td><td>Use <code>ConsoleTools.ResetColors()</code> instead of <code>Console.ResetColor</code></td></tr><tr><td>Description</td><td><code>ConsoleTools.ResetColors()</code> contains VT sequences that help reset colors in a portable way.</td></tr></tbody></table>

### Extended Description

This code analyzer detects the usage of `ResetColor` from the standard `Console` class found in the `System` namespace.

While `ResetColor()` is enough to reset all the colors, we felt that using simple VT sequences to achieve the same goal is more viable. Therefore, we've made the `ResetColors()` function that allows you to simply reset the colors.

### Analysis Comparison

To get a brief insight about how this analyzer works, compare the two code blocks shown to you below:

#### Before the fix

<pre class="language-csharp" data-title="Somewhere in your mod code..." data-line-numbers><code class="lang-csharp">public static void MyFunction()
{
<strong>    Console.ResetColor();
</strong>}
</code></pre>

#### After the fix

<pre class="language-csharp" data-title="Somewhere in your mod code..." data-line-numbers><code class="lang-csharp">public static void MyFunction()
{
<strong>    ConsoleTools.ResetColors();
</strong>}
</code></pre>

### Suppression

You can suppress this suggestion by including it in the appropriate place, whichever is convenient.

For more information about how to suppress any warning issued by the Nitrocid analyzer, visit the below page:

{% embed url="https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/suppress-warnings" %}

### Recommendation

We recommend that every caller which use this function use the recommended abovementioned method.
