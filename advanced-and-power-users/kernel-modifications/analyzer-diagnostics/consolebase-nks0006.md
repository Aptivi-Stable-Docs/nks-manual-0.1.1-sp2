---
description: Use ReadLine() from InputTools
icon: chart-mixed
---

# ConsoleBase - NKS0006

This analyzer provides the following strings:

<table><thead><tr><th width="174">Context</th><th>String</th></tr></thead><tbody><tr><td>Error List</td><td>Caller uses <code>Console.ReadLine</code> instead of <code>ReadLine()</code></td></tr><tr><td>Suggestion Box</td><td>Use <code>ReadLine()</code> instead of <code>Console.ReadLine</code></td></tr><tr><td>Description</td><td><code>ReadLine()</code> provided by the input helper from Nitrocid allows you to seamlessly read a user input with settings provided by Terminaux.</td></tr></tbody></table>

### Extended Description

This code analyzer detects the usage of `ReadLine` from the standard `Console` class found in the `System` namespace. While this function brings some of the awesome features, such as the history and the positioning, they are limited in terms of flexibility.

Historically, it had problems with Mono on Linux not playing nice with these features, causing the console to register an incorrect character to the buffer, messing things up in the process. This was one of the outstanding issues that we had to deal with during the development of 0.0.16.0 in 2021.

The `ReadLine()` provided by Nitrocid KS is a wrapper to Terminaux's console input reader feature, except that the settings are managed by Nitrocid KS and that their histories are saved once the kernel shuts down, further allowing flexibility.

### Analysis Comparison

To get a brief insight about how this analyzer works, compare the two code blocks shown to you below:

#### Before the fix

<pre class="language-csharp" data-title="Somewhere in your mod code..." data-line-numbers><code class="lang-csharp">public static void MyFunction() =>
<strong>    string input = Console.ReadLine();
</strong></code></pre>

#### After the fix

<pre class="language-csharp" data-title="Somewhere in your mod code..." data-line-numbers><code class="lang-csharp">public static void MyFunction() =>
<strong>    string input = InputTools.ReadLine();
</strong></code></pre>

### Suppression

You can suppress this suggestion by including it in the appropriate place, whichever is convenient.

For more information about how to suppress any warning issued by the Nitrocid analyzer, visit the below page:

{% embed url="https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/suppress-warnings" %}

### Recommendation

We recommend that every caller which use this property use the recommended abovementioned method.
