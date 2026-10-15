# Operating Systems (Theory) - Complete Exam Solutions

**Course:** Operating Systems (Theory)  
**Instructor:** Mr. Masood Ahmed  
**Semester:** BSCS - 5  
**Institution:** Institute of Space Technology, KICSIT Campus, Kahuta  

---

## Question 1(a): CPU Scheduling Numerical (Shortest Job First - Preemptive / SRTF)

**Problem Data:**
* **Scheduling Algorithm:** Shortest Job First (Preemptive) $\rightarrow$ Also known as **Shortest Remaining Time First (SRTF)**.
* **To Calculate:** Completion Time (C.T), Turnaround Time (T.A.T), Waiting Time (W.T), Average Waiting Time, Average Turnaround Time, and Gantt Chart.

| Process ID | Arrival Time ($AT$) | Burst Time ($BT$) |
| :---: | :---: | :---: |
| $P_1$ | 4 | 1 |
| $P_2$ | 2 | 4 |
| $P_3$ | 5 | 2 |
| $P_4$ | 1 | 6 |
| $P_5$ | 5 | 3 |

---

### Step-by-Step Execution Analysis (SRTF)

1. **At Time $t = 0$:**
   * No process has arrived yet (earliest arrival is $P_4$ at $t = 1$).
   * CPU remains **Idle** from $0$ to $1$.

2. **At Time $t = 1$:**
   * Process $P_4$ arrives ($AT = 1$, $BT = 6$).
   * $P_4$ starts execution. Remaining Burst Time ($RBT$) of $P_4 = 6$.

3. **At Time $t = 2$:**
   * Process $P_2$ arrives ($AT = 2$, $BT = 4$).
   * Compare remaining time: $P_4$ has remaining time $5$, while $P_2$ has $4$. Since $4 < 5$, $P_4$ is **preempted** and $P_2$ takes over the CPU.
   * $P_4$ executed for $1$ unit (from $1$ to $2$).

4. **From $t = 2$ to $t = 4$:**
   * $P_2$ executes for $2$ units. At $t = 4$, $P_1$ arrives ($AT = 4$, $BT = 1$).
   * Remaining time of $P_2$ is now $4 - 2 = 2$.
   * Compare remaining times at $t = 4$: $P_1$ ($RBT = 1$) vs $P_2$ ($RBT = 2$). Since $1 < 2$, $P_2$ is **preempted** and $P_1$ gets the CPU.

5. **From $t = 4$ to $t = 5$:**
   * $P_1$ executes for $1$ unit and **completes** at $t = 5$.
   * **$C.T$ of $P_1 = 5$**

6. **At Time $t = 5$:**
   * Processes $P_3$ ($AT = 5$, $BT = 2$) and $P_5$ ($AT = 5$, $BT = 3$) arrive simultaneously.
   * Current processes ready: $P_2$ ($RBT = 2$), $P_3$ ($RBT = 2$), $P_5$ ($RBT = 3$).
   * Among those with the shortest remaining time ($2$), $P_2$ and $P_3$ tie. We can choose either (commonly arrival order or PID order, let's resume $P_2$).
   * $P_2$ runs from $5$ to $7$ and **completes**.
   * **$C.T$ of $P_2 = 7$**

7. **From $t = 7$ to $t = 9$:**
   * Ready queue contains $P_3$ ($RBT = 2$) and $P_5$ ($RBT = 3$).
   * $P_3$ has the shortest remaining time ($2$), so it runs from $7$ to $9$ and **completes**.
   * **$C.T$ of $P_3 = 9$**

8. **From $t = 9$ to $t = 12$:**
   * Only $P_5$ ($RBT = 3$) and $P_4$ ($RBT = 5$) remain.
   * $P_5$ runs from $9$ to $12$ and **completes**.
   * **$C.T$ of $P_5 = 12$**

9. **From $t = 12$ to $t = 17$:**
   * Finally, $P_4$ runs for its remaining $5$ units from $12$ to $17$ and **completes**.
   * **$C.T$ of $P_4 = 17$**

---

### Gantt Chart

```text
+------+------+------+------+------+------+------+
| Idle |  P4  |  P2  |  P1  |  P2  |  P3  |  P5  |  P4  |
+------+------+------+------+------+------+------+------+
0      1      2      4      5      7      9     12     17
```

---

### Calculation Table

* **Formulas:**
  $$\text{Turnaround Time (T.A.T)} = \text{Completion Time (C.T)} - \text{Arrival Time (AT)}$$
  $$\text{Waiting Time (W.T)} = \text{Turnaround Time (T.A.T)} - \text{Burst Time (BT)}$$

| Process ID | Arrival Time ($AT$) | Burst Time ($BT$) | Completion Time ($C.T$) | Turnaround Time ($T.A.T = C.T - AT$) | Waiting Time ($W.T = T.A.T - BT$) |
| :---: | :---: | :---: | :---: | :---: | :---: |
| $P_1$ | 4 | 1 | 5 | $5 - 4 = 1$ | $1 - 1 = 0$ |
| $P_2$ | 2 | 4 | 7 | $7 - 2 = 5$ | $5 - 4 = 1$ |
| $P_3$ | 5 | 2 | 9 | $9 - 5 = 4$ | $4 - 2 = 2$ |
| $P_4$ | 1 | 6 | 17 | $17 - 1 = 16$ | $16 - 6 = 10$ |
| $P_5$ | 5 | 3 | 12 | $12 - 5 = 7$ | $7 - 3 = 4$ |

* **Total Turnaround Time:** $1 + 5 + 4 + 16 + 7 = 33$
* **Total Waiting Time:** $0 + 1 + 2 + 10 + 4 = 17$

$$\text{Average T.A.T} = \frac{33}{5} = 6.6 \text{ units}$$
$$\text{Average W.T} = \frac{17}{5} = 3.4 \text{ units}$$

---

## Question 1(b): Banker's Algorithm & Safe State Analysis

**Problem Data:**
* **Resource Vector:** $R = \begin{bmatrix} 9 & 3 & 6 \end{bmatrix}$ corresponding to $(R_1, R_2, R_3)$
* **Available Vector:** Initially given as $A = \begin{bmatrix} 0 & 1 & 1 \end{bmatrix}$

### 1. Data Structures of Banker's Algorithm
The Banker's Algorithm requires four core vector/matrix data structures (let $n$ be the number of processes and $m$ be the number of resource types):
1. **Available Vector ($m$ length):** Indicates whether resources are currently free. If $Available[j] = k$, there are $k$ instances of resource type $R_j$ available.
2. **Max Matrix ($n \times m$):** Defines the maximum demand of each process. If $Max[i][j] = k$, process $P_i$ may request at most $k$ instances of resource $R_j$.
3. **Allocation Matrix ($n \times m$):** Defines the resources currently allocated to each process. If $Allocation[i][j] = k$, process $P_i$ currently holds $k$ instances of $R_j$.
4. **Need Matrix ($n \times m$):** Defines the remaining resources each process may still request.  
   $$\text{Need}[i][j] = \text{Max}[i][j] - \text{Allocation}[i][j]$$

---

### 2. Solving the Safety State Problem

#### Step A: Calculate the Allocation Matrix Sum
Sum of resources currently allocated across all processes ($P_1, P_2, P_3, P_4$):
* $R_1 = 1 + 6 + 2 + 0 = 9$
* $R_2 = 0 + 1 + 1 + 0 = 2$
* $R_3 = 0 + 2 + 1 + 2 = 5$  
$$\text{Total Allocated} = \begin{bmatrix} 9 & 2 & 5 \end{bmatrix}$$

#### Step B: Verify / Calculate Available Vector
Given Resource Vector = $\begin{bmatrix} 9 & 3 & 6 \end{bmatrix}$  
$$\text{Available} = \text{Total Resources} - \text{Total Allocated}$$
$$\text{Available} = \begin{bmatrix} 9 & 3 & 6 \end{bmatrix} - \begin{bmatrix} 9 & 2 & 5 \end{bmatrix} = \begin{bmatrix} 0 & 1 & 1 \end{bmatrix}$$
*(This matches the problem statement's given Available vector).*

#### Step C: Calculate the Need Matrix ($\text{Need} = \text{Max} - \text{Allocation}$)

* **Process $P_1$:**
  * $\text{Max} = \begin{bmatrix} 3 & 2 & 2 \end{bmatrix}$
  * $\text{Allocation} = \begin{bmatrix} 1 & 0 & 0 \end{bmatrix}$
  * $\text{Need}_1 = \begin{bmatrix} 3-1 & 2-0 & 2-0 \end{bmatrix} = \begin{bmatrix} 2 & 2 & 2 \end{bmatrix}$

* **Process $P_2$:**
  * $\text{Max} = \begin{bmatrix} 6 & 1 & 3 \end{bmatrix}$
  * $\text{Allocation} = \begin{bmatrix} 6 & 1 & 2 \end{bmatrix}$
  * $\text{Need}_2 = \begin{bmatrix} 6-6 & 1-1 & 3-2 \end{bmatrix} = \begin{bmatrix} 0 & 0 & 1 \end{bmatrix}$

* **Process $P_3$:**
  * $\text{Max} = \begin{bmatrix} 3 & 1 & 4 \end{bmatrix}$
  * $\text{Allocation} = \begin{bmatrix} 2 & 1 & 1 \end{bmatrix}$
  * $\text{Need}_3 = \begin{bmatrix} 3-2 & 1-1 & 4-1 \end{bmatrix} = \begin{bmatrix} 1 & 0 & 3 \end{bmatrix}$

* **Process $P_4$:**
  * $\text{Max} = \begin{bmatrix} 4 & 2 & 2 \end{bmatrix}$
  * $\text{Allocation} = \begin{bmatrix} 0 & 0 & 2 \end{bmatrix}$
  * $\text{Need}_4 = \begin{bmatrix} 4-0 & 2-0 & 2-2 \end{bmatrix} = \begin{bmatrix} 4 & 2 & 0 \end{bmatrix}$

---

#### Step D: Safety Algorithm Execution

* **Initial Available Vector:** $Work = \begin{bmatrix} 0 & 1 & 1 \end{bmatrix}$

1. **Check $P_1$:** $\text{Need}_1 = \begin{bmatrix} 2 & 2 & 2 \end{bmatrix}$  
   Is $\text{Need}_1 \le Work$ ($\begin{bmatrix} 2 & 2 & 2 \end{bmatrix} \le \begin{bmatrix} 0 & 1 & 1 \end{bmatrix}$)? **No.** (Cannot run yet).

2. **Check $P_2$:** $\text{Need}_2 = \begin{bmatrix} 0 & 0 & 1 \end{bmatrix}$  
   Is $\text{Need}_2 \le Work$ ($\begin{bmatrix} 0 & 0 & 1 \end{bmatrix} \le \begin{bmatrix} 0 & 1 & 1 \end{bmatrix}$)? **Yes!**  
   * Execute $P_2$: Release its allocation back to Work.  
   * $Work = Work + \text{Allocation}_2 = \begin{bmatrix} 0 & 1 & 1 \end{bmatrix} + \begin{bmatrix} 6 & 1 & 2 \end{bmatrix} = \begin{bmatrix} 6 & 2 & 3 \end{bmatrix}$.  
   * **Process $P_2$ completes successfully.** (Safe Sequence so far: $<P_2>$)

3. **Check $P_1$:** $\text{Need}_1 = \begin{bmatrix} 2 & 2 & 2 \end{bmatrix}$  
   Is $\text{Need}_1 \le Work$ ($\begin{bmatrix} 2 & 2 & 2 \end{bmatrix} \le \begin{bmatrix} 6 & 2 & 3 \end{bmatrix}$)? **Yes!**  
   * Execute $P_1$:  
   * $Work = Work + \text{Allocation}_1 = \begin{bmatrix} 6 & 2 & 3 \end{bmatrix} + \begin{bmatrix} 1 & 0 & 0 \end{bmatrix} = \begin{bmatrix} 7 & 2 & 3 \end{bmatrix}$.  
   * **Process $P_1$ completes.** (Safe Sequence: $<P_2, P_1>$)

4. **Check $P_3$:** $\text{Need}_3 = \begin{bmatrix} 1 & 0 & 3 \end{bmatrix}$  
   Is $\text{Need}_3 \le Work$ ($\begin{bmatrix} 1 & 0 & 3 \end{bmatrix} \le \begin{bmatrix} 7 & 2 & 3 \end{bmatrix}$)? **Yes!**  
   * Execute $P_3$:  
   * $Work = Work + \text{Allocation}_3 = \begin{bmatrix} 7 & 2 & 3 \end{bmatrix} + \begin{bmatrix} 2 & 1 & 1 \end{bmatrix} = \begin{bmatrix} 9 & 3 & 4 \end{bmatrix}$.  
   * **Process $P_3$ completes.** (Safe Sequence: $<P_2, P_1, P_3>$)

5. **Check $P_4$:** $\text{Need}_4 = \begin{bmatrix} 4 & 2 & 0 \end{bmatrix}$  
   Is $\text{Need}_4 \le Work$ ($\begin{bmatrix} 4 & 2 & 0 \end{bmatrix} \le \begin{bmatrix} 9 & 3 & 4 \end{bmatrix}$)? **Yes!**  
   * Execute $P_4$:  
   * $Work = Work + \text{Allocation}_4 = \begin{bmatrix} 9 & 3 & 4 \end{bmatrix} + \begin{bmatrix} 0 & 0 & 2 \end{bmatrix} = \begin{bmatrix} 9 & 3 & 6 \end{bmatrix}$.  
   * **Process $P_4$ completes.** (Safe Sequence: $<P_2, P_1, P_3, P_4>$)

### Conclusion for Q1(b):
The system is in a **Safe State**.  
A valid **Safe Sequence** is: **$<P_2, P_1, P_3, P_4>$**

---

## Question 2: Threads & Multithreading

### (a) Single-Threaded vs. Multi-Threaded Processes
* **Single-Threaded Process:** Contains a single execution stream (one Program Counter and one sequence of instructions executed at a time). If it blocks on I/O, the entire application halts.
* **Multi-Threaded Process:** Contains multiple execution threads within the same process context, sharing the same memory space, global variables, and open files, but having their own registers and stacks.

**Benefits of Threads vs. Processes:**
1. **Responsiveness:** Allows an application to remain interactive even if part of it is blocked.
2. **Resource Sharing:** Threads share memory and resources of the parent process naturally without complex IPC mechanisms.
3. **Economy:** Creating and context-switching threads is much faster and cheaper than full processes.
4. **Scalability:** Multi-threaded programs can execute concurrently on multi-core architectures.

---

### (b) Comparison of ULT, KLT, and Combined ULT/KLT Approaches

| Feature | User-Level Threads (ULT) | Kernel-Level Threads (KLT) | Combined Approach (Hybrid) |
| :--- | :--- | :--- | :--- |
| **Management** | Managed by user-space thread libraries (e.g., POSIX threads). | Managed directly by the OS kernel. | Managed by user library mapped onto multiple kernel threads. |
| **Kernel Awareness** | Kernel sees the process as a single-threaded task. | Kernel is fully aware of and schedules each individual thread. | Kernel schedules kernel threads; user threads map onto them. |
| **Performance** | Fast thread creation and switching (no kernel mode transition). | Slower switching due to kernel intervention and mode switches. | Balanced performance and flexibility. |
| **Blocking Behavior** | If one thread performs a blocking system call, the *entire* process blocks. | If one thread blocks, the kernel can schedule another thread of the same process. | Prevents blocking entire process while maintaining speed. |

---

### (c) Multithreading Models & Diagram

There are three primary relationships between User Threads and Kernel Threads:
1. **Many-to-One Model:** Many user-level threads mapped to a single kernel thread. Efficient, but unable to take advantage of multi-core processors.
2. **One-to-One Model:** Each user thread maps to a kernel thread. Provides maximum concurrency, but creates overhead on kernel resource management.
3. **Many-to-Many Model:** Multiplexes many user threads onto a smaller or equal number of kernel threads.

#### ASCII Diagram of Many-to-Many Multithreading Model:
```text
   User Space Threads          Kernel Space Threads
   
      [Thread 1]                 [Kernel Thread A]
      [Thread 2]        \           /
      [Thread 3]         +---------+      (Multiprocessor Hardware)
      [Thread 4]         +---------+        Core 1   |   Core 2
      [Thread 5]        /           \     [Kernel Thread B]
```

---

## Question 3: Virtual Memory, TLB, and Paging

### (a) Virtual Memory & TLB Operation
* **Virtual Memory:** A memory management technique that decouples logical user memory from physical RAM, allowing execution of processes larger than physical memory by swapping pages between disk and RAM.
* **Translation Lookaside Buffer (TLB):** A high-speed associative hardware cache inside the CPU memory management unit (MMU) that stores recent virtual-to-physical address translations to speed up memory access.

#### TLB Activity Diagram:
```text
CPU Generates Virtual Address (Page #, Offset)
                      |
                      v
             +-----------------+
             |   TLB Search    |
             +--------+--------+
                      |
         +------------+------------+
         | Hit                     | Miss
         v                         v
  Extract Physical Frame #   Access Page Table in RAM
         |                         |
         +------------+------------+
                      |
                      v
       Form Final Physical Address in RAM
```

---

### (b) Page Fetch & Replacement Policies
* **Page Fetch Policy:** Determines *when* a page should be brought into main memory.
  * **Demand Paging:** Pages are loaded only when explicitly referenced (faulted).
  * **Anticipatory / Pre-paging:** Predicting future references and loading multiple pages in advance to reduce page faults.
* **Replacement Policy:** Determines *which* resident page to evict when a page fault occurs and physical memory is full.
  * **FIFO (First-In, First-Out):** Evicts the oldest loaded page.
  * **LRU (Least Recently Used):** Evicts the page that hasn't been used for the longest period.
  * **Optimal:** Evicts the page that will not be used for the longest time in the future (theoretical best).

---

## Question 4: Memory Management (Partitioning, Paging, and Segmentation)

### (a) Buddy System & Physical Memory Partitioning
* **Buddy System:** A memory allocation algorithm that divides memory into partitions of sizes that are powers of 2 to satisfy allocation requests efficiently and merge adjacent free blocks (buddies) upon deallocation to eliminate external fragmentation.

#### Physical Memory Partitioning Types:
1. **Fixed (Static) Partitioning:** Memory is divided into fixed-size partitions at system boot time. Leads to **internal fragmentation**.
2. **Dynamic Partitioning:** Partitions are created dynamically to precisely fit arriving processes. Leads to **external fragmentation** over time, requiring compaction.

---

### (b) Address Translation & Paging vs. Segmentation

#### Address Translation Architecture in Paging and Segmentation:
* **Paging:** Logical address is split into **Page Number ($p$)** and **Offset ($d$)**. The Page Number indexes into a Page Table to retrieve the corresponding Frame Number ($f$), which is combined with the offset to form the physical address.
* **Segmentation:** Logical address consists of a **Segment Number ($s$)** and **Offset ($d$)**. The Segment Number indexes into a Segment Table to check base address and limit bounds, yielding the physical address ($\text{Base}_s + d$).

#### Difference Between Paging and Segmentation:

| Feature | Paging | Segmentation |
| :--- | :--- | :--- |
| **View of Memory** | Divided into fixed-size blocks (pages and frames). | Divided into variable-sized logical units (functions, data, stacks). |
| **Programmer Visibility** | Invisible to the programmer (managed entirely by hardware/OS). | Visible to the programmer (reflects logical program structure). |
| **Fragmentation** | Causes **Internal Fragmentation**. | Causes **External Fragmentation**. |
| **Protection & Sharing** | Harder to protect specific logical routines independently. | Excellent for protecting and sharing distinct subroutines or modules. |