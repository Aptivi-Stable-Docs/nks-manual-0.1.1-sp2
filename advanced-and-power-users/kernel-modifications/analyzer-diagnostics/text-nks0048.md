---
description: Use TextTools.SplitNewLines()
icon: chart-mixed
---

# Text - NKS0048

This analyzer provides the following strings:

<table><thead><tr><th width="174">Context</th><th>String</th></tr></thead><tbody><tr><td>Error List</td><td>Caller uses <code>.Split("\r\n")</code> instead of <code>TextTools.SplitNewLines()</code></td></tr><tr><td>Suggestion Box</td><td>Use <code>TextTools.SplitNewLines()</code> instead of <code>.Split("\r\n")</code></td></tr><tr><td>Description</td><td><code>TextTools.SplitNewLines()</code> simplifies the readability of the split by new lines function.</td></tr></tbody></table>

### Extended Description

This code analyzer detects the usage of `Split("\r\n")` from the `String` class found in the `System` namespace.

Splitting by new lines is a good way to get the lines from your string. However, the complexity needs to be reduced so that more readability is achieved. As a result, `SplitNewLines()` is here to do it in the simplest way possible.

### Analysis Comparison

To get a brief insight about how this analyzer works, compare the two code blocks shown to you below:

#### Before the fix

<pre class="language-csharp" data-title="Somewhere in your mod code..." data-line-numbers><code class="lang-csharp">public static void MyFunction()
{
    string var = "Hello\r\nWorld!";
<strong>    var split = var.Split("\r\n");
</strong>}
</code></pre>

#### After the fix

<pre class="language-csharp" data-title="Somewhere in your mod code..." data-line-numbers><code class="lang-csharp">public static void MyFunction()
{
    string var = "Hello\r\nWorld!";
<strong>    var split = var.SplitNewLines();
</strong>}
</code></pre>

#### After the fix (alternate)

<pre class="language-csharp" data-title="Somewhere in your mod code" data-line-numbers><code class="lang-csharp">public static void MyFunction()
{
    string var = "Hello\r\nWorld!";
<strong>    var split = var.SplitNewLinesOld();
</strong>}
</code></pre>

### Suppression

You can suppress this suggestion by including it in the appropriate place, whichever is convenient.

For more information about how to suppress any warning issued by the Nitrocid analyzer, visit the below page:

{% embed url="https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/suppress-warnings" %}

### Recommendation

We recommend that every caller which use this function use the recommended abovementioned method.
