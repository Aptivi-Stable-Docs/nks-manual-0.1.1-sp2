---
description: Make your own custom boot style!
icon: boot-heeled
---

# Custom Boot Styles

When Nitrocid reads the configuration file, the bootloader parses the boot style name and attempts to load that style. Your mods can load custom boot styles by consulting functions in the `BootStyleManager` class that allows you to add your own custom boot style to the bootloader.

## Implementing your style

To implement your custom boot style, these steps must be followed:

1. Assuming that you have your mod's source code, create a class and name it after your boot style that you want.
2. Write next to the class file `: BaseBootStyle, IBootStyle` and import the required namespace by `using Nitrocid.Kernel.Starting.Bootloader.Style;`.
3. Override the necessary methods using the `override` keyword for each one as appropriate.
   * `public override Dictionary<ConsoleKeyInfo, Action<BootAppInfo>> CustomKeys { get; }`
   * `public override string Render()`
   * `public override string RenderHighlight(int chosenBootEntry)`
   * `public override string RenderModalDialog(string content)`
   * `public override string RenderBootingMessage(string chosenBootName)`
4. Now, implement everything as you wish. Once you're done, add necessary functions in the mod initialization phase to add your own custom boot style.
5. Click on the Build menu and select Build Solution.
6. Repeat the steps to install your mod as highlighted in [this page](../kernel-modifications/#building).

## Key handling

Your custom boot style can make use of the custom key bindings to do custom actions. They can be defined by overriding the `CustomKeys` dictionary with a new dictionary containing the console key and the action with the boot app info passed to it as an argument, like below:

```csharp
public override Dictionary<ConsoleKeyInfo, Action<BootAppInfo>> CustomKeys { get; } = new()
{
    { new ConsoleKeyInfo(...), (bai) => ... }
}
```

If any key other than the `UP ARROW`, `DOWN ARROW`, and `ENTER`, the key handler attempts to parse the key according to the custom key list defined by your boot style. If the key pressed is the same as one of the defined keys, the handler executes the action with the chosen boot application, whose type is `BootAppInfo`, as the argument passed to it.

## Bootloader state

You can also access the bootloader state by referencing the `BootloaderState` class. This class provides you with the following options:

* `WaitingForBootKey`
* `WaitingForFirstBootKey`

This can be used for your custom boot styles to change their behavior, depending on the state.
