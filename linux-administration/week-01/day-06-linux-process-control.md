# Day 06 — Linux Process Control

**Programme:** Four-Month DevSecOps Engineering Journey
**Module:** Linux System Administration
**Week:** 01
**Environment:** Kali Linux 2026
**Virtualisation:** VMware Fusion
**Status:** Completed
**Assessment:** 4/5 (80%)

## **1. Learning Objectives**

The objective of this session was to understand how Linux manages running processes and how an administrator can control their execution.

By the end of this session, I practised how to:

- Identify running processes.
- Start processes in the background.
- Inspect background jobs.
- Suspend and resume processes.
- Understand Linux process states.
- Send signals to processes.
- Terminate processes safely.

## **2. Practical Environment**

|**Item**|**Details**|
|---|---|
|Operating system|Kali Linux|
|Environment|Kali Linux virtual machine|
|Shell|Linux terminal|
|Topic|Process Control|

## **3. Practical Exercises**

### **Exercise 1: Start a background process**

Command:

```bash
sleep 600 &
```

**Explanation:**

- `sleep 600` starts a process that waits for 600 seconds.
- `&` runs the process in the background.
- This allows the terminal to remain available for other commands.

### **Exercise 2: Identify background jobs**

Command:

```bash
jobs -l
```

Observed output:

```text
[1] + 51986 running sleep 600
```

**Explanation:**

- `jobs` lists jobs started from the current shell.
- `-l` displays the process ID (PID).
- `51986` was the PID assigned to the background process.

### **Exercise 3: Suspend a process**

Command:

```bash
kill -STOP 51986
```

**Explanation:**

`SIGSTOP` suspends a process immediately. Unlike a normal termination signal, it does not terminate the process.

Verification:

```bash
ps -o pid,ppid,stat,cmd -p 51986
```

Observed output:

```text
PID    PPID   STAT   CMD
51986  2855   TN     sleep 600
```

**Interpretation:**

- `PID`: Process identifier.
- `PPID`: Parent process identifier.
- `T`: Process is stopped.
- `N`: Nice-priority indicator.

The important observation was the `T` state, confirming that the process had been suspended.

### **Exercise 4: Resume a suspended process**

Command:

```bash
kill -CONT 51986
```

**Explanation:**

`SIGCONT` instructs a stopped process to resume execution.

Verification:

```bash
ps -o pid,ppid,stat,cmd -p 51986
```

Observed output:

```text
PID    PPID   STAT   CMD
51986  2855   SN     sleep 600
```

**Interpretation:**

- `S`: Interruptible sleep.
- `N`: Nice-priority indicator.

The process had resumed from its stopped state.

### **Exercise 5: Terminate and verify cleanup**

Command:

```bash
kill 51986
```

Verification:

```bash
ps -p 51986
```

**Result:** No process information was returned.

This confirmed that the process had terminated and was no longer running.

## **4. Understanding Linux Process States**

Linux uses process-state indicators to describe what a process is doing.

|**State**|**Meaning**|
|---|---|
|R|Running or runnable|
|S|Interruptible sleep|
|D|Uninterruptible sleep|
|T|Stopped or suspended|
|Z|Zombie process|

Additional indicators may appear alongside the main state. For example, `N` indicates a process with a positive nice value.

## **5. Understanding Linux Signals**

Signals allow administrators to communicate with and control processes.

|**Signal**|**Number**|**Purpose**|
|---|---|---|
|SIGTERM|15|Requests graceful termination|
|SIGKILL|9|Forces immediate termination|
|SIGSTOP|19*|Suspends a process|
|SIGCONT|18*|Resumes a stopped process|

_Signal numbers can vary across Linux architectures. Signal names are more portable._

Important commands:

```bash
kill -TERM PID
kill -KILL PID
kill -STOP PID
kill -CONT PID
```

**Key distinction:**

- `SIGTERM` gives a process an opportunity to shut down cleanly.
- `SIGKILL` cannot be caught or ignored and forces termination.
- `SIGSTOP` suspends execution.
- `SIGCONT` resumes execution.

## **6. Assessment Results**

**Final score: 4/5 (80%)**

|**Question**|**Result**|
|---|---|
|Q1: Purpose of SIGSTOP|Correct|
|Q2: Purpose of SIGCONT|Correct|
|Q3: Meaning of process state T|Incorrect|
|Q4: SIGTERM versus SIGKILL|Correct|
|Q5: Graceful process termination|Correct|

### **Important correction**

The answer to Question 3 was initially incorrect.

The correct interpretation is:

`T = Stopped`

It does not mean terminated. A stopped process remains in existence and can be resumed with `SIGCONT`.

## **7. Key Lessons Learned**

1. Linux assigns a unique process ID (PID) to each process.
2. Background processes allow users to continue working in the terminal.
3. The `jobs -l` command displays background jobs and their process IDs.
4. `SIGSTOP` suspends a process without terminating it.
5. `SIGCONT` resumes a suspended process.
6. `ps` helps inspect process information and states.
7. `SIGTERM` is preferable for normal termination where possible.
8. `SIGKILL` should be reserved for situations where normal termination fails.

## **8. Evidence and Screenshots**

Capture or attach the following evidence if available:

|**Evidence**|**What it should demonstrate**|
|---|---|
|Day06-01-background-process|Background process creation|
|Day06-02-jobs-output|Process identification using `jobs -l`|
|Day06-03-stopped-process|`T` state after SIGSTOP|
|Day06-04-resumed-process|`S` state after SIGCONT|
|Day06-05-process-cleanup|Process termination verification|

Do not mark screenshots as captured until they have actually been saved.

## **9. Session Completion**

**Status:** Completed
**Assessment:** 4/5 (80%)
**Practical verification:** Successful
**Documentation:** Prepared

The session established practical understanding of Linux process control, process-state inspection, signal handling, suspension, resumption and termination.

## **10. Next Learning Objective**

**Day 07 - Linux Process Monitoring and Resource Management**

The next session will build on process control by examining how Linux administrators monitor processes, inspect resource consumption and investigate system performance.