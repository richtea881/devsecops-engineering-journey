Day 2 — Linux Shared Directories and setgid Group Inheritance

Programme: DevSecOps Engineering Journey
Week: 1 — Linux Administration
Date: 11 November 2026
Status: Completed
Environment: Kali Linux 2026 (VMware)

1. Learning Objectives

* Understand how Linux shared directories work.
* Configure group ownership on a directory.
* Apply the setgid permission to a directory.
* Verify group inheritance on newly created files.
* Understand how Linux permissions support secure team collaboration.

2. Concepts Learned

setgid on directories

The setgid permission on a directory causes newly created files and subdirectories to inherit the directory’s group ownership.

This helps maintain consistent group ownership when multiple users collaborate in a shared directory.

Permission breakdown

The directory was configured with:

sudo chmod 2770 /opt/devops-project

The permission components are:

* 2: setgid bit.
* 7: Owner has read, write and execute permissions.
* 7: Group has read, write and execute permissions.
* 0: Others have no permissions.

Verified directory permissions:

drwxrws--- 2 root devops-lab 4096 Nov 11 09:43 /opt/devops-project

The s in rws confirms that setgid is enabled.

3. Practical Implementation

Step 1 — Create the shared directory

sudo mkdir -p /opt/devops-project

Step 2 — Set directory ownership

sudo chown root:devops-lab /opt/devops-project

Step 3 — Apply setgid and permissions

sudo chmod 2770 /opt/devops-project

Step 4 — Verify the user and group membership

whoami
id

Observed user:

devopsuser

Group membership included:

devops-lab

Step 5 — Create a test file

touch /opt/devops-project/day2-test.txt

Step 6 — Verify ownership and permissions

ls -l /opt/devops-project
stat /opt/devops-project/day2-test.txt

Observed file:

-rw-rw-r-- 1 devopsuser devops-lab 0 Nov 11 09:43 day2-test.txt

The file belonged to devopsuser, while its group was inherited from the shared directory: devops-lab.

The stat command also confirmed:

Access: (0664/-rw-rw-r--)
Uid: (1025/devopsuser)
Gid: (1024/devops-lab)

4. Verification Results

Check	Result
Shared directory created	Passed
Group ownership configured	Passed
setgid permission applied	Passed
User group membership verified	Passed
Test file created	Passed
Group inheritance verified	Passed

5. Assessment Results

Knowledge quiz: 7/8 — 87.5%

Independent exercise: 4/4 — 100%

One correction from the quiz:

Without setgid, a new file normally receives the creator’s effective filesystem group, often their primary group. With setgid on the directory, it inherits the directory’s group.

6. Security and DevSecOps Relevance

Shared-directory group inheritance helps teams maintain consistent access controls.

In DevOps environments, this is useful for:

* Shared deployment directories.
* Team-managed application files.
* Build and release workspaces.
* Controlled collaboration between service accounts and administrators.

The directory’s 2770 permissions prevent users outside the owner and group from accessing it.

Security consideration: The test file had mode 0664, allowing others to read it. Directory access restrictions still prevented users without directory traversal permission from reaching it. File permissions and directory permissions must therefore be considered together.

7. Evidence and Screenshots

Screenshots have not yet been captured for this lesson.

Suggested evidence filenames:

* day02-01-whoami.png
* day02-02-id-group-membership.png
* day02-03-directory-setgid-permissions.png
* day02-04-file-group-inheritance.png
* day02-05-stat-file-metadata.png

8. Lessons Learned

1. Linux permissions control access to files and directories.
2. Group ownership supports controlled collaboration.
3. The setgid bit on directories enforces group inheritance.
4. The stat command provides detailed file metadata.
5. Security requires understanding both file and directory permissions.

9. Next Learning Topic

Day 3 — Linux User Accounts and Account Management

Planned topics include user creation, account properties, password management, group membership and secure privilege management.

⸻

Portfolio note: Practical results were verified in the Kali Linux lab. Screenshots and GitHub publication remain pending.