## **1. Users in Linux**

### **Definition**

- A **user** is an account that allows access to the system.
- It provides **security limits**, determining what the user can and cannot do.

### **User Types**

1. **Regular User**
    - Has standard privileges.
    - Cannot perform administrative tasks unless added to `sudo`.
2. **System User**
    - Created for system processes (e.g., `www-data`, `sshd`).
    - Usually **cannot login**.
3. **Superuser (root)**
    - UID = 0.
    - Has **full privileges** on the system.
    - Root **does not need sudo**.

### **User Properties**

- **Username** → identifier
- **UID (User ID)** → unique numeric ID
- **Primary group** → determines default permissions on new files
- **Additional groups** → give access to shared resources
- **Home directory** → personal folder
- **Shell** → default command interpreter

---

### **Key Commands**

```bash
# Check if a user exists
id username
getent passwd username
cat /etc/passwd | grep username

# Create a user
sudo adduser username

```

---

### **Example – `/etc/passwd` Line**

```
kali:x:1000:1000:Kali User:/home/kali:/bin/bash

```

| Field | Meaning |
| --- | --- |
| kali | Username |
| x | Password placeholder (actual password in /etc/shadow) |
| 1000 | UID |
| 1000 | GID (primary group) |
| Kali User | Comment / Full name |
| /home/kali | Home directory |
| /bin/bash | Default shell |

---

## **2. Groups in Linux**

### **Definition**

- A **group** aggregates multiple users for easier permission management.
- Each user has a **primary group** and can belong to **additional groups**.

### **Key Points**

- **Primary group** → controls ownership of newly created files.
- **Additional groups** → grant access to shared files/resources.
- **Groups do not have passwords** in modern Linux systems (the `x` field is a placeholder).

### **Commands**

```bash
# Check a user's groups
groups username

# Create a new group
sudo groupadd groupname

# Add user to a group
sudo usermod -aG groupname username

# Remove user from a group
sudo gpasswd -d username groupname
sudo deluser username groupname

```

### **Example – `/etc/group` Line**

```
finance:x:1001:kali,bob

```

| Field | Meaning |
| --- | --- |
| finance | Group name |
| x | Password placeholder (unused) |
| 1001 | GID |
| kali,bob | Members of the group |

---

## **3. UID and GID**

| Term | Meaning |
| --- | --- |
| UID (User ID) | Unique identifier for each user. UID=0 → root, UID≥1000 → regular users |
| GID (Group ID) | Unique identifier for each group. Determines primary group of a user |

**Key Points:**

- UID and GID are used internally by Linux to manage **ownership and permissions**.
- Changing GID/UID can affect file ownership; always be careful.

---

## **4. File Permissions**

### **Standard Permission Format**

```
-rwxrwxrwx

```

- **First character**: file type ( regular, `d` directory, `l` link)
- **Next 9 characters** → permissions:
    - **r** = read
    - **w** = write
    - **x** = execute
- Divided into **user | group | others**

### **Example**

```
-rw-r-----  alice developers file.txt

```

| Field | Access |
| --- | --- |
| Owner (alice) | read + write |
| Group (developers) | read only |
| Others | no access |

---

### **RHCSA Key Points**

- Always check ownership before changing permissions.
- `chmod`, `chown`, and `chgrp` are your main tools.

---

## **5. Sudo and Privileges**

### **Regular Users**

- Cannot execute administrative commands by default.
- Must be **added to `sudo` group**:

```bash
sudo usermod -aG sudo username

```

- Use `sudo` before a command to run it as root.

### **Root User**

- Already has **all privileges**.
- Does not require `sudo`.

---

## **6. Quick Reference Commands**

| Action | Command |
| --- | --- |
| Check if user exists | `id username` / `getent passwd username` / `grep username /etc/passwd` |
| Create a user | `sudo adduser username` |
| Add user to group | `sudo usermod -aG groupname username` |
| Remove user from group | `sudo gpasswd -d username groupname` / `sudo deluser username groupname` |
| Create a group | `sudo groupadd groupname` |
| Check user groups | `groups username` |

---

## File Permissions

```
-rw-------
```

<img width="483" height="193" alt="image" src="https://github.com/user-attachments/assets/f496c558-9376-4a2d-8b05-9cb2c6b06764" />

| Symbol | Meaning |
| --- | --- |
| `-` | Regular file | Ordinary file |
| `r` | Read | Can view file contents or list directory contents |
| `w` | Write | Can modify file or create/delete files in a directory |
| `x` | Execute | Can run a file as a program or enter a directory |
| `-` | None | Permission is denied |

| Permission | Symbol | Purpose |
| --- | --- | --- |
| `d` | Directory | Folder|
| setuid | `s` | File executes with **owner’s privileges** |
| setgid | `s` | File executes with **group’s privileges** or directory inherits group |
| sticky bit | `t` | Only owner can **delete/rename files** in directory |

----

## Linux File Example: Understanding `ls -l` Output

### 1️⃣ List Files
```bash
ls -l
```

2️⃣ Create a New File
```bash
touch file1
```

Listing after creation:
```
-rw-rw-r-- 1 root root 0 Sep 22 09:26 file1
```

### 3️⃣ Breaking Down the Output

**Permissions and Type:** `-rw-rw-r--`

- → **regular file**
- `rw-` → **owner** (`root`) can read & write
- `rw-` → **group** (`root`) can read & write
- `r--` → **others** can read only

**Hard Links:** `1`

- Number of links pointing to this file

**Owner and Group:** `root root`

- First `root` → file owner
- Second `root` → group owner

**Size:** `0`

- File size in **bytes** (empty file)

**Last Modified:** `Sep 22 09:26`

- Date and time of last modification

**Filename:** `file1`

---


