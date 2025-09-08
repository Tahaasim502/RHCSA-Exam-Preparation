# User Management Commands

```bash
# Create a user
sudo adduser username

# Check if a user exists
id username
getent passwd username
cat /etc/passwd | grep username

# Add user to a group
sudo usermod -aG groupname username

# Remove user from a group
sudo gpasswd -d username groupname
sudo deluser username groupname

**remember when using root you dont need to use sudo**
```
---

# Group Management Commands

```bash
# Create a new group
sudo groupadd groupname

# Check a user's groups
groups username
```
---

# File Permissions

```bash
# Example permission check
ls -l file.txt
-rw-r----- alice developers file.txt
```

---

