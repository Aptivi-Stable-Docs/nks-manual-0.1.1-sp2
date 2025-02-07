---
description: Use Filesystem.NeutralizePath()
icon: chart-mixed
---

# Files - NKS0023

This analyzer provides the following strings:

<table><thead><tr><th width="174">Context</th><th>String</th></tr></thead><tbody><tr><td>Error List</td><td>Caller uses <code>Path.GetFullPath</code> instead of <code>Filesystem.NeutralizePath()</code></td></tr><tr><td>Suggestion Box</td><td>Use <code>Filesystem.NeutralizePath()</code> instead of <code>Path.GetFullPath</code></td></tr><tr><td>Description</td><td><code>Filesystem.NeutralizePath()</code> neutralizes the provided path to its absolute correct path, but also gives a path separated by the platform-agnostic path separator.</td></tr></tbody></table>

### Extended Description

This code analyzer detects the usage of `GetFullPath` from the standard `Path` class found in the `System.IO` namespace.

While `Path.GetFullPath()` operates on the executable project directory, it doesn't represent the state of the current working directory because the UESH shell doesn't use the built-in current working directory property.

`NeutralizePath()` had to be made to not only solve this problem, but also gives you a unified path that has folder names spearated by the platform-agnostic path separator.

### Analysis Comparison

To get a brief insight about how this analyzer works, compare the two code blocks shown to you below:

#### Before the fix

<pre class="language-csharp" data-title="Somewhere in your mod code..." data-line-numbers><code class="lang-csharp">public static void MyFunction()
{
<strong>    string path = Path.GetFullPath("test.txt");
</strong>}
</code></pre>

#### After the fix

<pre class="language-csharp" data-title="Somewhere in your mod code..." data-line-numbers><code class="lang-csharp">public static void MyFunction()
{
<strong>    string path = FilesystemTools.NeutralizePath("test.txt");
</strong>}
</code></pre>

### Suppression

You can suppress this suggestion by including it in the appropriate place, whichever is convenient.

For more information about how to suppress any warning issued by the Nitrocid analyzer, visit the below page:

{% embed url="https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/suppress-warnings" %}

### Recommendation

We recommend that every caller which use this function use the recommended abovementioned method.
