---
description: Let's go deeper into the filesystem functionality!
icon: cabinet-filing
---

# Nitrocid Filesystem

<figure><img src="../../../.gitbook/assets/063-ifm.png" alt=""><figcaption></figcaption></figure>

Generally, Nitrocid KS offers the fully-fledged filesystem functionality that allows you to perform filesystem operations ranging from simple to complex, like copying files to checking their lock state. This is done with the assistance of the kernel drivers, which we covered previously in the below page:

{% content-ref url="kernel-drivers/" %}
[kernel-drivers](kernel-drivers/)
{% endcontent-ref %}

Your mods can access this, too! However, we'll walk you through the Nitrocid filesystem class hierarchy:

* `Nitrocid.Files`
  * This is the general filesystem namespace containing security-related checks for the validity and openness of the files, like lock checks, which are found in the `FilesystemTools` class.
* `Nitrocid.Files.Attributes`
  * This is the namespace responsible for the attribute management for files.
* `Nitrocid.Files.Editors`
  * This is the namespace that holds the tools for type-specific text file editing tools
* `Nitrocid.Files.Extensions`
  * This is the namespace responsible for the extension management for files.
* `Nitrocid.Files.Folders`
  * This is the namespace that specializes with the listing of folders and querying the current directory.
* `Nitrocid.Files.Instances`
  * This is the namespace that holds filesystem-related classes.
* `Nitrocid.Files.LineEndings`
  * This is the namespace responsible for the management of the line endings for text files.
* `Nitrocid.Files.Operations`
  * This is the namespace which houses common filesystem operations, such as making, copying, opening, or deleting files and folders.
* `Nitrocid.Files.Paths`
  * This is the namespace that is responsible for the management of the kernel path system and the path lookup system.

{% hint style="danger" %}
We advice you not to use `LineEndings` functions on binary files, since they sometimes contain line endings and they may mess with these binary files. Security measures will ensure that these functions throw before any operation is made.
{% endhint %}

{% hint style="warning" %}
For your mods, access to certain filesystem operations require that you consent to accessing your files. This is to ensure increased privacy.
{% endhint %}

{% hint style="info" %}
To learn more about all the available API functions regarding the filesystem, click [here](https://aptivi.github.io/NitrocidKS/api/KS.Files.html).
{% endhint %}

### Path Checks and Lock Checks

Every filesystem operation committed within the `KS.Files` namespace and its functions get their paths checked with the path neutralizer to make a unified version of the path to point to the file relative to either the current path according to the filesystem routine or to the absolute path provided to the neutralizer. This neutralizer is called `Filesystem.NeutralizePath()`.

However, the neutralizer also checks for the validity of the path as a mitigation to the Windows 10 NTFS corruption and forced BSOD bugs by calling the `ThrowOnInvalidPath()` function.

{% hint style="warning" %}
If the conditions were met:

* Attempting to access a Windows path including `$i30`, which is the NTFS volume bitmap, or
* Attempting to open the `kernelconnect` node on `condrv` using a specially crafted path,

The checker will throw a `KernelException` with the `KernelExcceptionType` of `Filesystem` stating that the attempt of accessing these paths is invalid.
{% endhint %}

Optionally, the functions also make use of the file and folder lock checking mechanisms by invoking one of these functions:

* `IsFileLocked()`
* `IsFolderLocked()`
* `IsLocked()`
* `WaitForLockRelease()`
* `WaitForLockReleaseIndefinite()`

Consult the above link to the API reference for more info about how to use these functions to check the lock on your target file.

### Filesystem Entries

`FileSystemEntry` contains necessary information about your file or folder, like checking to see if it exists, determining the type of the entry, and so on. These are the information currently available, but you can obtain more information using the `BaseEntry` property:

* `Exists`: Checks to see if the file or directory exists
* `Type`: The type of the filesystem entry. This is only populated once.
* `OriginalFilePath`: The original file path that was passed to the constructor
* `FilePath`: The neutralized file path
* `FileSize`: The file size. This property's value is -1 if the entry is a directory or non-existent
* `BaseEntryUnprocessed`: The base entry from the unprocessed file path
* `BaseEntry`: The base entry from the neutralized file path

You can create a new instance of this class, assuming that you've imported the `Nitrocid.Files.Instances` namespace, by simply calling the constructor like below:

```csharp
var entry = new FileSystemEntry(pathToFile);
```

{% hint style="info" %}
You don't need to neutralize the path before making a new instance of this class; it'll neutralize your path and you can access both the original and the neutralized paths.
{% endhint %}

### Extension handling

The Nitrocid filesystem provides you with an option to open the files deterministically without opening the hex editor on binary files using the extension handlers. Currently, only the interactive file manager (IFM) uses this feature to open any file.

Handling extensions consists of a class, called `ExtensionHandler`, that contains information about how to open the file ending in a specific extension, and has the following properties:

* `Extension`
  * File extension that is being handled
* `MimeType`
  * MIME type of the extension
* `Implementer`
  * The extension handler implementer name (can also be used as a codename for the handler)
* `Handler`
  * A function to execute with the full path to the requested file as the first argument
* `InfoHandler`
  * A function get information from a file and render said information to a string instance

You can register your custom handler using the `RegisterHandler()` function found in the `ExtensionHandlerTools` class. This way, next time IFM opens up a file with an extension that contains your handler, it will execute your own custom handler.

```csharp
ExtensionHandlerTools.RegisterHandler(".txt", "txthandler", (path) => Opening.OpenEditor(path, true), (path) => $"Lines: {File.ReadAllLines(path).Length}");
```

Additionally, any mod that uses `OpenDeterministically()` on a file will trigger your custom handlers.

{% hint style="warning" %}
Make sure that you unregister all your handlers when unloading your mod. `RegisterHandler()` throws if it discovers that your handler that handles your specific extension is already added to the list of handlers.

```csharp
var handler = ExtensionHandlerTools.GetExtensionHandler(".txt", "txthandler");
ExtensionHandlerTools.UnregisterHandler(".txt", handler);
```
{% endhint %}

For the list of default handlers, they get saved to the `ExtensionHandlers.json` file. These handlers get used everytime a request to `GetExtensionHandler(string extension)` function is made.

### Filesystem read and write

You can read from a file or write to a file using the Nitrocid API to get its contents or write your own content to a file. The file paths are always neutralized to your current working directory, unless you specify a rooted path (absolute path).

The following writing APIs are available:

* `WriteContents()`
  * Useful if you have an array of text
* `WriteContentsText()`
  * Useful if you have a plain text
* `WriteAllLinesNoBlock()`
  * Useful if you have an array of text and want a non-blocking write
* `WriteAllTextNoBlock()`
  * Useful if you have a plain text and want a non-blocking write
* `WriteAllBytes()`
  * Useful for writing binary files

The following reading APIs are available:

* `ReadContents()`
  * Useful if you have a text file and want to parse it line by line
* `ReadContentsText()`
  * Useful if you have a text file and want to parse it as a whole string
* `ReadAllLinesNoBlock()`
  * Useful if you have a text file and want to parse it line by line with a non-blocking read
* `ReadAllTextNoBlock()`
  * Useful if you have a text file and want to parse it as a whole string with a non-blocking read
* `ReadAllBytes()`
  * Useful for reading binary files

### File decode and encode

You can decode and encode files using the below commands:

* `decodefile <file> [algorithm]`
* `encodefile <file> [algorithm]`

Encoded files using the default encoding algorithm are saved to `<file>.encoded`, and the decoded files are saved to the original file name without the `.encoded` prefix.

### File content printing

When you print file contents to the console or render it as a string for preview, you may need to use one of the `FileContentPrinter` functions. However, it depends on your goal:

* If you want to print the file content to the console deterministically, use the `Display*()` functions.
* If you just want to synthesize a string of how it would appear as previewed for different purposes, like previewing a file in an informational box, use the `Render*()` functions.

### MIME Metadata

Additionally, the `Extensions` part of the filesystem feature contains a mechanism that allows you to manipulate with the MIME metadata.

* You can get the MIME type from an extension using the `GetMimeType()` function.

{% hint style="info" %}
The supported MIME types are available [here](https://github.com/Aptivi/NitrocidKS/commit/c4a52ae47ebc870adb72d11b6a2a641db95684d7#diff-4591e343dac43550e94f94b4cfe0f51e2189301ea58219ef5f3efc05caab0cc7R31).
{% endhint %}

### Symbolic Links (a.k.a. Shortcuts)

Symbolic links are shortcuts to a single file or directory that simplify the path to the target file or folder. They are useful for many purposes.

To make a symbolic link, you can use the `MakeSymlink()` function and its `Try` counterpart from the `Making` class from the `Operations` namespace. This function takes two arguments:

* The link name specifies a path to the symbolic link. This name must not exist.
* The link target specifies a path to an existent file or folder.

### Custom paths

The `Nitrocid.Files.Paths` namespace contains a group of functions that allow you to use the pre-defined kernel paths and custom paths. The pre-defined kernel paths can be get by either getting a value from its associated property, such as `AddonsPath`, or by using the `GetKernelPath()` function.

```csharp
public static string GetKernelPath(KernelPathType PathType)
```

The custom paths can be registered and unregistered by using the functions that are outlined below:

```csharp
public static void RegisterKernelPath(string pathType, string path)
public static void UnregisterKernelPath(string pathType)
```

If you want to define your own custom path, you must register the kernel path with a file or folder that you choose. It's not necessarily a file or a folder that exists, since you may create it in your mod. After the registration, you can use the `GetKernelPath()` function to give it a name of your registered path type.

```csharp
public static string GetKernelPath(string PathType)
```

### Copying and Moving

The base filesystem driver implements three copying and moving modes:

*   Copying and moving a source file to a destination

    ```csharp
    // Copying
    public static void CopyFile(string Source, string Destination)
    public static bool TryCopyFile(string Source, string Destination)

    // Moving
    public static void MoveFile(string Source, string Destination)
    public static bool TryMoveFile(string Source, string Destination)
    ```
*   Copying and moving a source directory to a destination

    ```csharp
    // Copying
    public static void CopyDirectory(string Source, string Destination)
    public static void CopyDirectory(string Source, string Destination, bool ShowProgress)
    public static bool TryCopyDirectory(string Source, string Destination)
    public static bool TryCopyDirectory(string Source, string Destination, bool ShowProgress)

    // Moving
    public static void MoveDirectory(string Source, string Destination)
    public static void MoveDirectory(string Source, string Destination, bool ShowProgress)
    public static bool TryMoveDirectory(string Source, string Destination)
    public static bool TryMoveDirectory(string Source, string Destination, bool ShowProgress)
    ```
*   Copying and moving a source file or directory to a destination

    ```csharp
    // Copying
    public static void CopyFileOrDir(string Source, string Destination)
    public static bool TryCopyFileOrDir(string Source, string Destination)

    // Moving
    public static void MoveFileOrDir(string Source, string Destination)
    public static bool TryMoveFileOrDir(string Source, string Destination)
    ```

When copying or moving a file or directory, the functions first check for the source to check to see if it's a file, a directory, or both, depending on the function used. For example, if you're copying a directory to a destination, you can use either `CopyFileOrDir()` or `CopyDirectory()`, but not `CopyFile()`.

The `TryCopy*()` and the `TryMove*()` functions return `true` if copying or moving is successful, and false if the operation failed. The normal `Copy*()` and the `Move*()` functions throw an exception if any failure occurs.

### File and Folder Selector

<figure><img src="../../../.gitbook/assets/131-inner.png" alt=""><figcaption></figcaption></figure>

The file manager includes two separate TUIs that allow you to select a file or a folder conveniently. We'll explain what do the two selectors do and how they work.

#### File Selector

When it comes to file selection, there is a function dedicated to opening the file selector effortlessly, called `SelectFile()`, which you can find its two signatures below:

{% code title="Selection.cs" lineNumbers="true" %}
```csharp
public static string SelectFile()
public static string SelectFile(string path)
```
{% endcode %}

When the selector is open, you can do almost all the things, just like in the normal file management TUI, but with some of the features removed, such as copying and moving to the secondary pane.

To select a file, you can press `Enter` to select it. Then, you'll have to exit it manually to save the selected file, thus making `SelectedFile` return a selected file.

#### Files selector

Like the file selector, but it also allows you to select more than one file to allow your mod to perform various operations on these files, such as bulk copying the selected files to a single target directory.

There is a function that allows you to use this kind of selector, called `SelectFiles()`.

{% code title="Selection.cs" lineNumbers="true" %}
```csharp
public static string[] SelectFiles()
public static string[] SelectFiles(string path)
```
{% endcode %}

To select a file, press `Enter` to select it. Once you're done selecting various files, you'll have to exit it manually to save the selected files, thus making `SelectedFiles` return a list of selected files.

#### Folder Selector

When the folder selector is open using the `SelectFolder()` function, you can select a folder effortlessly by pressing the spacebar on the target directory highlighted.

{% code title="Selection.cs" lineNumbers="true" %}
```csharp
public static string SelectFolder()
public static string SelectFolder(string path)
```
{% endcode %}

After that, you can press `Esc` to save the selected folder and pass it to the application, which it can access the selected folder value using the `SelectedFolder` property.

#### Folders selector

Like the folder selector, but it also allows you to select more than one folder to allow your mod to perform various operations on these folders, such as bulk copying the selected folders to a single target directory.

There is a function that allows you to use this kind of selector, called `SelectFolders()`.

{% code title="Selection.cs" lineNumbers="true" %}
```csharp
public static string[] SelectFolders()
public static string[] SelectFolders(string path)
```
{% endcode %}

To select a file, press `Space` to select it. Once you're done selecting various folders, you'll have to exit it manually to save the selected folders, thus making `SelectedFolders` return a list of selected folders.
