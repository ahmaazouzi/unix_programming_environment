# User Account Management:
## Table of Contents:
## Intro:
- This chapter is 

## Creating User Accounts:
### `useradd`:
- Managing user accounts through CLI have multiple advantages over doing the same thing in a GUI. GUIs are nottt always available, especially in the case of servers. CLI also allows you to  automate user account management so you can add , remove, mod users in bulk and automatically if needed. 
- The **`useradd`** command which requires root privileges adds a new user to the system. It has one required parameter, the username, and the rest is optional:
```sh
sudo useradd somebody1
```
- `useradd` also takes a variety of options which include:

| Option | Description |
| --- | --- |
| **`-c "Mustapha K3ik3a"`** | Used to provide a description of the accountt. Used mostly for user full name. |
| **`-d "somewhre/user123"`** | Provides a custom home directory. Instead of `/home/somebody1`, the home directory is what you specify as an argument to this option. |
| **`-D`** | When invoked by itself, it displays default values for all newly created accounts. If invoked with other options, it changes such default values. |
| **`-e 2222-12-22`** | Expiry date of the account. |
| **`-f -1`** | Set number of days after password expiry until account is permanently deleted. The default is `-1` which probably means the system will use some default value stored in some `/etc/default/useradd` file. If you set this to `0` the account is disabled as soon as the password expires. Setting this t `2` disables the account 2 days after the expiry of the password. |
| **`-g somegroup`** | Sets the an existing group as the primary group of the new user. If not set a new group with the user's name is created. |
| **`-G wheel,sales,tech`** | Add the user to a list of additional groups |
| **`-k skel_dir`** | Sets/copies an initial directory skeleton to the new user's home directory containing initial configuration files. If not set, **`/etc/skel/`**. This must be used with the `-m` option |
| **`-m`** | Automatically creates a home directory for the new user and optionally copies `/etc/skel` to it. This is the default in Fedora but not in Ubuntu. |
| **`-M`** | Do not create a new user home directory even if the default states so! |
| **`-p passwd`** | Allows you to enter a password for the account. |
| **`-s /bin/bash`** | Specifies the default shell of the new account |
| **`-u user_id`** | Specifies a user id (UID) instead of having it generated automatically. |

- If you haven't provided a password to the new user at the time of account creation, you can do so with the following command:
```sh
sudo passwd somebody1
```
- When you run `useradd` a lot of things happen which have to do with a lot of configuration files we have seen earlier:
	1. Reads configuration files `/etc/logins.defs` and `/etc/useradd` to get default values for creating new user accounts. 
	2. Run through command options and arguments to find which default values to override.
	3. Create a new user entry in `/etc/passwd` (users) and `/etc/shadow` (passwords) based on default values and provided parameters.
	4. Might create new group entries in `/etc/group`.
	5. Creates user home directory, by default in `/home`.
	6. Copies some skeleton to the user's home directory which is `/etc/skel` if no alternative is provided.
- Ubuntu actually offers an easier way to add a new user. It is an interactive and more user-friendly and spares you the need to enter a lengthy blob of options. It is **`adduser`**.
- ***I have also begun to realize a few things about all the configuration business and how it works!! I used to hate having to reconfigure stuff, but I kinda started liking it. I seems to me like I can just add user by simply modifying some of configuration files like `/etc/passwd` although I'm aware that is not the recommended.***

### User Defaults:
- `usermod` can also be used to inspect and modify user creation defaults. This is done by the very special option we've seen in the table above **`-D`**:
```sh
useradd -D
```
- The command above displays the current `useradd` defaults which are stored in `/etc/default/useradd`. With other options, you can change these default. Let's change the default home directory with the following command:
```sh
useradd -D -b /home/users
```
- This command change the base directory where user home directories are placed from `/home` to `/home/users`. 
- You can also change some other defaults by modifying the `/etc/logins.defs` which also control age of passwords, their minimum lengths and many other things.
- As an admin, you can also create default files that you want added to new user accounts and whatnot.s


### `usermod`:
- In addition to adding a new user you can also modify an existing user. A lot of options for this command mirror those found in `useradd`. I will only list a few of the interesting ones and some not found in `useradd`. One can refer to the man page for more information:

| Option | Description |
| --- | --- |
| **`-c name`** | Change name or description to the new value. |
| **`-L`** | Lock the account by putting an exclamation mark before the encrypted password in `etc/shadow`. This locks the account without affecting the password. |
| **`-U`** | Unlocks an account by removing the the exclamation mark from the beginning of the encrypted password. |

- To change the name of user `sam`, you'd type:
```sh
sudo usermod -c "Ben Amherst" sam
```


### `userdel`:
- **`userdel`** is used to delete a user as in:
```sh
userdel -r john
```
- The option **`-r`** specifies that user `john`'s home directory is removed along with the user. If you don't use `-r` the home directory of the removed user stays around.
- The removed user might have other files lying around and these files belong to that user.
- After deleting a user, you might need to search the system for files left by that user and maybe remove them or do something else with them:
```sh
find / -user john -ls
```
- Files which are not assigned to any user are a security risk (***why?***). To find these, use the following command and then maybe assign them to a real user:
```sh
find -nouser -ls
```

## Group Accounts:
- Groups allow you to share a set of files between multiple users. You assign certain permissions on a file to a group and then add users to such a group. Now all those users have those permissions to these files.

### User Group Accounts:
- When you create a user account, a primary group account is (or must be) assigned to this new account. By default in Fedor and Ubuntu, this primary group has the same name as the user. This primary group is indicated in the third field in the `/etc/passwd` file. This is the group ID (GID).
- In `/etc/passwd` you don't see group names but just GIDs. Groups information is stored in the **`/etc/group`** file where you can see a list of group names and their GIDs in your system.
- When a user creates a file, that files is assigned by default to the user's primary group.
- A user can belong to 0 or more supplementary groups (*in addition to the primary group*). When a group is a supplementary group assigned to a user, that user appears in a list in the `etc/group`:
```sh
sales:x:1234:sam,ban,tam
```
- A regular user cannot add himself to any group and cannot add any other user to his primary group. Only users with root privilege can add users to groups.
- A regular user has group  or other permissions (whichever is higher) to files/directories she belongs to.
- A regular user can change her primary group temporarily to any of her supplementary groups with the **`newgrp`**. She can then create a file belonging to that group and exit back to her actual primary group as in:
```sh
$ newgrp sales
$ touch something
$ exit
```
- The `something` file belongs to the sales group.
- A regular user can also use `newgrp` and change a file's group to a group she doesn't belong to, but she must provide a password to do so.
- A group password is set by a root user with the **`gpasswd`** command as in:
```sh
sudo gpasswd sales
```

### Creating Group Accounts:
- Groups are created automatically when a new user is added. Each group is assigned a **group ID (GID)**. GIDs 0 to 999 are assigned to special administrative groups. Regular groups begin for some systems, including Fedora and Ubuntu at GID 1000.
- We create new groups with the **`groupadd`**:
```sh
sudo groupadd -g 1111 sales
```
- The **`-g`** option indicates a specific GID. Without it, the next available GID is assigned to the group.
- The **`groupmod`** command allows you to modify several attributes from a given group.

## Managing User Accounts in a Complex Environment:
## Centralizing User Accounts:

























































