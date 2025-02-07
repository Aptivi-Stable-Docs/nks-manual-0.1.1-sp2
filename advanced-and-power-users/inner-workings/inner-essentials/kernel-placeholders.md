---
description: Do you want to <placeholder>?
icon: billboard
---

# Kernel Placeholders

<figure><img src="../../../.gitbook/assets/115-inner.png" alt=""><figcaption></figcaption></figure>

Kernel placeholders are text variables for any piece of text that are replaced by different elements found within the kernel. They are parsed in several different areas of the kernel, the most famous example being the MOTD and the MAL messages.

The probing takes place in the `PlaceParse.ProbePlaces()` function found within the `Nitrocid.Misc.Probers.Placeholder` namespace. The following types of text use this function to parse the placeholders:

* MOTD and MAL messages
* Username prompt and their derivatives for each shell
* Password prompt and their derivatives for each shell
* Mail progress style
* Mail progress style (single)
* `echo` command
* Manual page information style
* GPG prompt style
* Custom welcome banner
* Remote debugger message format
* RSS feed URL prompt style
* Manual page contents
* Download and upload progress for network transfers
* Progress Clock screensaver's informational text variables

These are the placeholders and what possible values are going to replace them when being parsed:

| Placeholder                       | Value                                                                              |
| --------------------------------- | ---------------------------------------------------------------------------------- |
| `<user>`                          | Current logged-in username                                                         |
| `<ftpuser>`                       | Current logged-in FTP username                                                     |
| `<ftpaddr>`                       | Current logged-in FTP server address                                               |
| `<currentftpdirectory>`           | Current FTP remote directory                                                       |
| `<currentftplocaldirectory>`      | Current FTP local directory                                                        |
| `<currentftplocaldirectoryname>`  | Current FTP local directory name                                                   |
| `<sftpuser>`                      | Current logged-in SFTP username                                                    |
| `<sftpaddr>`                      | Current logged-in SFTP server address                                              |
| `<currentsftpdirectory>`          | Current SFTP remote directory                                                      |
| `<currentsftplocaldirectory>`     | Current SFTP local directory                                                       |
| `<currentsftplocaldirectoryname>` | Current SFTP local directory name                                                  |
| `<mailuser>`                      | Current logged-in mail address                                                     |
| `<mailaddr>`                      | Current logged-in mail server                                                      |
| `<currentmaildirectory>`          | Current mail directory                                                             |
| `<host>`                          | Host name of the kernel                                                            |
| `<currentdirectory>`              | Current directory                                                                  |
| `<currentdirectoryname>`          | Current directory name                                                             |
| `<shortdate>`                     | Short date                                                                         |
| `<longdate>`                      | Long date                                                                          |
| `<shorttime>`                     | Short time                                                                         |
| `<longtime>`                      | Long time                                                                          |
| `<date>`                          | Today's date                                                                       |
| `<time>`                          | The time right now                                                                 |
| `<timezone>`                      | Local timezone                                                                     |
| `<summertimezone>`                | Summer local timezone                                                              |
| `<system>`                        | Operating system                                                                   |
| `<newline>`                       | New line                                                                           |
| `<dollar>`                        | User administrative indicator                                                      |
| `<randomfile>`                    | Random file path                                                                   |
| `<randomfolder>`                  | Random directory path                                                              |
| `<fgreset>`                       | Reset the foreground color                                                         |
| `<bgreset>`                       | Reset the background color                                                         |
| `<f:color>`                       | Sets the foreground color (where `color` is either `0-255` or `0-255;0-255;0-255`) |
| `<b:color>`                       | Sets the background color (where `color` is either `0-255` or `0-255;0-255;0-255`) |
| `<$var>`                          | Uses the value of a UESH variable (where `var` is an initialized variable)         |
| `<uptime>`                        | Shows the kernel uptime                                                            |
| `<rid>`                           | Gets the specific RID for your operating system from .NET                          |
| `<ridgeneric>`                    | Gets the non-specific RID for your operating system                                |
| `<termemu>`                       | Terminal emulator used (empty on Windows)                                          |
| `<termtype>`                      | Terminal type used (empty on Windows)                                              |

## Custom placeholders

You can make your own placeholders, too, with a simple one-line function that allows you to create your custom placeholders for commands and text that use the placeholders feature, with the dynamic content represented in text that you want.

However, you need to register a placeholder before you can use it. Luckily, the registration is easy, unlike all the other components:

{% code title="Somewhere in your mod code" lineNumbers="true" %}
```csharp
PlaceParse.RegisterCustomPlaceholder("placeholder", (_) => "MyDynamicText");
```
{% endcode %}

{% hint style="info" %}
Additionally, you can pass the first argument to the function that takes a string argument so that your placeholder behaves according to the argument provided. For example, colors are defined like this:

```csharp
PlaceParse.RegisterCustomPlaceholder("colorfgtrue", (c) => new Color(c).VTSequenceForegroundTrueColor);
```
{% endhint %}

You can verify that your placeholder is registered by calling the below function:

{% code title="Verification" lineNumbers="true" %}
```csharp
bool regged = PlaceParse.IsPlaceholderRegistered("<placeholder>");
// regged should be true
```
{% endcode %}

If you no longer want a custom placeholder, you can remove it using the `UnregisterCustomPlaceholder()` function using the same placeholder name.

{% hint style="warning" %}
Please note that you need to surround your placeholder name with the `<` and the `>` marks, except for `RegisterCustomPlaceholder()`, so that the prober can recognize your placeholder. It throws an exception if it's not surrounded.
{% endhint %}
