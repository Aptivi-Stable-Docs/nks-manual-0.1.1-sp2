---
description: What are the lockscreen widgets?
icon: calendar-range
---

# Lockscreen Widgets

Lockscreen widgets is a modern lockscreen feature that gives you quick insights about different information, including the world clock and the news, in a full-screen format. These widgets can be found in the modern logon screen introduced in the 0.1.0 version of Nitrocid. In the lock screen, you can easily switch between the main screen and the widgets (up to 2 widgets).

{% hint style="warning" %}
Lockscreen widgets are not supported in the classic lock screen style. For custom lock screens, you'll need to handle the widget display yourself.
{% endhint %}

## Setting widgets

The `WidgetTools` class provides a plethora of functions that allow you to manipulate with the widgets, such as registering them and unregistering them. You can also get a widget instance and its name.

In addition to that, the kernel configuration contains the following basic settings that you can use to set your widgets in your choice:

* `EnableWidgets`: Enables or disables widgets in all the supported lock screens
* `FirstWidget`: Specifies the first widget to use
* `SecondWidget`: Specifies the second widget to use

## Configuring widgets

You can configure the widgets using the settings application by executing the `settings` command, passing it the `-widgets` switch. Select a widget using the up/down arrow keys, then use the tab key to change the focus to the widget settings entry list. You can use the enter key afterwards to customize the widgets.

## Building widgets

In order to be able to build your widget, you must make a kernel mod that will register your widget when it starts (`AddWidget()`) and will unregister it when your mod stops (`RemoveWidget()`). Follow the instructions on how to build your mod [here](../../advanced-and-power-users/kernel-modifications/your-mod.md) and use it as a starting point to be able to build such a mod.

Afterwards, make a new class file somewhere on your mod source that will hold all your widget code, such as `MyWidget`, and make it implement both the `BaseWidget` class and the `IWidget` interface so that Nitrocid recognizes it as a widget. A minimal example for this class can be consulted below:

{% code title="MyWidget.cs" lineNumbers="true" %}
```csharp
using Nitrocid.Users.Login.Widgets;

namespace MyMod
{
    internal class MyWidget : BaseWidget, IWidget
    {
        public override string Cleanup(int left, int top, int width, int height)
        {
            // Your cleanup code here
        }
        
        public override string Initialize(int left, int top, int width, int height)
        {
            // Your initialization code here
        }
        
        public override string Render(int left, int top, int width, int height)
        {
            // Your widget rendering code here
        }
    }
}
```
{% endcode %}

{% hint style="info" %}
The strings that the three functions return will be printed raw to the console by Nitrocid using the available Terminaux functions, so make sure that you use the `Render` variants of the writers. In case of things, such as steps, image numbers, or changing colors, you can make use of the class-wide global settings and initialize their initial values by putting the assignments to the Initialize function. For example, [here](https://github.com/Aptivi/NitrocidKS/blob/main/public/Nitrocid/Users/Login/Widgets/Implementations/AnalogClock.cs) is the implementation of the analog clock widget. You can also omit the initialization and the clean-up code by making them return an empty string.
{% endhint %}
