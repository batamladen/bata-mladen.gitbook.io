# Managing Users & Groups

## Managing Users

In Linux, user properties encompass various attributes and settings associated with a user account. These properties define the user's identity, permissions, and environment within the system. Here are the key user properties you can manage:

* Username
* Password
* UserID (UID)
* GroupID (GID)
* Comment
  * Additional information
  * Commonly used for full name
* Home directory (\~)
* Default shell

### User Data Files

Data is stored in `/etc/passwd`.\
Commonly there is `x` where the password is.\
Password hashes are stored in `/etc/shadow`.

### User Management

{% tabs %}
{% tab title="Create User" %}
```bash
useradd $options $username

```

**Options**:\
• <mark style="color:orange;">**-p**</mark>, --pasword \
&#x20;   ◦ NOT RECOMMENDED\
• <mark style="color:orange;">**-u**</mark>, --uid \
• <mark style="color:orange;">**-g**</mark>, --gid \
• <mark style="color:orange;">**-s**</mark>, --shell \
• <mark style="color:orange;">**-d**</mark>, --home-dir \
• <mark style="color:orange;">**-c**</mark>, --comment \
If option isn’t specified the defaults are used
{% endtab %}

{% tab title="Delete User" %}
```bash
userdel $username
```


{% endtab %}

{% tab title="Edit User" %}
```bash
usermod $options $username
```

**Options**:\
• <mark style="color:orange;">**-l**</mark>, --login\
&#x20;   ◦ Change username \
• <mark style="color:orange;">**-p**</mark>, --pasword \
&#x20;   ◦ NOT RECOMMENDED \
• <mark style="color:orange;">**-u**</mark>, --uid \
• <mark style="color:orange;">**-g**</mark>, --gid \
• <mark style="color:orange;">**-s**</mark>, --shell \
• <mark style="color:orange;">**-d**</mark>, --home-dir \
• <mark style="color:orange;">**-c**</mark>, --comment\
If option isn’t specified then it is unchanged

***

```bash
passwd $user
```

Changes user password interactively.\
Recommended
{% endtab %}

{% tab title="More" %}
View user information

```bash
id $username
```

View all user details from /etc/passwd

```bash
getent passwd $username
```

View password expiration details

```bash
sudo chage -l $username
```
{% endtab %}
{% endtabs %}

***

## Managing Groups

In Linux, group properties define the characteristics and settings associated with a group. Groups are used to manage sets of users and control their permissions and access rights. Here are the key properties of group:

* Group name
* Password&#x20;
  * Generally not used&#x20;
* GroupID (GID)
* Members&#x20;

### Group Data Files

Stored in `/etc/group`

### Group Management

{% tabs %}
{% tab title="Create Group" %}
```bash
groupadd $options $groupname
```

**Options**:\
• <mark style="color:orange;">**-g**</mark>, --gid \
• <mark style="color:orange;">**-p**</mark>, --password \
If option isn’t specified the defaults are used
{% endtab %}

{% tab title="Delete Group" %}
```bash
groupdel $groupname
```


{% endtab %}

{% tab title="Edit Group" %}
```bash
groupmod $options $groupname
```

**Options**:\
• <mark style="color:orange;">**-n**</mark>, --new-name \
• <mark style="color:orange;">**-g**</mark>, --gid \
• <mark style="color:orange;">**-p**</mark>, --password \
If option isn’t specified then it is unchanged
{% endtab %}
{% endtabs %}

***

## Users Group

To edit a user's group, witch is defaultyly built with the user, just add -G flag to `useradd/usermod` commands

You can use -aG flag to add group to a user.
