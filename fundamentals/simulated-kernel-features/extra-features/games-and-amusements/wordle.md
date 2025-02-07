---
description: Master your words!
icon: square-w
---

# Wordle

<figure><img src="../../../../.gitbook/assets/016-wordle.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
As of 0.1.0, this feature has been moved to the kernel addons.
{% endhint %}

You can start the game with the `wordle` command. It gives you 8 tries to guess the right word based on the amount of characters within the random word. The game does the following:

* If the written character has no match in the entire word, the box is not highlighted by any color.
* Any written character after a wrong attempt will be highlighted as orange to give you the hint of what the word should be.
* Any written character that start with the right character will be highlighted as green to give you an indication that you're progressing well on the current try.

If you guessed all the characters right, you win the game. If you guessed the word wrong after 8 tries, the game is over.

## Controls

This game contains the following controls to help you play:

* `Any character`: Place a character
* `Escape`: Quits the game
