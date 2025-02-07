---
description: Use TWC.Write()
icon: chart-mixed
---

# ConsoleBase - NKS0007

This analyzer provides the following strings:

<table><thead><tr><th width="174">Context</th><th>String</th></tr></thead><tbody><tr><td>Error List</td><td>Caller uses <code>Console.Write</code> instead of <code>TWC.Write()</code></td></tr><tr><td>Suggestion Box</td><td>Use <code>TWC.Write()</code> instead of <code>Console.Write</code></td></tr><tr><td>Description</td><td><code>TextWriterColor.Write()</code> contains workarounds for VT sequences needed for Linux hosts to properly report the correct position post-write. Its overloads also allow you to specify the color and the line writing.</td></tr></tbody></table>

### Extended Description

This code analyzer detects the usage of `Write` from the standard `Console` class found in the `System` namespace. The `Write` function from that class used to be so good in Nitrocid's very early days until we started to support Linux systems.

Since then, we were suffering from the VT sequence problems and the correct positioning of the Linux console cursor, causing `Console.CursorLeft` and `CursorTop` to report incorrect values. We've made a wrapper class that works around this problem using the `GetFilteredPositions` function from `ConsoleExtensions`, and it worked well on most situations, especially after the usage of the master VT sequence regular expression matching.

`TextWriterColor.Write()` deals with this scenario before trying to use the plain writer to write to the console so that the cursor positioning is correct after the write. It also uses your current console driver for further flexibility.

### Analysis Comparison

To get a brief insight about how this analyzer works, compare the two code blocks shown to you below:

#### Before the fix

<pre class="language-csharp" data-title="Somewhere in your mod code..." data-line-numbers><code class="lang-csharp">public static void MyFunction()
{
<strong>    Console.Write("My text");
</strong><strong>    Console.Write("Hi! I'm {0}!", "Sarah");
</strong>}
</code></pre>

#### After the fix

<pre class="language-csharp" data-title="Somewhere in your mod code..." data-line-numbers><code class="lang-csharp">public static void MyFunction()
{
<strong>    TextWriterColor.Write("My text", false);
</strong><strong>    TextWriterColor.Write("Hi! I'm {0}!", false, "Sarah");
</strong>}
</code></pre>

### Suppression

You can suppress this suggestion by including it in the appropriate place, whichever is convenient.

For more information about how to suppress any warning issued by the Nitrocid analyzer, visit the below page:

{% embed url="https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/suppress-warnings" %}

### Recommendation

We recommend that every caller which use this property use the recommended abovementioned method.
