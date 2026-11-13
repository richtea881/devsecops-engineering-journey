# **Day 01 - Linux Permissions and Ownership**

**Programme:** Independent DevSecOps Engineering Journey**Track:** Linux Administration**Week:** 01 - Linux Fundamentals***Status:** Practical exercises completed — documentation updated; GitHub publication pending

## **1. Learning objectives**

By the end of this lesson, I should be able to:

- Explain Linux file ownership.
- Identify file and directory permissions.
- Understand read, write and execute permissions.
- Interpret the output of `ls -l`.
- Change permissions using `chmod`.
- Change ownership using `chown`.
- Change group ownership using `chgrp`.
- Apply the principle of least privilege.

## **2. Understanding Linux ownership**

Every Linux file and directory has:

1. An owner (user).
2. A group.
3. Permissions assigned to three categories.

The three permission categories are:

|**Category**|**Meaning**|
|---|---|
|User (u)|The file owner|
|Group (g)|Members of the assigned group|
|Others (o)|Everyone else|

## **3. Understanding permissions**

Linux uses three basic permissions:

|**Permission**|**Symbol**|**Numeric value**|
|---|---|---|
|Read|r|4|
|Write|w|2|
|Execute|x|1|
|No permission|-|0|

### **Numeric permissions**

Permissions can be represented by adding the numeric values.

|**Permission**|**Calculation**|**Result**|
|---|---|---|
|Read only|4|4|
|Write only|2|2|
|Execute only|1|1|
|Read and write|4 + 2|6|
|Read and execute|4 + 1|5|
|Read, write and execute|4 + 2 + 1|7|

For example:

`chmod 755 script.sh`

Means:

- Owner: read, write, execute (7).
- Group: read, execute (5).
- Others: read, execute (5).

`chmod 644 document.txt`

Means:

- Owner: read and write (6).
- Group: read only (4).
- Others: read only (4).

## **4. Reading Linux permissions**

Command:

```bash
ls -l
```

Example output:

```text
-rwxr-xr-- 1 kali kali 1200 Nov 12 script.sh
```

Interpretation:

- `-` — regular file.
- `rwx` — owner permissions.
- `r-x` — group permissions.
- `r--` — others’ permissions.
- First `kali` — owner.
- Second `kali` — group.

Other file-type indicators include:

- `d` — directory.
- `l` — symbolic link.

## **5. Important commands**

### **Inspect permissions and ownership**

```bash
ls -l filename
stat filename
```

### **Change permissions**

```bash
chmod 644 filename
chmod 755 filename
chmod u+x filename
chmod g-w filename
chmod o-r filename
```

### **Change file ownership**

```bash
sudo chown username filename
```

### **Change group ownership**

```bash
sudo chgrp groupname filename
```

### **Check current user and groups**

```bash
whoami
id
groups
```

## **6. Practical Laboratory — Completed**

**Environment:** Kali Linux 2026 running in VMware Fusion**Working directory:** `~/day01-permissions-lab`

### **Exercise 1 — Create a test environment**

Commands executed:

```bash
mkdir -p ~/day01-permissions-lab
cd ~/day01-permissions-lab
touch report.txt script.sh
```

Verified file creation using `ls -l`.

Initial permissions: `664` for both files.

### **Exercise 2 — Inspect file permissions and metadata**

Commands executed:

```bash
stat report.txt
ls -l report.txt
```

Inspected file ownership, permissions, size and timestamps.

### **Exercise 3 — Change permissions numerically**

```bash
chmod 640 report.txt
ls -l report.txt
```

Result: `-rw-r-----`

- Owner: read and write.
- Group: read only.
- Others: no access.

### **Exercise 4 — Change permissions symbolically**

Commands executed:

```bash
chmod u+x script.sh
chmod g-w report.txt
ls -l
```

Practised adding execute permission to the owner and removing group write permission.

### **Exercise 5 — Inspect and change ownership**

Commands executed:

```bash
whoami
id
sudo chown kali:kali report.txt
sudo chown :kali report.txt
```

Verified:

- File owner: `kali`
- Group owner: `kali`
- Permission bits remained unchanged by the ownership commands.

### **Exercise 6 — Apply the principle of least privilege**

Commands executed:

```bash
chmod 600 report.txt
chmod 750 script.sh
```

Final verification:

```bash
stat -c '%n | Owner: %U | Group: %G | Permissions: %a' report.txt script.sh
```

Actual output:

```text
report.txt | Owner: kali | Group: kali | Permissions: 600
script.sh | Owner: kali | Group: kali | Permissions: 750
```

**Final permissions:**

|**File**|**Permission**|**Access**|
|---|---|---|
|`report.txt`|600|Owner: read/write; group and others: no access|
|`script.sh`|750|Owner: read/write/execute; group: read/execute; others: no access|

### **Practical outcome**

All six practical exercises were completed and verified successfully. The lab demonstrated file metadata inspection, numeric and symbolic permissions, ownership management, group ownership management and least-privilege implementation.
## **7. Security relevance**

Incorrect file permissions can expose confidential information, allow unauthorised modifications, or permit unauthorised execution.

As a DevSecOps practitioner, I should:

- Apply least privilege.
- Avoid unnecessarily permissive `777` permissions.
- Restrict sensitive configuration files.
- Review ownership and group membership.
- Verify permissions before deploying applications.
- Understand that file permissions are one layer of Linux security, not the entire security model.

## **8. Evidence and Screenshots**

**Evidence folder:**

`02 - DevSecOps Engineering Journey/02 - Evidence & Screenshots/Week 01 - Linux Fundamentals/Day 01 - Permissions and Ownership/`

### **Evidence captured**

- ☑ Final `ls -l` output showing `report.txt` with `600` and `script.sh` with `750`.

### **Earlier practical evidence**

The following exercises were completed and verified through terminal outputs, but screenshots were not captured:

- ☐ Screenshot 1 — Initial file permissions using `ls -l`.
- ☐ Screenshot 2 — File metadata using `stat`.
- ☐ Screenshot 3 — Numeric permissions using `chmod 640`.
- ☐ Screenshot 4 — Symbolic permission changes.
- ☐ Screenshot 5 — Ownership and group ownership verification.

**Final evidence filename:**

`day01-05-final-least-privilege-permissions.png`

- **Filename:** `day01-05-final-least-privilege-permissions.png`
- **Storage location in Obsidian vault:**

`01 - DevSecOps Engineering Journey/02 - Evidence & Screenshots/Week 01 - Linux Fundamentals/Day 01 - Permissions and Ownership/`

- **Evidence shown:** Final `ls -l` output confirming `report.txt` has permissions `600` and `script.sh` has permissions `750`.


**Evidence status:** Final practical screenshot captured. Earlier stages have recorded command outputs but no screenshots.

## **9. Knowledge Assessment**

**Assessment result:** 5/5 — 100%

The Day 01 knowledge assessment was completed successfully.

### **Topics assessed**

1. Interpreting numeric Linux permissions.
2. Understanding file ownership and permission management.
3. Understanding directory read, write and execute permissions.
4. Distinguishing between `chmod` and `chown`.
5. Understanding the security implications of directory permissions.

### **Additional revision**

Completed targeted revision exercises covering:

- Directory read (`r`) permissions.
- Directory write (`w`) permissions.
- Directory execute (`x`) permissions.
- Permission combinations such as `300`, `500` and `770`.
- The relationship between directory permissions and file access.

**Assessment outcome:** All five graded questions answered correctly. Additional directory-permission revision completed successfully.

## **10. Completion Record**

**Completion date:** 13 November 2026**Practical environment:** Kali Linux 2026 running in VMware Fusion on macOS Sequoia 15.7.8**Lab directory:** `~/day01-permissions-lab`

**Final evidence screenshot**

Evidence captured: Final least-privilege screenshot saved (see Section 8).

|**Component**|**Status**|
|---|---|
|Theory|Completed|
|Practical exercises|Completed and verified|
|Knowledge assessment|Passed — 100%|
|Evidence captured|Final screenshot captured|
|Documentation|Updated|
|GitHub publication|Pending|

### **Lesson outcome**

Successfully completed the Linux Permissions and Ownership practical laboratory.

Demonstrated the ability to inspect file metadata, interpret Linux permissions, modify permissions using numeric and symbolic methods, manage file and group ownership, and apply the principle of least privilege.

### **Outstanding tasks**

- ☐ Review the complete lesson documentation.
- ☐ Confirm evidence screenshot is saved in the correct folder.
- ☐ Publish the updated Day 01 documentation to GitHub.
- ☐ Update the DevSecOps Engineering Dashboard.

**Overall status:** Practical learning and assessment completed. GitHub publication and final project administration pending.
