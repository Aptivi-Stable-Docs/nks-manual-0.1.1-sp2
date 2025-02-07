---
description: Use TimeDateRenderersUtc.RenderDateUtc()
icon: chart-mixed
---

# Kernel - NKS0032

This analyzer provides the following strings:

<table><thead><tr><th width="174">Context</th><th>String</th></tr></thead><tbody><tr><td>Error List</td><td>Caller uses <code>TimeDateTools.KernelDateTimeUtc.ToString</code> instead of <code>TimeDateRenderersUtc.RenderDateUtc()</code></td></tr><tr><td>Suggestion Box</td><td>Use <code>TimeDateRenderersUtc.RenderDateUtc()</code> instead of <code>TimeDateTools.KernelDateTimeUtc.ToString</code></td></tr><tr><td>Description</td><td><code>TimeDateRenderersUtc.RenderDateUtc()</code> respects your kernel settings when rendering date.</td></tr></tbody></table>

### Extended Description

This code analyzer detects the usage of `KernelDateTimeUtc.ToString` from the `TimeDateTools` class found in the `KS.Kernel.Time` namespace.

`TimeDateTools.KernelDateTime` contains an override to a function that converts that instance of the current date and time to its string representation. However, there is a function dedicated to that, called `Render()` and its siblings, that renders your target time to its equivalent string representation and also respects your kernel settings.

Similarly, `TimeDateTools.KernelDateTimeUtc` has been given the same treatment, because you can also use the `RenderUtc()` function and its siblings.

### Analysis Comparison

To get a brief insight about how this analyzer works, compare the two code blocks shown to you below:

#### Before the fix

<pre class="language-csharp" data-title="Somewhere in your mod code..." data-line-numbers><code class="lang-csharp">public static void MyFunction()
{
<strong>    string value = TimeDateTools.KernelDateTimeUtc.ToString();
</strong>}
</code></pre>

#### After the fix

<pre class="language-csharp" data-title="Somewhere in your mod code..." data-line-numbers><code class="lang-csharp">public static void MyFunction()
{
<strong>    string value = TimeDateRenderersUtc.RenderDateUtc();
</strong>}
</code></pre>

### Suppression

You can suppress this suggestion by including it in the appropriate place, whichever is convenient.

For more information about how to suppress any warning issued by the Nitrocid analyzer, visit the below page:

{% embed url="https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/suppress-warnings" %}

### Recommendation

We recommend that every caller which use this function use the recommended abovementioned method.
