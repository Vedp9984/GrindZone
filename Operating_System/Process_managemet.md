## 1. 🧵 Process Management

Process Management is a fundamental concept in Operating Systems which involves the creation, scheduling, execution, and termination of processes. A **process** is an instance of a program in execution.

---

### 🔸 Difference between Process and Program

| Aspect      | Program                                  | Process                                 |
|-------------|------------------------------------------|------------------------------------------|
| Definition  | A passive set of instructions            | An active instance of a program          |
| Stored in   | Disk                                     | Main Memory (RAM)                        |
| Execution   | Not executed, only stored                | Actively executed by CPU                |
| Lifespan    | Long (until deleted)                     | Temporary (exists during execution)      |
| Example     | A `.c` source file                       | The running compiled code (`a.out`)      |

---

### 🔸 Process States

A process goes through various states during its lifecycle:

1. **New** – Process is being created.
2. **Ready** – Waiting to be assigned to a processor.
3. **Running** – Instructions are being executed.
4. **Waiting** – Waiting for an event (I/O).
5. **Terminated** – Process has finished execution.

> OS uses state transition diagrams to manage these transitions.

---

### 🔸 Process Control Block (PCB)

A PCB is a data structure maintained by the OS to keep information about each process.

Contents of PCB:
- Process ID (PID)
- Process state
- Program counter
- CPU registers
- Memory management info
- Accounting info (CPU used, time limits)
- I/O status

---
### 🔸 Process Lifecycle & State Transitions

```
  +--------+         +-------+
  |  New   | ----->  | Ready |
  +--------+         +-------+
            |
            v
        +--------+      I/O wait
        | Running|------------+
        +--------+            |
            |                 v
      +------------+      +--------+
      | Terminated |<-----| Waiting|
      +------------+      +--------+
```

---

### 🔸 Context Switching

Context Switching is the process of storing the state of a currently running process so that it can be resumed later, and loading the state of another process.

Involves saving:
- CPU registers
- Program counter
- Stack pointer

> Context switching introduces overhead (non-productive time).

---

### 🔸 Process Creation & Termination

#### 🔹 fork()
- Used to create a new process (child).
- Returns:
  - 0 to the child process
  - PID of child to the parent process

#### 🔹 exec()
- Replaces the current process image with a new process image.
- Often used after fork() to load a new program.

#### 🔹 exit()
- Terminates the process and returns status to the parent.
- Kernel releases resources (memory, I/O).

---

### 🔸 Types of Processes

- **Foreground Process**: Requires user interaction (e.g., text editor).
- **Background Process**: Runs in the background without user interaction (e.g., daemons).
- **Zombie Process**:
  - A process that has completed but still has an entry in the process table.
  - Occurs if the parent hasn't read its exit status using wait().
- **Orphan Process**:
  - A child process whose parent has terminated.
  - Adopted by init process (PID 1) in Unix systems.

---

### 🔸 Interprocess Communication (IPC)

IPC is a mechanism that allows processes to communicate and synchronize their actions.

#### 🔹 Pipes
- **Anonymous Pipes**:
  - Unidirectional.
  - Used between parent and child processes.
  - It connects the stdout (standard output) of the command on the left to the stdin (standard input) of the command on the right.
- **Named Pipes (FIFOs)**:
  - Exist as a file.
  - Allow communication between unrelated processes.

#### 🔹 Message Passing
- Processes communicate by sending and receiving messages.
- Can be:
  - Direct (name of sender/receiver known)
  - Indirect (messages through a mailbox)

#### 🔹 Shared Memory
- Two or more processes share a portion of memory.
- Fastest IPC method.
- Requires synchronization mechanisms like semaphores to avoid race conditions.

#### 🔹 Sockets
- Endpoints for communication between processes over a network.
- Useful for client-server architecture.
- Can be TCP (connection-oriented) or UDP (connectionless).

---

### 🔍 Important Interview Questions – Process Management

1. What is the difference between a process and a program?
2. Explain the five states of a process with a diagram.
3. What is a Process Control Block and what does it contain?
4. How does fork() work? What is returned in parent and child?
5. What is the purpose of exec() in process management?
6. What is a zombie process? How can it be avoided?
7. How does context switching work? What overheads does it introduce?
8. What are the differences between foreground and background processes?
9. How does interprocess communication work in Unix?
10. Compare shared memory and message passing.
11. What happens to orphan processes in Unix/Linux?
12. How do named pipes differ from anonymous pipes?
13. When and why do we use sockets for IPC?
