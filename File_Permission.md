# Linux File Permissions & Ownership





# 1. Overview of Linux Security & Authorization

Linux is a multi-user operating system derived from Unix. As Linux is used in mainframes,servers, and cloud infrastructure, multiple users access the system concurrently. To prevent unauthorized users from corrupting, altering, or stealing data, Linux divides authorization into two key levels:

- **Ownership:** Assigns specific file/directory controls to users and groups.
- **Permissions:** Defines exact actions (Read, Write, Execute) permitted for each user category.

---

# 2. Ownership Levels in Linux

Every file and directory in Linux has three distinct ownership assignments:

| Category | Symbol | Description |
|----------|--------|-------------|
| User (Owner) | u | The user who created or currently owns the file. Possesses primary permission rights. |
| Group | g | A collection of multiple users. All members in the group share identical group permissions. |
| Other | o | Any system user who does not own the file and does not belong to the assigned group. |

## Critical Ownership Rule

Two groups cannot own the same file. A file/directory is owned by exactly one user and one primary group.

---

# 3. Linux Permission System

Linux assigns permissions across three basic operations:

| Permission | Symbol | File Effect | Directory Effect |
|------------|--------|-------------|------------------|
| Read | r | View or open file contents. | List contents (e.g., `ls`). |
| Write | w | Modify, edit, or delete file contents. | Create, delete, or rename files inside. |
| Execute | x | Run the file as a program/script. | Enter/traverse directory (e.g., `cd`). |

When running `ls -l`, permissions appear as a 10-character string (e.g., `-rwxr-xr--`):

- **1st char:** File type (`-` = file, `d` = directory)
- **2nd–4th chars:** User/Owner rights (`rwx`)
- **5th–7th chars:** Group rights (`r-x`)
- **8th–10th chars:** Other rights (`r--`)

---

# 4. Modifying Permissions: The `chmod` Command

The `chmod` (Change Mode) command alters permissions via two modes:

- Absolute Mode
- Symbolic Mode

## 4.1 Absolute (Numeric / Octal) Mode

Permissions are set using a 3-digit octal number representing:

`[User][Group][Other]`

Digits are calculated by adding active permission octal values:

| Digit | Active Permission | Symbolic Equivalent | Calculated Value |
|------:|-------------------|---------------------|------------------|
| 0 | No Permission | --- | 0 |
| 1 | Execute | --x | 1 |
| 2 | Write | -w- | 2 |
| 3 | Write + Execute | -wx | 2 + 1 = 3 |
| 4 | Read | r-- | 4 |
| 5 | Read + Execute | r-x | 4 + 1 = 5 |
| 6 | Read + Write | rw- | 4 + 2 = 6 |
| 7 | Read + Write + Execute | rwx | 4 + 2 + 1 = 7 |

### Absolute Mode Command Examples

```bash
# Set permissions: Owner (rwx=7), Group (r-x=5), Other (r-x=5)
chmod 755 document.txt

# Set permissions: Owner (rw-=6), Group (r--=4), Other (r--=4)
chmod 644 document.txt

# Set permissions: Owner (rwx=7), Group (---=0), Other (---=0)
chmod 700 document.txt
```

## 4.2 Symbolic Mode

Modifies permissions selectively using target symbols and operators.

**Targets:** `u` (User), `g` (Group), `o` (Other), `a` (All: u+g+o)

**Operators:**

- `+` Add
- `-` Remove
- `=` Set explicitly

### Symbolic Mode Command Examples

```bash
# Change permissions of 'test' file for Other to 'rwx'
chmod o=rwx test

# Add execution permission (+) for Group on 'test' file
chmod g+x test

# Remove read permission (-) for Owner/User on 'test' file
chmod u-r test

# Add execute permission for All users (user, group, other)
chmod a+x script.sh
```

---

# 5. Changing Ownership & Group: `chown` & `chgrp`

## 5.1 The `chown` Command

Changes user owner or both user owner and group simultaneously.

**Syntax**

```bash
chown user <filename>
```

or

```bash
chown user:group <filename>
```

### Examples

```bash
# Change file owner to 'root' for file 'commands'
sudo chown root commands

# Change file owner to 'guru99' AND group to 'guru99' for file 'commands'
sudo chown guru99:guru99 commands

# Recursively change owner and group for a directory tree
sudo chown -R john:developers /var/www/html
```

## 5.2 The `chgrp` Command

Changes only group ownership of a file or directory.

**Syntax**

```bash
chgrp group <filename>
```

### Examples

```bash
# Change group owner to 'root' for file 'commands'
sudo chgrp root commands

# Change group owner to 'devteam' for file 'project.py'
sudo chgrp devteam project.py
```

---

# 6. Group Management Utilities & Configuration

| File / Command | Description & Usage |
|----------------|---------------------|
| `/etc/group` | System file containing definitions and user lists for all groups defined on the system. |
| `groups` | Command used to display groups you (or a specified user) belong to: `groups [username]` |
| `newgrp` | Command used to work as a member of a new group other than default group: `newgrp cdrom` |

---

# Summary Quick Reference

**Numeric Reference**

- `7 = rwx`
- `6 = rw-`
- `5 = r-x`
- `4 = r--`
- `0 = ---`

**Commands**

```bash
chmod [mode] [file]
chown [user]:[group] [file]
chgrp [group] [file]
```