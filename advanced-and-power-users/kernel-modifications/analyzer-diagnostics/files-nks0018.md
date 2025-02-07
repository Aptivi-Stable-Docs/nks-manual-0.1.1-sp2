---
description: Use Checking.FileExists()
icon: chart-mixed
---

# Files - NKS0018

This analyzer provides the following strings:

<table><thead><tr><th width="174">Context</th><th>String</th></tr></thead><tbody><tr><td>Error List</td><td>Caller uses <code>File.Exists</code> instead of <code>Checking.FileExists()</code></td></tr><tr><td>Suggestion Box</td><td>Use <code>Checking.FileExists()</code> instead of <code>File.Exists</code></td></tr><tr><td>Description</td><td><code>Checking.FileExists()</code> neutralizes the provided path to its absolute correct path, while <code>File.Exists</code> operates at the executable directory (<code>Environment.CurrentDirectory</code>), which may not be what you want.</td></tr></tbody></table>

### Extended Description

This code analyzer detects the usage of `Exists` from the standard `File` class found in the `System.IO` namespace.

Using `File.Exists()`, path neutralization doesn't take place to ensure that we have the correct absolute path. This causes some of the filesystem operations to operate on files, which are located in the wrong place, and, therefore, errors throw about a target not being found.

A solution to this problem was made with `FileExists()`, because it takes care of the absolute paths and tries to reduce this kind of error caused by passing relative directories to the arguments.

### Analysis Comparison

To get a brief insight about how this analyzer works, compare the two code blocks shown to you below:

#### Before the fix

<pre class="language-csharp" data-title="Somewhere in your mod code..." data-line-numbers><code class="lang-csharp">public static void MyFunction()
{
<strong>    File.Exists("test.txt");
</strong>}
</code></pre>

#### After the fix

<pre class="language-csharp" data-title="Somewhere in your mod code..." data-line-numbers><code class="lang-csharp">public static void MyFunction()
{
<strong>    Checking.FileExists("test.txt");
</strong>}
</code></pre>

### Suppression

You can suppress this suggestion by including it in the appropriate place, whichever is convenient.

For more information about how to suppress any warning issued by the Nitrocid analyzer, visit the below page:

{% embed url="https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/suppress-warnings" %}

### Recommendation

We recommend that every caller which use this function use the recommended abovementioned method.
