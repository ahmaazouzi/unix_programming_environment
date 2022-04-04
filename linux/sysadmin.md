# What Linux System Administration Is all about:
## Table of Contents:
* [Intro](#intro)
* [System Administration](#system-administration)
	+ [Root](#root)
	+ [What the Admin Can Do](#what-the-admin-can-do)
* [Root](#root)
	+ [Using **`su`** and Becoming Root in Ubuntu](#using-**su**-and-becoming-root-in-ubuntu)
	+ [Granting Administrative Access](#granting-administrative-access)
* [Administrative Commands, Configuration Files, and Log Files](#administrative-commands-configuration-files-and-log-files)
	+ [Administrative Commands](#administrative-commands)
	+ [Administrative Configuration Files](#administrative-configuration-files)
	+ [Administrative Log Files](#administrative-log-files)

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

## Administrative Commands, Configuration Files, and Log Files:
- Administering a Linux system is done with the help of a bunch of commands, configuration files and log files that are available in a typical Linux in certain places regardless of the distribution. Let's have a quick look att these and how to use them effectively.

### Administrative Commands:
- Two important locations where administrative commands are stored include:
	- **`/sbin`** which includes commands needed to boot the system such as commands for checking the filesystem.
	- **`/usr/sbin`** contains commands for such tasks  as managing user accounts or processes that have open files. Processes that run as daemons are also found here.
- Some administrative commands are in `/bin` and `/usr/bin` because they are meant to be used by both regular users and administrators. A good example is `mount` which a regular user can run to see/list mounted filesystems, but an admin can actually use it to mount a filesystem. 
- Names of administrative commands can be found in **`/usr/share/man/man8`**. 
- It's recommended to add one's own administrative commands to **`usr/local/bin`** or **`/usr/local/sbin`** and add these folders to the path if they are not already there. These commands can overrides commands with the same names in other directories.

### Administrative Configuration Files:
- Administrative tasks and configurations are best done using **configuration files** which are basic text files containing commands and information on how services and other things should work in a system. 
- Configuration files might use well-known formats such as XML but not always. Changing a configuration file can mess things up and you don't know if your changes are correct. However, some software offer utilities for checking if changing configuration files was done correctly.
- Most configuration files are placed in **`$HOME`** and **`/etc`** directories. Home configuration files are obviously there for controlling a particular user's part of the system, while `/etc/` files have a systemwide effect. Home configuration file names usually start with a dot **`.`** (hence the name *dot files*). Dot files include files like `.bashrc`. 
- Some important systemwide subdirectories in `/etc` where configuration files are found include:

| Subdirectory | Configuration files |
| --- | --- |
| **`/etc`** | Most basic Linux conf files. |
| **`/etc/cron*`** | Directories containing `crond` setups for hourly, daily, monthly, etc. jobs. |
| **`/etc/default`** | Default values for several utilities. |
| **`/etc/apache2`** | Files for controlling apache2 web server behavior. |

- There are many other configuration file locations like the ones listed above.
- Examples of configuration files found in the `/etc` include:

| Conf file | What it does |
| --- | --- |
| **`bash.bashrc`** | Systemwide bash conf. |
| **`crontab`** | Contains some automated tasks associated with `cron` as well as some variables like `cron`'s shell and its PATH. |
| **`group`** | Has group names and ids found in the system. |
| **`gshadow`** | Has group passwords for groups |
| **`hostname`** | Contains the hostname of the current Linux system. |
| **`hosts`** | Has IP addresses and host names reachable from the current system. Mostly for systems in a LAN or small private  network. |
| **`hosts.allow`** | Lists computers allowed to use some TCP/IP services from the current system. |
| **`hosts.deny`** | Lists computers that are NOT allowed to use some TCP/IP services from the current system. |
| **`mtab`** | Lists currently mounted filesystems |
| **`named.conf`** | Contains DNS settings if using one's own DNS server (with `bind` package). |
| **`passwd`** | Contains information on valid users in the local system. |
| **`profile`** | Contains systemwide environment for all users. File is read when user logs in. |
| **`protocols`** | Sets protocol names and numbers from several Internet services. |
| **`rpc`** | Defines remote procedure call names and numbers. |
| **`services`** | Defines TCP and UDP service names and their ports. |
| **`shadow`** | Contains encrypted passwords from users in the `passwd` file. |
| **`shells`** | Lists shells available in the system and their locations. |
| **`sudoers`** | Sets who can and who cannot run certain commands using the `sudo` command. |
| **`rsyslog.conf`** | Defines log messages gathered by `rsyslogd` and which files they are stored in. Log messages are typically stored in **`/var/log`** |

### Administrative Log Files:
- Logging gives the admin information about errors and fails and also allows him to track suspicious activity. 
- There are different ways of logging system events such as `rsyslogd` and `syslogd`, but `systemd`systems (whatever that is) are probably the norm today and they gather display log messages using something called **`journalctl`**. By running the **`journalctl`** command you can get logs about the system in different forms. The command has several options and combinations thereof that can be learnt more about in man pages.
- **`rsyslogd`** gathers and places logs in the **`/var/log`** directory. 
- We might some of this stuff again in chapters concerned with server administration. 








