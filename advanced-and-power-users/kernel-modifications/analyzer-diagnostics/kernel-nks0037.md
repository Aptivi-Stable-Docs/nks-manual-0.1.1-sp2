---
description: Use KernelPlatform.IsOnWindows()
icon: chart-mixed
---

# Kernel - NKS0037

This analyzer provides the following strings:

<table><thead><tr><th width="174">Context</th><th>String</th></tr></thead><tbody><tr><td>Error List</td><td>Caller uses <code>Environment.OSVersion.Platform == PlatformID.Win32NT</code> instead of <code>KernelPlatform.IsOnWindows()</code></td></tr><tr><td>Suggestion Box</td><td>Use <code>KernelPlatform.IsOnWindows()</code> instead of <code>Environment.OSVersion.Platform == PlatformID.Win32NT</code></td></tr><tr><td>Description</td><td><code>KernelPlatform.IsOnWindows()</code> is more readable and less verbose than <code>Environment.OSVersion.Platform == PlatformID.Win32NT</code>.</td></tr></tbody></table>

### Extended Description

This code analyzer detects the comparison of `Environment.OSVersion.Platform` against the `Win32NT` value.

`Environment.OSVersion.Platform` is an ancient way of determining the current operating system that Nitrocid is running on. As a result, it doesn't implement the macOS operating system detection, since it was considered as Unix.

As a result, `KernelPlatform` implements a handful of platform detection functions to allow you to more accurately detect your platform. Also, it simplifies the complicated platform checking statements to its simpler equivalent.

### Analysis Comparison

To get a brief insight about how this analyzer works, compare the two code blocks shown to you below:

#### Before the fix

<pre class="language-csharp" data-title="Somewhere in your mod code..." data-line-numbers><code class="lang-csharp">public static void MyFunction()
{
<strong>    bool value = Environment.OSVersion.Platform == PlatformID.Win32NT;
</strong>}
</code></pre>

#### After the fix

<pre class="language-csharp" data-title="Somewhere in your mod code..." data-line-numbers><code class="lang-csharp">public static void MyFunction()
{
<strong>    bool value = KernelPlatform.IsOnWindows();
</strong>}
</code></pre>

### Suppression

You can suppress this suggestion by including it in the appropriate place, whichever is convenient.

For more information about how to suppress any warning issued by the Nitrocid analyzer, visit the below page:

{% embed url="https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/suppress-warnings" %}

### Recommendation

We recommend that every caller which use this function use the recommended abovementioned method.
