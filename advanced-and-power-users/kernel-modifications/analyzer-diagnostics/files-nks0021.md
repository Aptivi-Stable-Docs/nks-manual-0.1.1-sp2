---
description: Use Checking.Rooted()
icon: chart-mixed
---

# Files - NKS0021

This analyzer provides the following strings:

<table><thead><tr><th width="174">Context</th><th>String</th></tr></thead><tbody><tr><td>Error List</td><td>Caller uses <code>Path.IsPathRooted</code> instead of <code>Checking.Rooted()</code></td></tr><tr><td>Suggestion Box</td><td>Use <code>Checking.Rooted()</code> instead of <code>Path.IsPathRooted</code></td></tr><tr><td>Description</td><td><code>Checking.Rooted()</code> uses the filesystem driver to call <code>Path.IsPathRooted</code>.</td></tr></tbody></table>

### Extended Description

This code analyzer detects the usage of `Delete` from the standard `Directory` class found in the `System.IO` namespace.

Using `Path.IsPathRooted()`, it doesn't use any driver to wrap this call, so it calls the built-in `IsPathRooted()` directly.

Alternatively, you can use `Checking.Rooted()`, which uses the filesystem driver for extensibility.

### Analysis Comparison

To get a brief insight about how this analyzer works, compare the two code blocks shown to you below:

#### Before the fix

<pre class="language-csharp" data-title="Somewhere in your mod code..." data-line-numbers><code class="lang-csharp">public static void MyFunction()
{
<strong>    Path.IsPathRooted("C:/test.txt");
</strong>}
</code></pre>

#### After the fix

<pre class="language-csharp" data-title="Somewhere in your mod code..." data-line-numbers><code class="lang-csharp">public static void MyFunction()
{
<strong>    Checking.Rooted("C:/test.txt");
</strong>}
</code></pre>

### Suppression

You can suppress this suggestion by including it in the appropriate place, whichever is convenient.

For more information about how to suppress any warning issued by the Nitrocid analyzer, visit the below page:

{% embed url="https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/suppress-warnings" %}

### Recommendation

We recommend that every caller which use this function use the recommended abovementioned method.
