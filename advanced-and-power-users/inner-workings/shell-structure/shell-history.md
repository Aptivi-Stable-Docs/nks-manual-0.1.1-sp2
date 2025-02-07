---
description: What have you run last time?
icon: hourglass-end
---

# Shell History

The shell history is another UESH shell feature that allows you to recall the last commands that you've executed in the current and in the previous sessions. This is accompanied by a file that shows you a history of commands executed in all the Nitrocid sessions.

That file is called `ShellHistories.json` that has this format: (Note that this varies based on your input)

```json
{
  "General": [
    "2"
  ],
  "AdminShell": [],
  "DebugShell": [
    "currentbt",
    "exit"
  ],
  "HexShell": [],
  "Shell": [
    "debugshell",
  (...)
}
```

Basically, this JSON file contains keys and values that have the following format:

* The keys hold the shell type name
* The values hold the input history associated with the target shell type

Accessing the shell history is easy; just press the up arrow button on your keyboard to recall the previous command executed. You can also press the down arrow button to recall the next command in the whole history.
