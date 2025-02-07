---
description: Your checkbook
icon: clipboard-list-check
---

# To-do List

<figure><img src="../../../../.gitbook/assets/038-todo.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Nitrocid KS provides this feature as an addon.
{% endhint %}

This program serves as your checkbook to track your tasks. They help you organize your everyday tasks and plans, like plans for the next release of your project, grocery lists, and so on. They also help keep track of things that you need or plan to do.

## Basics

Nitrocid provides you with a single command, `todo`, that allows you to do the following:

* `todo add <taskname>`
  * Adds a task to the task list
* `todo remove <taskname>`
  * Removes a task from the task list
* `todo done <taskname>`
  * Marks a selected task as done
* `todo undone <taskname>`
  * Marks a selected task as not done yet
* `todo list`
  * Lists all tasks and their status
* `todo save`
  * Saves the task list to a JSON file, `ToDoList.json`
* `todo load`
  * Loads the task list from the above JSON file
