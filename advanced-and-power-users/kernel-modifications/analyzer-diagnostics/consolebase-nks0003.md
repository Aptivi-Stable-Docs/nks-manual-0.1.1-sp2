---
description: Use SetTitle() from ConsoleExtensions
icon: chart-mixed
---

# ConsoleBase - NKS0003

This analyzer provides the following strings:

<table><thead><tr><th width="174">Context</th><th>String</th></tr></thead><tbody><tr><td>Error List</td><td>Caller uses <code>Console.Title</code> instead of <code>SetTitle()</code></td></tr><tr><td>Suggestion Box</td><td>Use <code>SetTitle()</code> instead of <code>Console.Title</code></td></tr><tr><td>Description</td><td><code>SetTitle()</code> uses the VT sequence to set the title, while <code>Console.Title</code> works in certain conditions.</td></tr></tbody></table>

### Extended Description

This code analyzer detects the usage of `Title` from the standard `Console` class found in the `System` namespace. Based on our earlier tests with TMUX and Unix consoles, we've concluded that the VT sequence approach is better than using the normal `Console.Title` setter.

### Analysis Comparison

To get a brief insight about how this analyzer works, compare the two code blocks shown to you below:

#### Before the fix

<pre class="language-csharp" data-title="Somewhere in your mod code..." data-line-numbers><code class="lang-csharp">public static void MyFunction() =>
<strong>    Console.Title = "Hello, world!";
</strong></code></pre>

#### After the fix

<pre class="language-csharp" data-title="Somewhere in your mod code..." data-line-numbers><code class="lang-csharp">public static void MyFunction() =>
<strong>    ConsoleMisc.SetTitle("Hello, world!");
</strong></code></pre>

### Suppression

You can suppress this warning by including it in the appropriate place, whichever is convenient.

For more information about how to suppress any warning issued by the Nitrocid analyzer, visit the below page:

{% embed url="https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/suppress-warnings" %}

### Recommendation

We recommend that every caller which use this property use the recommended abovementioned method.
