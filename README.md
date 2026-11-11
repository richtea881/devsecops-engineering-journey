# devsecops-engineering-journey
Linux Administration Lab 02: Shared Directories and Setgid Group Inheritance

Status: Completed and verified
Date: 11 November 2026
Environment: Kali Linux 2026

Project overview

This practical exercise demonstrates how Linux setgid directory permissions support controlled collaboration through consistent group ownership.

Objective

Create and verify a shared Linux project directory in which files inherit the directory’s group while retaining ownership by the user who created them.

Lab architecture

* Linux user: devopsuser
* Shared group: devops-lab
* Directory: /opt/devops-project
* Directory owner: root
* Directory group: devops-lab
* Directory permissions: 2770

Implementation and verification

The existing directory was inspected using:

ls -ld /opt/devops-project

The user and group memberships were verified using:

whoami
id

A test file was created:

touch /opt/devops-project/day2-test.txt

Ownership was checked:

ls -l /opt/devops-project/day2-test.txt

Detailed metadata was inspected:

stat /opt/devops-project/day2-test.txt

Verified results

The directory was owned by root:devops-lab with setgid enabled.

The test file was created with:

* Owner: devopsuser
* Group: devops-lab
* Permissions: 0664
* File size: 0 bytes

This confirmed the expected group inheritance behaviour.

Security relevance

Consistent group ownership helps prevent collaboration problems and reduces pressure to grant excessive permissions to shared project files.

The exercise also demonstrates that directory permissions and file permissions must be considered together.

Scope and limitations

This was an isolated local Linux lab using a Kali Linux virtual machine.

No production system or public cloud resource was modified.

The exercise verified group inheritance for one test file. It did not constitute a full multi-user access-control test.

Evidence

Terminal output was recorded during the exercise.

Screenshots should be added to the evidence directory after capture.

Suggested commit

docs(linux): document setgid shared directory lab

Learning outcome

Demonstrated practical understanding of Linux setgid directory behaviour, group inheritance, and access-control fundamentals relevant to DevSecOps engineering.
