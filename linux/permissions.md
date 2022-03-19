# Permissions

# Contents:

### Users
- Add User = sudo adduser username
- Change User password = sudo passwd username
- Delete User = sudo userdel username
- Edit Users Configuration File = sudo vim /etc/passwd (shows usernames, names of users, home directories)

### Groups
- Sudo groupadd groupname
- Sudo groupdel groupname
- Sudo vim /etc/group (shows groups and users)

### Permissions
- Numbers = owner/group/everyone else
- 4 = read, 2 = write, 1 = execute
- To Chanege Permissions of a File or Folder = sudo chmod 777 file/folder (-R for recursive)

### Changing Ownership:
- To Change User Ownership = sudo chown -R username file/folder
- To Change Group Ownership =sudo chgrp --R groupname file/folder
- R for Recursive for Folders