# What Linux System Administration Is all about:
## Table of Contents:
## Intro:
- We can think of a system administrator as the person who controls and sets limits on how the system is used. Such a person is especially important for Linux systems which inherited from their Unix forefathers the fact of being multi-user where multiple users run multiple programs simultaneously and interact with networks and all. There are dangers and problems that come with all this flexibility and complexity. 
- A system administrator can be done by a user who is logged in as a *superuser* (*root user*). The root user has extra privileges and have total control over the system. It can add and remove users and set what they can do. It is also the only one that can do certain things like installing software and changing some configurations.
- This document will talk in very general terms about system administration and what it's all about. 

## System Administration:
### Root:
- Separating a regular user from an administrator has several benefits because it prevents the regular user who don't know what they are doing from damaging the system.
- To administer the system you need to first log in as a regular user then ask for administrative privileges, which you can do in two ways in a non-graphical Linux:
	1. By running **`sudo -s`** in Ubuntu (**`su`** in Fedora), you open a new shell as root user. You do the work you need to do and then logout to go back to being a regular user. Actually, `su` in Ubunu allows me to login as somebody else from somebody's shell. Let's say `sam` is not a sudoer, and `am` is a sudoer. Sam is logged in and i use his console to login as `am` with the command `su am`.. Now I am `am` and can log in as a root user.
	2. You can also precede your commands with the **`sudo`** command. For example, trying to run **`ls /root`** will fail and tells you have no permission to run it, so you'd use **`sudo ls /root`** instead. 
- In both of the cases above you might be prompted to enter your password. When you enter the password to be a root user that password hangs around for sometime so you might not have to enter again for a short period of time. If you try again to log in as root after while, you will need to enter the password. If you logout of the shell while the password is still around that password will not be around anymore, so you will have to enter again to  gain root privilege. 

### What the Admin Can Do:
- The system administrator can do the following:
	- The admin can modify the **filesystem** by adding extra storage or removing part of it, changing its layout (partitioning and all that jazz), and add, remove, change, copy or modify files belonging to some user. 
	- Admin can **install any and all software anywhere in the system**. Regular users have a limited capability to install certain software in their own directory. They can also list information about installed software.
	- Admin can add, remove and modify information about user accounts and group accounts.
	- Admin have higher privileges in what can be done with **networking** stuff.
	- **Configuring servers** such as file servers, web servers, mail servers can only be done by admins.
	- **`Security features`** such as setting firewalls and user access lists are the job of admins. Admins also oversee the use and prevent the abuse of system resources. 

## Root:
- Every Linux system starts out with at least one root user.
- the home directory of a root user (or a user logged in as root) is **`/root`**, so running **`cd ~`** or **`cd`** places you in `/root`.
- The entry for `root` in `/etc/passwd` is:
```
root:x:0:0:root:/root:/bin/bash
```
- The line above is divided into 7 fields by columns **`:`**. Let's see what these mean:

| Field Value | Meaning |
| --- | --- |
| **`root`** | Username |
| **`x`** | User uses a password which is stored in an encrypted form in **`/etc/shadow`** |
| **`0`** | Root user id |
| **`0`** | Root group id |
| **`root`** | root group name |
| **`/root`** | Root home directory |
| **`/bin/bash`** | Shell for the user |

- By changing this document (as a root user, of course), you can change many things about users like the groups they belong to and their home directories. It's not a good idea to try to edit this file by hand but use **`usermod`** which we will see when discussing user account management in another document.
- Ubuntu has a root user but you cannot log in as root by for example using `su root`. This is for security purposes so you'd use `sudo` with commands and are often aware that you are in a danger zone. Another layer of security is customizing your home as a regular user by say using colors customizing your prompt. These settings might not be available for the root **`sudo -s`** and it's all just white text. You are not meant to be always a root user. 

### Using **`su`** and Becoming Root in Ubuntu:
- By running `su` or `su -` in other systems, you can become a root user. This doesn't work in Ubuntu as you can never log in as root in Ubuntu. `su`, however has uses in Ubunu which allow you to login as a different user. For example:

| Command | Alternative | Description |
| --- | --- | --- |
| **`su otherUser`** | **`sudo runuser otherUser`** | Allows you to login as another user but not quite, because you will still have the current pwd |
| **`su - otherUser`** | **`sudo runuser - otherUser`** | This completely logs you in as another user. |

- I read in the man page for `su` that sudo users can and should run **`runuser`** instead of `su`. This way they can log in without supplying that user's password. 
- From the table above, the **`-`** changes your environment and current working directory, but removing the hyphen doesn't. 
- Being a superuser and logging in as some other regular user allows an admin to run only the commands that user can run. This functionality is used to allow the admin to troubleshoot problems encountered by a particular user. s te

### Granting Administrative Access:
- A very important piece of functionality is the ability to grant other users administrative privileges. 
- To grant a regular user who doesn't have administrative access administrative privileges, we need to make such a user a **`sudoer`**. We need to add this user to the **`/etc/sudoers`**. We need to be logged in as root users (or use `sudo`). We don't edit the `/edit/sudoers` file directly but use the **`visudo`** command as in:
```sh
sudo visudo
```
- The command will open the `/etc/sudoers` file where we add our regular user as in:
```
sam     ALL=(ALL:ALL) ALL
```
- Here we are granting all administrative access to the user sam. It is possible to grant sudo only on certain commands, but I don't know how to do that!!










