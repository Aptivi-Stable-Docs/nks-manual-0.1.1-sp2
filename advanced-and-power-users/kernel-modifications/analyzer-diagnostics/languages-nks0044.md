---
description: Use CultureManager.CurrentCult
icon: chart-mixed
---

# Languages - NKS0044

This analyzer provides the following strings:

<table><thead><tr><th width="174">Context</th><th>String</th></tr></thead><tbody><tr><td>Error List</td><td>Caller uses <code>CultureInfo.CurrentUICulture</code> instead of <code>CultureManager.CurrentCult</code></td></tr><tr><td>Suggestion Box</td><td>Use <code>CultureManager.CurrentCult</code> instead of <code>CultureInfo.CurrentUICulture</code></td></tr><tr><td>Description</td><td><code>CultureManager.CurrentCult</code> gives you a current culture that is set by the kernel settings without affecting the host system.</td></tr></tbody></table>

### Extended Description

This code analyzer detects the usage of `CurrentUICulture` from the `CultureInfo` class found in the `System.Globalizaiton` namespace.

The `CultureInfo.CurrentUICulture` property gives you your current UI culture for your applications. However, Nitrocid is not a GUI application, so we've decided to make our own culture handler that respects your kernel settings.

It's advisable to use the `CultureManager.CurrentCult` property so that your mod can respect the kernel settings when it comes to cultures.

### Analysis Comparison

To get a brief insight about how this analyzer works, compare the two code blocks shown to you below:

#### Before the fix

<pre class="language-csharp" data-title="Somewhere in your mod code..." data-line-numbers><code class="lang-csharp">public static void MyFunction()
{
<strong>    var culture = CultureInfo.CurrentUICulture;
</strong>}
</code></pre>

#### After the fix

<pre class="language-csharp" data-title="Somewhere in your mod code..." data-line-numbers><code class="lang-csharp">public static void MyFunction()
{
<strong>    var culture = CultureManager.CurrentCult;
</strong>}
</code></pre>

### Suppression

You can suppress this suggestion by including it in the appropriate place, whichever is convenient.

For more information about how to suppress any warning issued by the Nitrocid analyzer, visit the below page:

{% embed url="https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/suppress-warnings" %}

### Recommendation

We recommend that every caller which use this function use the recommended abovementioned method.
