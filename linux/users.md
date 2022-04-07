# User Account Management:
## Table of Contents:
* [Intro](#intro)
* [Creating User Accounts](#creating-user-accounts)
	+ [`useradd`](#useradd)
	+ [User Defaults](#user-defaults)
	+ [`usermod`](#usermod)
	+ [`userdel`](#userdel)
* [Group Accounts](#group-accounts)
	+ [User Group Accounts](#user-group-accounts)
	+ [Creating Group Accounts](#creating-group-accounts)
	+ [Locked out of `sudoers` and Fixed](#locked-out-of-sudoers-and-fixed)
* [Managing Users in Complex Systems](#managing-users-in-complex-systems)
	+ [Setting Permissions with Access Control Lists (ACL)](#setting-permissions-with-access-control-lists-(acl))
	+ [Setting ACLs with `setfacl`](#setting-acls-with-setfacl)
	+ [Setting Default ACLs](#setting-default-acls)
	+ [Enabling ACLs](#enabling-acls)
	+ [Adding Directories for Users to Collaborate](#adding-directories-for-users-to-collaborate)
	+ [Creating Restricted Deletion Directories with Sticky Bits](#creating-restricted-deletion-directories-with-sticky-bits)

## Intro:
- This chapter is about managing users in Linux. It does that in 3 main sections:
	- First, we tackle user accounts: how to add, delete and change information about single users.
	- Second, we treat user group accounts and see how we can create settings for a multiple users and what they can do by simply changing their group setting and by adding them or removing them from groups.
	- Third, we look at more advanced ways of managing users such as ACLs and setting collaborative directories with set GIT bit and sticky bit.

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

### Locked out of `sudoers` and Fixed:
- ***I have stupidly managed to accidentally lock myself out of the sudoers files. I don't know what exactly caused this. Was it `usermod -G` or `usermod -g`.***
- To fix such a problem I had to boot the system in recovery mode by pressing the shift key during the booting process. Some screen showing an option for *recovery mode* appears. I click on root and, voilà, I'm logged in as root. I check the sudoers file and don't see my actual username, but see another one. I had deleted that user. Instead of modifying the sudoers file in this recovery mode I thought it'd be wiser to just add that user which is in the sudoers file. Before that I had to make the filesystem also writable because it's read-only in recovery mode. Making the filesystem writable is done with the following command:
```sh
mount -o rw,remount /
```
- The question now is: how do I secure my system so that only an authorized user can actually log in in recovery mode and fix problems like this!!?

## Managing Users in Complex Systems:
- The user/group model is good in simple setups with few users. In more complex settings, this model becomes restrictive and suffers from such shortcomings as:
	- Only one user and one group can be assigned to a file/directory. 
	- A regular user's ability to set permissions on different files/directories is limited.
- We sometimes need more users to share a file and regular users to have more freedom to set permissions on files and directories. We will do this using **access control lists (ACLs)** and *shared directories* (**sticky bit** and **set GID bit** directories).

### Setting Permissions with Access Control Lists (ACL):
- Basically, ACLs allow regular users to share their files and directories with certain other users and groups. A regular user can allow select users/groups to read/write/execute files without such files having open permissions.
- The following general rules apply to ACLs:
	- To use ACLs on a filesystem, you must enable them when that filesystem is mounted.
	- In Ubuntu, Fedora and others, ACLs are enabled by default on the filesystem created during installation of the system.
	- If you want ACLs enabled on a filesystem you create/add after installation, you must set the acl mount option when that filesystem is mounted.
	- ACLs are added to a file using the **`setfacl`** commands. Viewing ACLs on a file is done with **`getfacl`**.
	- Only the owner of a file can set ACLs on it. If a user is assigned user/group permissions using `setfacl`, that user cannot change ACLs on that file.
	- Because multiple groups/users can be assigned to a file, the permissions the user has on the file is the union of permissions assigned to the user and all the groups he belongs to. If I belong to sales which has read permission, and marketing which has write permission and development which has execute permission, the I have their permissions combined so I can read/write/execute the file.

### Setting ACLs with `setfacl`:
- You set ACL permissions on a file using `setfacl` along with the **`-m`** option as in:
```sh
setfacl -m u:username:rwx filename
```
- You can remove these options using the **`-x`** option:
```sh
setfacl -x u:username filename
```
- When you run `ls -l` and see a file has a plus in its permissions as in **`rw-rw-r--+`**, this means ACLs are set on it and you can use `getfacl` to see what ACLs it has. Let's look at an example of what `getfacl` prints out:
```
# file: someFile
# owner: am
# group: am
user::rw-
user:fafa:rwx
group::rw-
mask::rwx
other::r--
```
- **`mask`** in the example above is the maximum permissions on the file including ACL and regular users and groups. In Fedora this mask cannot exceed the regular group permissions. This *might be true for Ubuntu as well.*

### Setting Default ACLs:
- You can set default ACL permissions on a directory so all files and directories created in that directory inherit those ACL permissions. In the following example, we set default ACLs on directory `/business/john` using **`d:`** and **`g:`** is for group sales. Each file or directory added to this directory have the same ACLs for sales (`rwx`):
```sh
setfacl -m d:g:sales:rwx /business/john
```
- Files/directories you create within this file might differently mainly due to the masking that dictates they cannot exceed the file's group permissions. Their **effective** permissions are union of the mask and the default ACL permissions.

### Enabling ACLs:
- I don't understand what's going on here, so I'll just skip this for now!

### Adding Directories for Users to Collaborate:
- A fourth set of bits is usually ignored when using `chmod`. These bits can be used by `chmod` to set special permissions. These are the so-called **set user ID bit**, **set group ID bit**, and the **sticky bit**.
- The following table shows how these bits and their values work with `chmod`

| Name | Numeric value | Letter value |
| --- | --- | --- |
| Set user ID bit | **`4`** | **`u+s`** |
| Set group ID bit | **`2`** | **`g+s`** |
| Sticky bit | **`1`** | **`o+t`** |

- This section is about creating directories for collaboration, so we will just focus on the set group ID bit and sticky bit.
- By creating a GID directory, any file created in that directory is automatically shared with/assigned to users of that directory's group. Let's say we want to make `mabi3at` directory a collaborative file among sales users. We start by using a new chmod that includes a 4th number or its equivalent letter representation:
```sh
chmod 2775 mabi3at
```
- We assign the mabi3at directory to sales group, so users belonging to the sales group have permissions `775` on any file created in this directory. When a user belonging to the sales group creates a file in this directory, the group of the file is automatically sales, not the primary group of the user. I see how this can be more convenient than ACLs.
- Directories with the set GID bit have an **`s`** in their group permissions bit, which we can see by running ls -l:
```sh
drwxrwsr-x  2 am   sales 4096 Apr  7 20:43 mabi3at
```

### Creating Restricted Deletion Directories with Sticky Bits:
- By setting the sticky bit on a directory, you effectively prevent a user from deleting files that belong to other users. Imagine we have a collaborative directory where everyone can read everyone else's files, and can also add files to that directory. We cannot do this any other way. A user with the permission to write to a directory can create but also delete files in that directory. How can we get a user to only create files in a directory and delete her own but not delete other's files. This is the job of sticky bits which we can set using letter values as follows:
```sh
chmod o+t mabi3at
```
- Directories with the set sticky bit have a **`t`** in their permissions as in **`drwxrwsr-t`**.
- By the way, only the root user and the owner of the directory can delete files from this directory.

