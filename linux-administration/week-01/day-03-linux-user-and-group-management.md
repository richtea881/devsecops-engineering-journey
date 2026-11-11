Day 3 – Linux User and Group Management

1. Overview

This practical session focused on Linux user account administration, group management, account verification, and basic access control.

The objective was to understand how Linux identifies users, manages group memberships, and protects user accounts and home directories.

2. Learning Objectives

* Understand Linux UID and GID.
* Identify primary and supplementary groups.
* Create and manage Linux user accounts.
* Understand /etc/passwd and /etc/shadow.
* Assign users to supplementary groups.
* Verify account ownership and directory permissions.
* Understand secure user offboarding and the principle of least privilege.

3. Key Concepts

UID – User Identifier

A UID is a unique numerical identifier assigned to a Linux user account.

GID – Group Identifier

A GID identifies a Linux group. Users can have a primary group and additional supplementary groups.

Primary Group

The primary group is the default group associated with a user’s processes and file creation.

Supplementary Groups

Supplementary groups provide additional access to shared resources without changing a user’s primary group.

User Private Group (UPG)

Linux commonly creates a group with the same name as a new user. This is known as a User Private Group.

4. Important Linux Account Files

File	Purpose
/etc/passwd	Stores basic user account information
/etc/shadow	Stores password hashes and password-aging information
/etc/group	Stores group information and supplementary membership details

The x in the password field of /etc/passwd indicates that password information is stored separately, normally in /etc/shadow.

5. Practical Lab

Step 1: Verify the existing DevOps group

Command:

getent group devops-lab

Observed output:

devops-lab:x:1024:devopsuser

This confirmed that the existing devops-lab group was available.

Step 2: Create a test user

Command:

sudo useradd -m day3test

The command created a new user account and its home directory.

Step 3: Set the user’s password

Command:

sudo passwd day3test

A password was successfully assigned. Password characters were not displayed during entry, as expected in Linux.

Step 4: Verify user identity

Command:

id day3test

Observed output:

uid=1026(day3test) gid=1026(day3test) groups=1026(day3test)

This confirmed that the account had UID 1026 and primary GID 1026.

Step 5: Add the user to the DevOps group

Command:

sudo usermod -aG devops-lab day3test

The -aG options appended devops-lab as a supplementary group without replacing existing supplementary group memberships.

Step 6: Verify group membership

Command:

id day3test

Observed output:

uid=1026(day3test) gid=1026(day3test) groups=1026(day3test), 1024(devops-lab)

This confirmed successful supplementary group membership.

Step 7: Verify home directory ownership and permissions

Command:

ls -ld /home/day3test

Observed output:

drwx------ 5 day3test day3test 4096 Nov 11 22:28 /home/day3test

The directory was owned by day3test, with permissions set to 700.

Numerical verification:

stat -c '%U %G %a %n' /home/day3test

Expected and observed result:

day3test day3test 700 /home/day3test

Permission 700 means the owner has read, write and execute permissions, while group members and other users have no permissions.

Step 8: Inspect the user account record

Command:

getent passwd day3test

Observed output:

day3test:x:1026:1026;;/home/day3test:/bin/sh

Field interpretation:

1. Username: day3test
2. Password placeholder: x
3. UID: 1026
4. Primary GID: 1026
5. GECOS/user information: Empty
6. Home directory: /home/day3test
7. Login shell: /bin/sh

6. Security Lessons

Principle of Least Privilege

Users should receive only the access required to perform their responsibilities.

Safe Group Management

The usermod -aG command is important because it appends supplementary group membership rather than replacing existing memberships.

Home Directory Protection

Permissions of 700 provide private access to the account owner.

Secure Account Offboarding

When an employee leaves an organisation:

* Disable or revoke account access.
* Review supplementary and privileged group memberships.
* Review SSH keys, active sessions and credentials.
* Preserve required files and audit records.
* Delete the account only after authorisation and retention requirements are addressed.

7. Assessment

Final theory assessment: 5/5 (100%)

Topics assessed:

* Purpose of /etc/shadow.
* Safe supplementary group management.
* Meaning of UID.
* Secure employee offboarding.
* Purpose of useradd -m.

8. Practical Outcome

Successfully created and verified the day3test Linux account, assigned its password, added it to the existing devops-lab group, and verified its identity, account record, home directory and permissions.

9. Reflection

This exercise strengthened my understanding of Linux identity and access management. I learned how user accounts relate to groups and how administrators can provide controlled access to shared resources.

These skills are foundational to Linux administration, DevOps engineering, infrastructure management and system security.

⸻

Learning. Building. Automating. Securing.
