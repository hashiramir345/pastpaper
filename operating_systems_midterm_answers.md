# Operating Systems (Theory) - Mid-Term Exam Solutions (Fall 2025)

**Course:** Operating Systems (Theory)  
**Instructor:** Mr. Masood Ahmed  
**Semester:** BSCS - 5  
**Max Points:** 60  
**Institution:** Institute of Space Technology, KICSIT Campus, Kahuta  

---

## Question 1 (15 Marks)
**Draw and explain the diagram of Seven States Process Model, also describe the Process Control Block (PCB).**

---

### Part A: Seven-State Process Model

In an Operating System, memory (RAM) is limited. When too many processes enter memory or when a process waits for a slow I/O device, the OS moves processes between Main Memory (RAM) and Secondary Memory (Disk/Swap Space). The **Seven-State Process Model** represents the complete lifecycle of a process under these conditions.

#### 1. Diagram of 7-State Process Model (ASCII Architecture)

```
                       +------------+
                       |    NEW     |
                       +-----+------+
                             | Admit
                             v
+------------------+  Suspend+------------+             +------------+
|  READY/SUSPEND   |<--------|   READY    |------------>|  RUNNING   |
| (Secondary Mem)  |-------->| (RAM/Mem)  | Dispatch    |   (CPU)    |
+------------------+ Activate+------------+             +-----+------+
         ^                         ^                          |
         |                         | Event                    | Time Slice Exceeded /
         |                         | Occurs /                 | Preemption
         | Event                   | I/O Done                 |
         | Occurs                  |                          v
+--------+---------+  Suspend+-----+------+             +------------+
| BLOCKED/SUSPEND  |<--------|  BLOCKED   |<------------|  BLOCKED   |
| (Secondary Mem)  |-------->| (RAM/Mem)  | I/O Wait    | (Exit/I/O) |
+------------------+ Activate+------------+             +-----+------+
                                                              |
                                                              | Exit / Terminate
                                                              v
                                                        +------------+
                                                        | TERMINATED |
                                                        +------------+
```

#### 2. Explanation of the Seven States

1. **New:** The process is being created and its data structure is being initialized, but it has not yet been loaded into Main Memory (RAM).
2. **Ready:** The process is loaded into Main Memory and is waiting in the Ready Queue to be assigned to the CPU by the scheduler.
3. **Running:** The process has been assigned to the CPU (processor) and its instructions are currently being executed.
4. **Blocked (Waiting):** The process cannot execute until a specific event occurs (e.g., waiting for I/O completion or user input). It resides in Main Memory.
5. **Terminated (Exit):** The process has finished its execution or was halted due to an error. The OS reclaims its allocated resources.
6. **Blocked/Suspend:** The process is blocked waiting for an I/O event AND has been swapped out from Main Memory to Secondary Storage (Disk) due to RAM shortages.
7. **Ready/Suspend:** The process is ready to execute as soon as it is loaded into Main Memory, but it currently resides in Secondary Storage (Disk).

#### 3. Key Transitions Between States

* **Admit:** Process is created and moved to `Ready` (or `Ready/Suspend` if memory is full).
* **Dispatch:** The OS CPU Scheduler selects a process from `Ready` state and allocates CPU to it (`Running`).
* **Time Slice Exceeded / Preemption:** A running process is moved back to `Ready` because its time limit expired or a higher-priority process arrived.
* **I/O or Event Wait:** Running process requires I/O or an event, so it enters `Blocked` state.
* **I/O or Event Completion:** The event finishes; the process transitions from `Blocked` to `Ready` (or `Blocked/Suspend` to `Ready/Suspend`).
* **Suspend (Swapping Out):** When RAM is full, the OS moves processes from `Ready` $\rightarrow$ `Ready/Suspend` or `Blocked` $\rightarrow$ `Blocked/Suspend`.
* **Activate (Swapping In):** When RAM becomes available, the OS moves processes from `Ready/Suspend` $\rightarrow$ `Ready` or `Blocked/Suspend` $\rightarrow$ `Blocked`.

---

### Part B: Process Control Block (PCB)

The **Process Control Block (PCB)** (also called a Task Control Block) is a data structure used by the Operating System to store all metadata and execution state information about a specific process.

#### Key Components of a PCB:

1. **Process Identifier (PID):** A unique integer assigned by the OS to identify the process.
2. **Process State:** Stores the current state of the process (`New`, `Ready`, `Running`, `Blocked`, etc.).
3. **Program Counter (PC):** Holds the memory address of the next instruction to be executed for this process.
4. **CPU Registers:** Stores accumulator values, index registers, stack pointers, and general-purpose registers when context switching occurs.
5. **CPU Scheduling Information:** Includes process priority, pointers to scheduling queues, and other scheduling parameters.
6. **Memory Management Information:** Page tables, segment tables, and base/limit registers detailing allocated memory space.
7. **Accounting Information:** Execution time used, time limits, account numbers, and process age.
8. **I/O Status Information:** List of open files, allocated I/O devices, and pending I/O requests.

---

## Question 2 (15 Marks)
**What is a System Call? Write the three general methods which are used to pass the parameters between a running program and OS. Describe the type of System Calls.**

---

### Part A: Definition of System Call

A **System Call** is a programmatic interface provided by the Operating System that allows a user-level program to request services from the OS kernel (which runs in privileged mode). Examples include reading a file, creating a new process, or accessing hardware.

---

### Part B: 3 General Methods to Pass Parameters to System Calls

When a user program invokes a system call, it often needs to pass parameters (e.g., file paths, buffer sizes) to the OS kernel. The three primary methods are:

1. **Pass in CPU Registers:**
   * **Mechanism:** Parameters are stored directly inside CPU registers (e.g., `EAX`, `EBX`, `ECX`).
   * **Pros/Cons:** Fastest method, but limited by the total number of physical CPU registers available.

2. **Pass via Block/Table in Memory:**
   * **Mechanism:** Parameters are written into a memory block or table, and the *address* (pointer) of that block is passed to the kernel inside a register.
   * **Pros/Cons:** No limit on the number or size of parameters passed; widely used in Linux and Windows.

3. **Pass via System Stack:**
   * **Mechanism:** The user application pushes parameters onto the user program stack. The OS kernel pops the parameters off the stack during the system call handler execution.
   * **Pros/Cons:** Simple and scalable without restricting parameter size or number.

---

### Part C: Types of System Calls

System calls are categorized into 5 main functional areas:

1. **Process Control:**
   * Manages process execution, creation, and termination.
   * *Examples:* `fork()`, `exec()`, `exit()`, `wait()`.

2. **File Management:**
   * Handles operations on files (creating, reading, writing, closing, deleting).
   * *Examples:* `open()`, `read()`, `write()`, `close()`.

3. **Device Management:**
   * Requests access to physical or virtual hardware devices (disks, printers, monitors).
   * *Examples:* `read()`, `write()`, `ioctl()`.

4. **Information Maintenance:**
   * Transfers information between the user application and the OS kernel (e.g., getting time, date, system metrics).
   * *Examples:* `getpid()`, `time()`, `alarm()`.

5. **Communication:**
   * Manages inter-process communication (IPC) via message passing or shared memory.
   * *Examples:* `pipe()`, `shmget()`, `mmap()`, `socket()`.

---

## Question 3 (15 Marks)
**Describe the Aim of Process Scheduling and Compare the different types of Process Schedulers, also explain the Inter Process Communication (IPC).**

---

### Part A: Aim of Process Scheduling

The primary aims of process scheduling in an Operating System are:

1. **Maximize CPU Utilization:** Keep the CPU as busy as possible (ideally 100% of the time).
2. **Maximize Throughput:** Complete the maximum number of processes per unit of time.
3. **Minimize Turnaround Time ($T.A.T$):** Minimize the total time taken from process submission to its completion.
4. **Minimize Waiting Time ($W.T$):** Minimize the total time a process spends waiting in the Ready Queue.
5. **Minimize Response Time:** Minimize time from request submission to the first response (crucial for interactive systems).
6. **Ensure Fairness:** Guarantee that every process gets a fair share of CPU time without suffering from starvation.

---

### Part B: Comparison of Process Schedulers

| Feature | Long-Term Scheduler (Job Scheduler) | Short-Term Scheduler (CPU Scheduler) | Medium-Term Scheduler (Swapper) |
| :--- | :--- | :--- | :--- |
| **Primary Goal** | Selects processes from pool and loads them into memory (RAM). | Selects a process from Ready Queue and assigns CPU. | Swaps processes in/out of Main Memory to disk. |
| **Frequency of Action** | Very infrequently (seconds or minutes). | Extremely frequently (milliseconds). | Moderately frequently (as RAM fills up). |
| **Control Degree** | Controls the **Degree of Multiprogramming** (number of processes in RAM). | Controls execution speed and CPU utilization. | Reduces the **Degree of Multiprogramming**. |
| **Speed** | Slowest. | Fastest. | Medium speed. |
| **State Transitions** | `New` $\rightarrow$ `Ready` | `Ready` $\rightarrow$ `Running` | `Blocked` $\leftrightarrow$ `Blocked/Suspend`<br>`Ready` $\leftrightarrow$ `Ready/Suspend` |

---

### Part C: Inter-Process Communication (IPC)

**Inter-Process Communication (IPC)** is a mechanism provided by the Operating System that allows independent processes to communicate, share data, and synchronize their actions.

#### The Two Main Models of IPC:

1. **Shared Memory Model:**
   * **Concept:** A region of physical memory is designated as "shared." Communicating processes map this memory region directly into their address space.
   * **Characteristics:** Extremely fast (no kernel involvement required during actual data transfers after initial setup). Requires synchronization primitives (e.g., Semaphores, Mutexes) to avoid race conditions.

2. **Message Passing Model:**
   * **Concept:** Processes communicate by exchanging explicit messages (`send(message)` and `receive(message)`) through kernel-managed message queues or sockets.
   * **Characteristics:** Slower than shared memory due to system call overhead, but easier to implement in distributed/networked systems without memory sharing.

---

## Question 4 (15 Marks)
**Solve the followings by using First Come First Served (FCFS) Scheduling to calculate C.T, T.A.T, W.T, Average W.T and Average T.A.T. Also draw the Gantt chart.**

---

### Given Data

| Process ID | Arrival Time ($A.T$) | Burst Time ($B.T$) |
| :---: | :---: | :---: |
| $P_1$ | 3 | 4 |
| $P_2$ | 5 | 3 |
| $P_3$ | 0 | 2 |
| $P_4$ | 5 | 1 |
| $P_5$ | 4 | 3 |

---

### Step 1: Sort Processes by Arrival Time (FCFS Rule)

First Come First Served (FCFS) schedules processes non-preemptively based strictly on the lowest **Arrival Time ($A.T$)**:

1. **$P_3$** ($A.T = 0, B.T = 2$)
2. **$P_1$** ($A.T = 3, B.T = 4$)
3. **$P_5$** ($A.T = 4, B.T = 3$)
4. **$P_2$** ($A.T = 5, B.T = 3$) *(Tie-breaker between $P_2$ & $P_4$ based on table order)*
5. **$P_4$** ($A.T = 5, B.T = 1$)

---

### Step 2: Step-by-Step Scheduling Execution Trace

* **Time $t = 0$ to $t = 2$:** $P_3$ arrives at $0$, executes for $2$ units. Completes at $t = 2$.
* **Time $t = 2$ to $t = 3$:** CPU is **IDLE** for $1$ time unit because the next process ($P_1$) arrives at $t = 3$.
* **Time $t = 3$ to $t = 7$:** $P_1$ executes for $4$ units ($3 + 4 = 7$). Completes at $t = 7$.
* **Time $t = 7$ to $t = 10$:** $P_5$ (arrived at $4$) executes for $3$ units ($7 + 3 = 10$). Completes at $t = 10$.
* **Time $t = 10$ to $t = 13$:** $P_2$ (arrived at $5$) executes for $3$ units ($10 + 3 = 13$). Completes at $t = 13$.
* **Time $t = 13$ to $t = 14$:** $P_4$ (arrived at $5$) executes for $1$ unit ($13 + 1 = 14$). Completes at $t = 14$.

---

### Step 3: Gantt Chart

```
+-------+-------+-------+-------+-------+-------+
|  P3   | IDLE  |  P1   |  P5   |  P2   |  P4   |
+-------+-------+-------+-------+-------+-------+
0       2       3       7       10      13      14
```

---

### Step 4: Important Formulas

$$\text{Turnaround Time } (T.A.T) = \text{Completion Time } (C.T) - \text{Arrival Time } (A.T)$$

$$\text{Waiting Time } (W.T) = \text{Turnaround Time } (T.A.T) - \text{Burst Time } (B.T)$$

---

### Step 5: Complete Calculation Table

| Process ID | Arrival Time ($A.T$) | Burst Time ($B.T$) | Completion Time ($C.T$) | Turnaround Time ($T.A.T = C.T - A.T$) | Waiting Time ($W.T = T.A.T - B.T$) |
| :---: | :---: | :---: | :---: | :---: | :---: |
| **$P_1$** | 3 | 4 | 7 | $7 - 3 = 4$ | $4 - 4 = 0$ |
| **$P_2$** | 5 | 3 | 13 | $13 - 5 = 8$ | $8 - 3 = 5$ |
| **$P_3$** | 0 | 2 | 2 | $2 - 0 = 2$ | $2 - 2 = 0$ |
| **$P_4$** | 5 | 1 | 14 | $14 - 5 = 9$ | $9 - 1 = 8$ |
| **$P_5$** | 4 | 3 | 10 | $10 - 4 = 6$ | $6 - 3 = 3$ |
| **Total** | — | — | — | **29** | **16** |

---

### Step 6: Final Averages

#### Average Turnaround Time (Average $T.A.T$):

$$\text{Average } T.A.T = \frac{\sum T.A.T}{\text{Total Processes}} = \frac{4 + 8 + 2 + 9 + 6}{5} = \frac{29}{5} = 5.8 \text{ time units}$$

#### Average Waiting Time (Average $W.T$):

$$\text{Average } W.T = \frac{\sum W.T}{\text{Total Processes}} = \frac{0 + 5 + 0 + 8 + 3}{5} = \frac{16}{5} = 3.2 \text{ time units}$$

---

### Final Answers Summary (Q4):

* **Completion Times ($C.T$):** $P_1 = 7$, $P_2 = 13$, $P_3 = 2$, $P_4 = 14$, $P_5 = 10$
* **Turnaround Times ($T.A.T$):** $P_1 = 4$, $P_2 = 8$, $P_3 = 2$, $P_4 = 9$, $P_5 = 6$
* **Waiting Times ($W.T$):** $P_1 = 0$, $P_2 = 5$, $P_3 = 0$, $P_4 = 8$, $P_5 = 3$
* **Average $T.A.T$:** $5.8 \text{ units}$
* **Average $W.T$:** $3.2 \text{ units}$