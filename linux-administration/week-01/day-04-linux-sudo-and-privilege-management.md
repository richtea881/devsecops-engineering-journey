Day 04 — Linux Sudo and Privilege Management

Programme: Independent DevSecOps Engineering Journey
Week: 01 — Linux Administration
Day: 04
Date: 12 November 2026
Environment: Kali Linux running in VMware Fusion
Status: Practical exercises completed

1. Learning Objectives

The objectives for this session were to:

1. Understand the difference between a normal user and the root account.
2. Understand how sudo provides temporary elevated privileges.
3. Identify users and groups with administrative access.
4. Inspect sudo permissions and configuration files.
5. Understand the security risks of excessive privileges.
6. Apply the principle of least privilege when evaluating access rights.

2. Practical Environment

All practical exercises were performed inside the Kali Linux virtual machine using its terminal.

The account used during the exercise was:

* Username: kali
* UID: 1000
* Primary group: kali
* Primary group ID: 1000

3. Practical Exercises

Exercise 1 — Identify the Current User

Command:

whoami

Observed output:

kali

Explanation:

The whoami command displays the username of the currently active user.

The result confirmed that the terminal session was running under the normal account kali, rather than directly as the root user.

Exercise 2 — Inspect User Identity and Group Membership

Command:

id

Observed output:

uid=1000(kali) gid=1000(kali) groups=1000(kali),4(adm),20(dialout),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),100(users),101(netdev),102(scanner),114(wireshark),119(kaboxer),968(docker),979(bluetooth),999(lpadmin)

Explanation:

The id command displays the user’s UID, primary group, GID and supplementary group memberships.

Important findings:

* kali has UID 1000.
* Its primary group is kali, with GID 1000.
* The account belongs to the sudo group.
* The account also belongs to the docker group.

Security observation:

Membership of the sudo group can grant administrative privileges, depending on the system’s sudoers configuration.

Membership of the docker group can also be highly privileged. On systems using a rootful Docker daemon, Docker access can potentially provide a route to root-level control. Access should therefore be restricted to trusted users.

Exercise 3 — Inspect Sudo Permissions

Command:

sudo -l

Observed output:

Matching Defaults entries for kali on kali-Linux:
    secure_path=/usr/sbin\:/usr/bin\:/sbin\:/bin
Runas and Command-specific defaults for kali:
    Defaults!/usr/sbin/visudo env_keep+="SUDO_EDITOR EDITOR VISUAL"
User kali may run the following commands on kali-Linux:
    (ALL : ALL) ALL

Explanation:

The sudo -l command lists the commands the current user is permitted to execute through sudo.

The rule:

(ALL : ALL) ALL

indicates that the kali account is allowed to execute any command as any user and group, subject to the configured sudo policy.

This is broad administrative access.

Exercise 4 — Demonstrate Temporary Privilege Elevation

Command:

sudo whoami

Observed output:

root

Then:

whoami

Observed output:

kali

Explanation:

The first command ran whoami with root privileges through sudo.

The second command confirmed that the original terminal session remained under the kali account.

This demonstrates that sudo can elevate privileges for an individual command without permanently changing the logged-in user.

Exercise 5 — Identify Members of Administrative Groups

Command:

getent group sudo

Observed output:

sudo:x:27:kali

The output confirms that kali is listed as a member of the sudo group.

Next:

getent group docker

Observed output:

docker:x:968:kali

This confirms that kali is also listed in the docker group.

Security observation:

Administrative and privileged groups should be reviewed regularly. Users should only retain membership when their responsibilities require it.

Exercise 6 — Validate Sudoers Configuration Syntax

Command:

sudo visudo -c

Observed output:

/etc/sudoers: parsed OK
/etc/sudoers.d/kali-grant-root: parsed OK
/etc/sudoers.d/ospd-openvas: parsed OK

Explanation:

The visudo -c command checks the syntax of sudoers configuration files.

All three files were successfully parsed.

Important distinction:

A successful syntax check does not automatically mean that the permissions are secure or appropriately restricted. It confirms that the configuration can be parsed.

Exercise 7 — Inspect Additional Sudoers Rules

Command:

sudo grep -nEv '^[[:space:]]*(#|$)' /etc/sudoers.d/kali-grant-root /etc/sudoers.d/ospd-openvas

Observed output:

/etc/sudoers.d/kali-grant-root:3:%kali-trusted   ALL=(ALL:ALL) NOPASSWD: ALL
/etc/sudoers.d/ospd-openvas:1:_gvm ALL = NOPASSWD: /usr/sbin/openvas

Explanation:

This command searches the specified files and displays non-comment, non-empty configuration lines.

The options mean:

* -n: Display line numbers.
* -E: Use extended regular expressions.
* -v: Invert the match.
* The expression excludes blank lines and comment lines.

Security findings:

The first rule:

%kali-trusted ALL=(ALL:ALL) NOPASSWD: ALL

Allows members of the kali-trusted group to execute all commands as any user and group without entering a sudo password.

This is a high-risk, unrestricted passwordless sudo permission.

The second rule:

_gvm ALL = NOPASSWD: /usr/sbin/openvas

Allows the _gvm service account to execute the specified OpenVAS command without a password.

This is more narrowly scoped because it specifies a particular command rather than granting unrestricted command execution.

Exercise 8 — Verify Membership of the kali-trusted Group

Command:

getent group kali-trusted

Observed output:

kali-trusted:x:118:

The group exists, but no supplementary members are listed in the output.

To verify the actual membership of the kali account, the following command was executed:

id kali

The output showed that kali belongs to groups including sudo and docker, but not kali-trusted.

Finding:

The current kali account is not a member of kali-trusted. Therefore, the unrestricted passwordless sudo rule assigned to that group does not apply to this account through group membership.

No sudoers configuration was modified during this investigation.

4. Security Findings Summary

Area	Finding
Current account	kali
UID	1000
Sudo group membership	Yes
Docker group membership	Yes
Unrestricted sudo permission	Yes, through sudo -l
kali-trusted membership	No
Sudoers syntax validation	All three files parsed successfully
Passwordless sudo rule	Present for kali-trusted
OpenVAS sudo rule	Restricted to /usr/sbin/openvas

5. Security Concepts Learned

Normal User vs Root User

A normal user operates with the permissions assigned to their account. The root account has extensive administrative control over the operating system.

Sudo

sudo allows an authorized user to execute commands with elevated privileges according to the system’s sudoers policy.

Principle of Least Privilege

Users, applications and processes should receive only the minimum permissions required to perform their assigned responsibilities.

This reduces the potential damage caused by mistakes, compromised accounts or malicious activity.

Risks of Excessive Privileges

Unnecessary administrative permissions can increase the risk of:

* Unauthorized access to sensitive information.
* Accidental modification or deletion of system files.
* Installation of malicious software.
* Privilege abuse following account compromise.
* Loss of system integrity and availability.

6. Knowledge Check

Question 1: Why is it dangerous to give every user unrestricted sudo access?

My answer: Because unauthorized users would gain access to sensitive information and cause so much harm.

Assessment: Correct. Unrestricted administrative privileges can allow unauthorized system changes, data exposure and other serious security incidents.

Question 2: What is the principle of least privilege?

My answer: Only access is granted to users who have a need for specific tasks.

Assessment: Correct. The principle means granting only the minimum permissions necessary to complete assigned tasks.

7. Challenges and Observations

The session involved inspecting several layers of Linux privilege management rather than simply checking whether a user belonged to the sudo group.

Important observations included:

* Sudo permissions can be broader than expected.
* Privileged access can also come through groups such as docker.
* Sudoers configuration can contain rules beyond the main /etc/sudoers file.
* Passwordless sudo is not automatically safe; the scope of the permission matters.
* A configuration can pass syntax validation while still presenting security risks.

No sudoers files or group memberships were modified during this exercise. The work was limited to inspection and verification.

8. Evidence and Screenshot Checklist

Save useful terminal screenshots in:

linux-administration/week-01/evidence/day-04/

Suggested evidence files:

1. day-04-user-identity-and-groups.png — whoami and id showing the account identity and group memberships.
2. day-04-sudo-permissions.png — sudo -l showing the account’s administrative permissions.
3. day-04-privilege-elevation.png — sudo whoami returning root, followed by whoami returning kali.
4. day-04-group-membership.png — the sudo and docker group lookups.
5. day-04-sudoers-validation.png — visudo -c showing the three successful syntax checks.
6. day-04-sudoers-rules-inspection.png — the two additional sudoers rules.
7. day-04-kali-trusted-check.png — the group lookup and id kali confirming that the account is not a member.

Only mark evidence as captured after saving the corresponding screenshots.

9. Final Reflection

Day 04 improved my understanding of Linux privilege management and the importance of controlling administrative access.

I learned how to identify privileged users, inspect sudo permissions, validate sudoers syntax and investigate additional privilege rules.

One important discovery was that privilege management extends beyond membership of the sudo group. Other groups, including Docker-related groups, may also carry significant security implications.

This knowledge is relevant to DevOps and DevSecOps because infrastructure security depends on controlling who can access, modify and administer systems.

Day 04 Status: PRACTICAL COMPLETED

Next session: Day 05 — Linux Process Management and Service Administration.