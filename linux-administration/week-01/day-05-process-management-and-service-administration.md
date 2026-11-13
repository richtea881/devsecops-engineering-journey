
# Day 05 — Process Management and Service Administration

**Programme:** Four-Month DevSecOps Engineering Journey  
**Module:** Linux System Administration  
**Week:** 02  
**Environment:** Kali Linux 2026  
**Virtualisation:** VMware Fusion  
**Status:** Completed  
**Assessment:** 15/15 (100%)

---

## 1. Learning Objectives

The objectives of this session were to:

- Understand Linux processes, PIDs and PPIDs.
- Inspect and monitor running processes.
- Understand parent-child process relationships.
- Manage foreground and background processes.
- Interpret Linux process states.
- Understand and use process signals.
- Manage scheduling priority with `nice` and `renice`.
- Identify processes consuming system resources.
- Terminate processes safely and verify the result.

## 2. Process Fundamentals

A process is an instance of a running program. Linux assigns each process a Process ID (PID).

Important process information:

| Field | Meaning |
|---|---|
| PID | Process identifier |
| PPID | Parent Process ID |
| USER | Process owner |
| %CPU | CPU utilisation |
| %MEM | Memory utilisation |
| STAT | Process state |
| COMMAND | Executed program or command |

### Process inspection

```bash
ps
ps aux
ps -ef
ps -o pid,ppid,comm
```

- `ps` displays process information.
- `ps aux` provides a detailed snapshot of processes.
- `ps -ef` displays processes in full format.
- `ps -o` selects specific output fields.

### Process hierarchy

```bash
pstree -p
```

Displays the parent-child relationships between processes, including their PIDs.

### Finding processes

```bash
pgrep sleep
```

Returns matching process IDs.

```bash
pgrep -a sleep
```

Returns matching process IDs and command lines.

## 3. Foreground and Background Processes

Linux supports foreground and background execution.

| Command | Function |
|---|---|
| `command &` | Starts a command in the background |
| `jobs` | Lists jobs managed by the current shell |
| `jobs -l` | Displays jobs and their PIDs |
| `bg` | Resumes a suspended job in the background |
| `fg` | Brings a job into the foreground |
| Ctrl+C | Sends an interrupt to the foreground process group |
| Ctrl+Z | Suspends the foreground process group |

Practical example:

```bash
sleep 300 &
```

This starts a harmless test process in the background.

**Important:** `jobs` only displays jobs managed by the current shell. It is not a complete system-wide process listing.

## 4. Linux Process States

| State | Meaning |
|---|---|
| R | Running or runnable |
| S | Interruptible sleep |
| D | Uninterruptible sleep |
| T | Stopped or suspended |
| Z | Zombie process |

A zombie process has terminated but remains in the process table until its parent collects its exit status.

## 5. Process Monitoring

### Using top

```bash
top
```

`top` provides a continuously refreshed view of running processes.

Useful keyboard controls:

| Key | Function |
|---|---|
| P | Sort by CPU usage |
| M | Sort by memory usage |
| k | Send a signal to a process |
| q | Quit |

### Using htop

```bash
htop
```

`htop` provides an interactive process-monitoring interface.

Important function keys:

| Key | Function |
|---|---|
| F1 | Help |
| F2 | Setup |
| F3 | Search |
| F4 | Filter |
| F5 | Process tree |
| F6 | Sort by column |
| F9 | Send signal |
| F10 | Quit |

**MacBook keyboard note:** Use the Fn modifier with function keys where required, for example `Fn + F5` to display the process tree.

During monitoring, Jenkins was observed using approximately 13.9% memory. Its displayed process information included PID 1004 and PPID 1.

This reinforced the importance of investigating a process before terminating it.

### Difference between ps and top

- `ps aux` provides a snapshot.
- `top` continuously refreshes process information.
- `htop` provides an interactive alternative with additional navigation and visualisation features.

## 6. Process Scheduling Priority

Linux uses nice values to influence CPU scheduling priority.

| Nice value | Relative scheduling priority |
|---|---|
| -20 | Highest |
| 0 | Default |
| +19 | Lowest |

### Starting a process with a nice value

```bash
nice -n 10 sleep 300 &
```

### Changing an existing process priority

```bash
renice 10 -p PID
```

Key distinction:

- `nice` starts a process with a requested nice value.
- `renice` changes the nice value of an existing process.

A practical `renice` exercise successfully changed a test process's nice value to 10.

## 7. Linux Signals and Process Termination

Signals allow processes to receive instructions from the operating system or other processes.

| Signal | Number | Purpose |
|---|---:|---|
| SIGTERM | 15 | Requests graceful termination |
| SIGKILL | 9 | Forces immediate termination |
| SIGSTOP | 19 | Stops a process |
| SIGCONT | 18 | Resumes a stopped process |

Examples:

```bash
kill -TERM PID
kill -9 PID
kill -STOP PID
kill -CONT PID
```

**Professional practice:** Use SIGTERM first when termination is justified. SIGKILL should be reserved for cases where graceful termination fails and escalation is necessary.

### kill versus pkill

Terminate a specific process by PID:

```bash
kill -TERM 12345
```

Terminate processes by exact process name:

```bash
pkill -TERM -x sleep
```

Use `pkill` carefully because it can affect multiple matching processes.

## 8. Hands-On Practical Exercises

### Exercise 1 — Create and identify a process

Command:

```bash
sleep 600 &
```

Identify the process:

```bash
pgrep -a sleep
```

Observed output:

```text
114584 sleep 600
```

The process was running with PID 114584.

### Exercise 2 — Inspect a specific process

Command:

```bash
ps -p 114584 -o pid,ppid,user,%cpu,%mem,stat,etime,cmd
```

Observed output:

```text
114584   51519   kali   0.0   0.1   SN   01:00   sleep 600
```

Interpretation:

| Field | Value | Explanation |
|---|---|---|
| PID | 114584 | Process identifier |
| PPID | 51519 | Parent process |
| USER | kali | Process owner |
| CPU | 0.0% | No measurable CPU consumption |
| MEM | 0.1% | Low memory consumption |
| STAT | SN | Interruptible sleep with positive nice value |
| ETIME | 01:00 | Approximately one minute elapsed |
| CMD | sleep 600 | Executed command |

This demonstrated how to investigate a process before taking action.

### Exercise 3 — Terminate the process gracefully

Command:

```bash
kill -TERM 114584
```

SIGTERM was sent to the test process, requesting graceful termination.

### Exercise 4 — Verify termination

Command:

```bash
ps -p 114584
```

**Observed result:** Empty output.

Interpretation: The process no longer appeared in the process table, confirming successful termination.

### Exercise 5 — Terminate processes by name

Command:

```bash
pkill -TERM -x sleep
```

Verification:

```bash
pgrep -a sleep
```

**Observed result:** No matching processes remained.

This demonstrated process termination by exact process name and verification using `pgrep`.

## 9. Professional Process Troubleshooting Workflow

A key lesson from this session was the importance of investigating processes before terminating them.

The professional workflow is:

1. **Observe:** Identify unusual CPU or memory consumption.
2. **Identify:** Establish the PID and command.
3. **Investigate:** Examine ownership, PPID, process state and resource usage.
4. **Decide:** Determine whether termination is justified.
5. **Act:** Use an appropriate signal, starting with SIGTERM where appropriate.
6. **Verify:** Confirm whether the process terminated.
7. **Escalate:** Consider SIGKILL only when necessary.

Example inspection command:

```bash
ps -p 12345 -o pid,ppid,user,%cpu,%mem,stat,etime,cmd
```

This approach is relevant to production Linux administration and DevOps troubleshooting.

## 10. Assessment Results

**Final assessment score: 15/15 (100%).**

The assessment tested:

1. Process snapshots using `ps aux`.
2. PID and PPID.
3. Process hierarchy using `pstree -p`.
4. Background execution using `&`.
5. Shell job management using `jobs -l`.
6. Zombie process identification.
7. SIGTERM.
8. `renice`.
9. `pgrep -a`.
10. `top` versus `ps`.
11. Nice values.
12. `pkill` and exact process-name matching.
13. Process tree navigation in `htop`.
14. Zombie process interpretation.
15. Safe process investigation and termination.

All 15 questions were answered correctly.

A practical exercise followed the assessment. A real test process was created, inspected, terminated and verified.

## 11. Practical Competencies Demonstrated

- [x] Identify processes using PIDs.
- [x] Understand PID and PPID relationships.
- [x] Inspect processes using `ps`.
- [x] Display process hierarchy using `pstree`.
- [x] Locate processes using `pgrep`.
- [x] Understand foreground and background execution.
- [x] Interpret common Linux process states.
- [x] Monitor processes using `top`.
- [x] Navigate `htop`.
- [x] Understand process scheduling priority.
- [x] Apply `renice` to an existing process.
- [x] Understand Linux signals.
- [x] Terminate a process using SIGTERM.
- [x] Use `pkill` for process-name matching.
- [x] Verify process termination.
- [x] Apply a structured troubleshooting approach.

## 12. Evidence Register

Recommended evidence filenames:

| Evidence ID | Filename | Evidence |
|---|---|---|
| D5-E01 | `day5-process-inspection.png` | Process inspection using ps |
| D5-E02 | `day5-process-hierarchy.png` | Parent-child process relationships |
| D5-E03 | `day5-htop-monitoring.png` | Interactive process monitoring |
| D5-E04 | `day5-process-priority.png` | Nice and renice exercises |
| D5-E05 | `day5-process-termination.png` | SIGTERM and verification |
| D5-E06 | `day5-process-assessment.png` | Final assessment results |

These are recommended evidence targets, not claims that screenshots have already been captured.

## 13. Relevance to DevOps and DevSecOps

Process management is fundamental to Linux administration and infrastructure operations.

It supports:

- Monitoring application services.
- Investigating CPU and memory consumption.
- Troubleshooting Jenkins and other CI/CD services.
- Managing background jobs.
- Identifying unexpected processes.
- Understanding service dependencies.
- Investigating suspicious process activity.

From a security perspective, process inspection helps administrators identify unexpected execution, examine process ownership and investigate potentially abnormal behaviour.

## 14. Learning Reflection

Day 5 strengthened my understanding of how Linux creates, manages, monitors and terminates processes.

The most important lesson was that process management is not simply about knowing how to kill a process. It is about understanding what is running, why it is running, who owns it and what could happen if it is terminated.

The practical exercises reinforced the importance of investigating processes before taking action and verifying the outcome afterward.

---

**Day 5 Outcome:** Process management concepts, final assessment and practical verification completed.

**Next:** Day 6 — following the four-month DevSecOps Engineering curriculum.

