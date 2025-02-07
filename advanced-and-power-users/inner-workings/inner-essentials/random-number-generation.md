---
description: Your random number generator is here! Let's see if you're lucky.
icon: slot-machine
---

# Random Number Generation

The random number generator is a module that allows you to get a random number from the smallest possible number to the largest possible number. It's available for the majority of the computers, such as your desktop computer, your tablet, and so on.

Random number generation involves a sequence of numbers that can't be reasonly predicted by a human in which a random number in the sequence is selected. For example, a dice is an example of a mechanical hardware RNG. You can read more about random number generation [here](https://en.m.wikipedia.org/wiki/Random\_number\_generation).

Nitrocid KS has a random number generator driver that allows you to generate various random numbers or choices based on your purpose, such as getting a random integer number, a random boolean (true or false), and so on. This is wrapped with a kernel driver handler, which you can read about it below:

{% content-ref url="kernel-drivers/" %}
[kernel-drivers](kernel-drivers/)
{% endcontent-ref %}

## Random Numbers

When it comes to RNGs, the most essential task for such operation is to generate a random integer number within a sequence. You can easily access such operations using the `RandomDriver` class, which wraps around the current random number generator driver that you can set up.

You have various types of random number generation classes, such as:

* `Random()`: Gets a random number from 0 to the maximum integer value, inclusive.
* `Random(max)`: Gets a random number from 0 to the specified number, inclusive.
* `Random(min, max)`: Gets a random number from the minimum number, inclusive, to the specified maximum number, inclusive.
* `RandomShort()`: Gets a random number from 0 to the maximum short integer value, inclusive.
* `RandomShort(max)`: Gets a random number from 0 to the specified number, inclusive.
* `RandomShort(min, max)`: Gets a random number from the minimum number, inclusive, to the specified maximum number, inclusive.
* `RandomIdx()`: Gets a random number from 0 to the maximum integer value, exclusive.
* `RandomIdx(max)`: Gets a random number from 0 to the specified number, exclusive.
* `RandomIdx(min, max)`: Gets a random number from the minimum number, inclusive, to the specified maximum number, exclusive.
* `RandomDouble()`: Gets a random floating-point number from 0 to a value that is less than 1.0.
* `RandomDouble(max)`: Gets a random floating-point number from 0 to the specified number, exclusive.

## Random Boolean Decisions

When it comes to random boolean decisions, you have various types to generate a random boolean decision:

* `RandomChance(prob)`: Gets a random boolean answer from the probability by a raw value from 0.0 (0%) to 1.0 (100%)
* `RandomChance(percent)`: Gets a random boolean answer from the probability by a percentage value from 0% (0.0) to 100% (1.0)
* `RandomRussianRoulette()`: Gets a random boolean value based on a Russian Roulette luck factor.
* `RandomBoolean()`: Gets a random boolean value based on a normal random number generation.
