---
description: Group of users are here
icon: people-group
---

# Groups

Grouping the users is a common task for user management, because the groups not only organize users in a consistent manner, but they also derive permissions and other rules to all the users that are part in a group. For example, if a group has a permission that would allow users inside a group to use administrative commands, all the users will get that permission.

To derive the permissions to some of the users that you've added to a group, you'll have to grant a permission to the group permissions. This is explained in depth in the Permissions page, which you can consult below:

{% content-ref url="permissions.md" %}
[permissions.md](permissions.md)
{% endcontent-ref %}

## Operations

There are common operations that you can do to manage the groups and their permissions.

### Adding groups

<figure><img src="../../../.gitbook/assets/052-addgroup.png" alt=""><figcaption></figcaption></figure>

To add a group to the list of available groups, you can use the `addgroup` command. You need to specify a group name that will organize the user accounts based on the role and what is this group for.

### Removing groups

<figure><img src="../../../.gitbook/assets/053-rmgroup.png" alt=""><figcaption></figcaption></figure>

You can remove a group from the list of available groups if you no longer want a specific group using the `rmgroup` command. You need to specify a group name that exists to be able to remove it.

### Adding users to a group

<figure><img src="../../../.gitbook/assets/054-addusertogroup.png" alt=""><figcaption></figcaption></figure>

You can add a user to an existing group to organize a user by using the `addusertogroup` command so that the user gets permissions from the group. You need to specify an existing user and an existing group that the user didn't join to be able to add a user to the group.

### Removing users from a group

<figure><img src="../../../.gitbook/assets/055-rmuserfromgroup.png" alt=""><figcaption></figcaption></figure>

You can remove an unwanted user from an existing group using the `rmuserfromgroup` command so that the target user is no longer a member of that target group. You need to specify an existing user and an existing group that the user joined to be able to remove a user from the group.

## Permission operations

In order to get an idea of how to manipulate with the group permissions, see the `Permissions` page linked above.
