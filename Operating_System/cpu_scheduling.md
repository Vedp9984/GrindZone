# CPU Scheduling - Comprehensive Study Notes

## Table of Contents
1. [Introduction to CPU Scheduling](#introduction)
2. [Scheduling Criteria](#scheduling-criteria)
3. [Types of Scheduling](#types-of-scheduling)
4. [Scheduling Algorithms](#scheduling-algorithms)
5. [Important Concepts](#important-concepts)
6. [Performance Analysis](#performance-analysis)

---

## Introduction to CPU Scheduling {#introduction}

CPU scheduling is the process of determining which process should be allocated the CPU when multiple processes are ready to execute. The CPU scheduler (also called short-term scheduler) selects a process from the ready queue and allocates the CPU to it.

**Why CPU Scheduling is Important:**
- Maximizes CPU utilization
- Ensures fair allocation of CPU time
- Minimizes response time for interactive processes
- Optimizes system throughput

---

## Scheduling Criteria {#scheduling-criteria}

### 1. CPU Utilization
- **Definition:** Percentage of time the CPU is busy executing processes
- **Goal:** Keep CPU as busy as possible (ideally 40-90%)
- **Formula:** (Total execution time / Total time) × 100

### 2. Throughput
- **Definition:** Number of processes completed per unit time
- **Measurement:** Processes/second, processes/minute, etc.
- **Higher throughput indicates better system performance**

### 3. Turnaround Time
- **Definition:** Total time taken from process submission to completion
- **Formula:** Turnaround Time = Completion Time - Arrival Time
- **Includes:** Waiting time + Execution time + I/O time

### 4. Waiting Time
- **Definition:** Total time a process spends waiting in the ready queue
- **Formula:** Waiting Time = Turnaround Time - Burst Time
- **Goal:** Minimize average waiting time

### 5. Response Time
- **Definition:** Time from request submission until first response is produced
- **Formula:** Response Time = Time of First Response - Arrival Time
- **Critical for:** Interactive systems and real-time applications

---

## Types of Scheduling {#types-of-scheduling}

### Preemptive Scheduling
- **Definition:** CPU can be taken away from a currently executing process
- **Characteristics:**
  - Process can be interrupted during execution
  - Higher overhead due to context switching
  - Better response time for high-priority processes
  - Prevents process monopolization of CPU

**Examples:** Round Robin, Preemptive SJF, Preemptive Priority Scheduling

### Non-Preemptive Scheduling
- **Definition:** Once CPU is allocated, process runs until completion or voluntary release
- **Characteristics:**
  - Lower overhead (fewer context switches)
  - Process runs to completion once started
  - May lead to poor response time
  - Simpler to implement

**Examples:** FCFS, Non-preemptive SJF, Non-preemptive Priority Scheduling

---

## Scheduling Algorithms {#scheduling-algorithms}

### 1. FCFS (First-Come, First-Served)

**Characteristics:**
- Non-preemptive algorithm
- Processes are executed in order of arrival
- Simple to understand and implement

**Advantages:**
- Fair in terms of arrival order
- No starvation
- Simple implementation

**Disadvantages:**
- Poor average waiting time
- Convoy effect (short processes wait for long processes)
- Not suitable for time-sharing systems

**Example:**
```
Process | Arrival Time | Burst Time
P1      | 0           | 8
P2      | 1           | 4
P3      | 2           | 2

Execution Order: P1 → P2 → P3
Gantt Chart: |P1(0-8)|P2(8-12)|P3(12-14)|
```

### 2. SJF (Shortest Job First)

**Characteristics:**
- Selects process with smallest burst time
- Can be preemptive (SRTF) or non-preemptive
- Optimal for minimizing average waiting time

**Non-Preemptive SJF:**
- Once started, process runs to completion
- New arrivals wait until current process finishes

**Preemptive SJF (SRTF - Shortest Remaining Time First):**
- If new process has shorter remaining time, preempt current process
- Continuously checks for shorter jobs

**Advantages:**
- Minimum average waiting time
- Good throughput

**Disadvantages:**
- Starvation of long processes
- Difficult to predict burst time
- Not practical for interactive systems

### 3. Round Robin (RR)

**Characteristics:**
- Preemptive algorithm
- Each process gets fixed time slice (time quantum)
- After time quantum expires, process goes to end of ready queue

**Time Quantum Selection:**
- Too small: High overhead due to frequent context switching
- Too large: Degenerates to FCFS
- Typical range: 10-100 milliseconds

**Advantages:**
- Fair allocation of CPU time
- Good response time
- No starvation
- Suitable for time-sharing systems

**Disadvantages:**
- Higher average turnaround time than SJF
- Context switching overhead
- Performance depends on time quantum size

**Example:**
```
Process | Arrival Time | Burst Time
P1      | 0           | 10
P2      | 1           | 4
P3      | 2           | 6

Time Quantum = 3
Gantt Chart: |P1(0-3)|P2(3-6)|P3(6-9)|P1(9-12)|P2(12-13)|P3(13-16)|P1(16-19)|
```

### 4. Priority Scheduling

**Characteristics:**
- Each process assigned a priority number
- CPU allocated to highest priority process
- Can be preemptive or non-preemptive

**Priority Assignment:**
- Lower number = Higher priority (or vice versa)
- Internal priorities: Based on measurable quantities (time limits, memory requirements)
- External priorities: Set by factors outside OS (importance, payment)

**Preemptive Priority:**
- Higher priority process can preempt lower priority process
- New high-priority arrival can interrupt current execution

**Non-Preemptive Priority:**
- Once CPU allocated, process runs to completion
- Priority checked only when CPU becomes free

**Advantages:**
- Important processes get priority
- Flexible priority assignment
- Good for real-time systems

**Disadvantages:**
- Starvation of low-priority processes
- Priority inversion problem
- Complexity in priority assignment

### 5. Multilevel Queue Scheduling

**Characteristics:**
- Ready queue divided into separate queues
- Each queue has its own scheduling algorithm
- Processes permanently assigned to queues based on properties

**Common Queue Categories:**
1. **System processes** (highest priority)
2. **Interactive processes**
3. **Interactive editing processes**
4. **Batch processes**
5. **Student processes** (lowest priority)

**Scheduling Between Queues:**
- Fixed priority preemptive scheduling
- Time slice allocation to each queue

**Advantages:**
- Different algorithms for different process types
- Clear separation of process types
- Efficient for systems with distinct process categories

**Disadvantages:**
- Starvation of lower-level queues
- Lack of flexibility (processes can't move between queues)
- Complex implementation

### 6. Multilevel Feedback Queue

**Characteristics:**
- Similar to multilevel queue but processes can move between queues
- Process behavior determines queue level
- Aging mechanism prevents starvation

**Common Implementation:**
- Multiple queues with different priorities
- New processes start at highest priority queue
- Process demoted if it uses full time quantum
- Process promoted if it becomes interactive

**Parameters:**
- Number of queues
- Scheduling algorithm for each queue
- Method to determine when to upgrade/demote process
- Method to determine initial queue assignment

**Advantages:**
- Adaptive to process behavior
- Prevents starvation through aging
- Gives preference to short and interactive processes
- Most flexible scheduling algorithm

**Disadvantages:**
- Most complex to implement
- High overhead due to queue management
- Difficult to tune parameters

---

## Important Concepts {#important-concepts}

### Gantt Charts

**Definition:** Visual representation of process execution timeline

**Components:**
- Time axis (horizontal)
- Process names
- Execution periods
- Context switches

**Uses:**
- Visualize scheduling algorithm execution
- Calculate waiting and turnaround times
- Analyze CPU utilization

**Example Gantt Chart:**
```
Time:    0    3    6    9    12   15
        |P1  |P2  |P3  |P1  |P2  |
```

### Waiting Time and Turnaround Time Calculation

**Formulas:**
- **Turnaround Time = Completion Time - Arrival Time**
- **Waiting Time = Turnaround Time - Burst Time**
- **Response Time = Time of First Response - Arrival Time**

**Calculation Steps:**
1. Create Gantt chart for the scheduling algorithm
2. Identify completion time for each process
3. Calculate turnaround time for each process
4. Calculate waiting time for each process
5. Find average turnaround time and average waiting time

**Example Calculation:**
```
Process | Arrival | Burst | Completion | Turnaround | Waiting
P1      | 0      | 5     | 5          | 5          | 0
P2      | 2      | 3     | 8          | 6          | 3
P3      | 4      | 1     | 9          | 5          | 4

Average Turnaround Time = (5 + 6 + 5) / 3 = 5.33
Average Waiting Time = (0 + 3 + 4) / 3 = 2.33
```

### Starvation and Aging

#### Starvation
**Definition:** Indefinite postponement of process execution

**Causes:**
- Low-priority processes in priority scheduling
- Long processes in SJF scheduling
- Poor resource allocation

**Effects:**
- Process never gets CPU time
- System appears unresponsive
- Unfair resource distribution

#### Aging
**Definition:** Technique to prevent starvation by gradually increasing process priority

**Implementation:**
- Increase priority of waiting processes over time
- Processes that wait longer get higher priority
- Eventually, even low-priority processes get CPU time

**Benefits:**
- Prevents indefinite postponement
- Ensures fairness
- Maintains system responsiveness

**Example Aging Algorithm:**
```
Every minute:
    For each process in ready queue:
        priority = priority - 1  // Assuming lower number = higher priority
```

---

## Performance Analysis {#performance-analysis}

### Metrics Comparison

| Algorithm | Average Waiting Time | Response Time | Throughput | Starvation Risk |
|-----------|---------------------|---------------|------------|-----------------|
| FCFS      | Poor               | Poor          | Good       | No              |
| SJF       | Optimal            | Good          | Excellent  | Yes (long jobs) |
| Round Robin| Fair              | Good          | Good       | No              |
| Priority  | Variable           | Good          | Good       | Yes (low priority)|
| Multilevel| Good               | Excellent     | Good       | Possible        |
| MLFQ      | Good               | Excellent     | Excellent  | No (with aging) |

### Algorithm Selection Guidelines

**Choose FCFS when:**
- Simple system with minimal requirements
- Batch processing systems
- No preemption required

**Choose SJF when:**
- Batch systems with known execution times
- Minimizing average waiting time is critical
- Long-term scheduling

**Choose Round Robin when:**
- Time-sharing systems
- Interactive systems
- Fair CPU allocation required

**Choose Priority Scheduling when:**
- Real-time systems
- Different importance levels exist
- System processes need precedence

**Choose Multilevel Queue when:**
- Clear process categories exist
- Different scheduling needs for different types
- System resources are well-defined

**Choose MLFQ when:**
- General-purpose systems
- Mixed workload (interactive + batch)
- Adaptive behavior required
- Highest flexibility needed

---
