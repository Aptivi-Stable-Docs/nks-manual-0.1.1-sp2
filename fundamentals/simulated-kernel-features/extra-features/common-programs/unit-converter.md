---
description: Calculate your mathematical expressions and convert units
icon: scale-balanced
---

# Unit Converter

{% hint style="info" %}
Nitrocid KS provides this feature as an addon.
{% endhint %}

Unit conversion is a transformation of a given number from the source unit, such as Celsius or Moles, to the target unit, such as Kelvin or Kilo-Moles. The unit converter application helps you convert between two units easily and quickly without the need of manually calculating the difference between the two units.

Some units can be converted easily, such as from 1 kilometer to 1000 meters for geometric measurement, such as length, and some of them can be complicated, such as from 1 atmosphere to 101325 Pascals for pressure. This is why the unit converter helps simplify the calculation for you.

## CLI version

<figure><img src="../../../../.gitbook/assets/Beta3-035-UnitConv.png" alt=""><figcaption></figcaption></figure>

The unit converter is used to easily convert the source units to different units, like centimeters to meters. It's powered by the UnitsNet library. You can consult the list of units UnitsNet supports here:

{% @github-files/github-code-block url="https://github.com/angularsen/UnitsNet/tree/master/UnitsNet/GeneratedCode/Units" %}

To use this program, refer to the command usages and individual explanations below.

* `unitconv [-tui] <unittype> <quantity> <sourceunit> <targetunit>`
  * Converts the unit by quantity and type from the source unit to the target unit

<figure><img src="../../../../.gitbook/assets/039-unitconv.png" alt=""><figcaption></figcaption></figure>

You can use the `listunits` command to get all the available units by type.

## TUI version

<figure><img src="../../../../.gitbook/assets/040-unitconvtui.png" alt=""><figcaption></figcaption></figure>

The TUI version of unit converter allows you to more interactively convert the units. It lets you choose a unit type and a conversion form. To choose a unit type, press the `Up Arrow` and the `Down Arrow` keys to change the type. Press `TAB` to switch between selecting the unit type and the conversion form.

Once you're done selecting your source and your target unit, you can press `F1` to bring up the conversion dialog so that you can enter a number.

This dialog prompts you for a number (using the source unit) to convert it to the same representation in the target unit. Once you press `ENTER`, the result will show up.

You can run this version of the unit converter by running `unitconv -tui`.
