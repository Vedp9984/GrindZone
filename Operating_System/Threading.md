# Threads & Multithreading - Comprehensive Notes

## 1. Thread vs Process

### Process
- **Definition**: A process is an independent program in execution with its own memory space
- **Characteristics**:
  - Has its own address space (memory)
  - Contains program code, data, heap, and stack
  - Processes are isolated from each other
  - Communication between processes requires IPC (Inter-Process Communication)
  - Creating a process is expensive (high overhead)
  - Context switching between processes is costly

### Thread
- **Definition**: A thread is a lightweight unit of execution within a process
- **Characteristics**:
  - Shares the same address space with other threads in the same process
  - Has its own stack and register set
  - Shares code, data, and heap with other threads
  - Communication between threads is easier (shared memory)
  - Creating a thread is less expensive than creating a process
  - Context switching between threads is faster

### Key Differences Summary
| Aspect | Process | Thread |
|--------|---------|--------|
| Memory | Separate address space | Shared address space |
| Creation Cost | High | Low |
| Context Switch | Expensive | Cheap |
| Communication | IPC required | Shared memory |
| Independence | Fully independent | Dependent on process |

## 2. Advantages of Multithreading

### Performance Benefits
- **Parallelism**: Multiple threads can execute simultaneously on multi-core systems
- **Concurrency**: Even on single-core systems, threads can provide better resource utilization
- **Responsiveness**: User interface remains responsive while background tasks execute
- **Faster Context Switching**: Thread switching is faster than process switching

### Resource Benefits
- **Resource Sharing**: Threads share memory, files, and other resources efficiently
- **Memory Efficiency**: Less memory overhead compared to multiple processes
- **Economy**: Thread creation and management is less expensive

### Application Benefits
- **Modularity**: Different parts of an application can run in separate threads
- **Scalability**: Applications can scale better with available hardware resources

## 3. Types of Threads

### User-Level Threads (ULT)
- **Definition**: Threads managed entirely by user-space thread libraries without kernel involvement

#### Characteristics:
- Kernel is unaware of thread existence
- All thread management is done in user space
- Fast thread creation, switching, and synchronization
- No system calls required for thread operations

#### Advantages:
- Very fast thread switching (no kernel involvement)
- Can be implemented on any operating system
- Application-specific thread scheduling possible
- Low overhead

#### Disadvantages:
- If one thread blocks, entire process blocks
- Cannot take advantage of multiprocessor systems
- Kernel scheduling may not be optimal for threads

### Kernel-Level Threads (KLT)
- **Definition**: Threads managed directly by the operating system kernel

#### Characteristics:
- Kernel maintains thread information
- System calls required for thread operations
- Kernel schedules threads individually
- Slower thread operations due to kernel involvement

#### Advantages:
- If one thread blocks, others can continue execution
- Can utilize multiple processors effectively
- Kernel can make better scheduling decisions
- Better for I/O-intensive applications

#### Disadvantages:
- Slower thread creation and switching
- Higher overhead due to system calls
- Limited by kernel thread implementation

## 4. Multithreading Models

### Many-to-One Model
- **Structure**: Many user-level threads mapped to one kernel thread
- **Implementation**: Pure user-level threading

#### Characteristics:
- All threads in a process share one kernel thread
- Thread management is done entirely in user space
- Examples: Green threads in early Java implementations

#### Advantages:
- Efficient thread management
- Fast thread operations
- Portable across different operating systems

#### Disadvantages:
- If one thread makes a blocking system call, all threads block
- Cannot run in parallel on multiprocessor systems
- Poor scalability

### One-to-One Model
- **Structure**: Each user thread mapped to one kernel thread
- **Implementation**: Direct mapping to kernel threads

#### Characteristics:
- Each user thread has a corresponding kernel thread
- Thread operations require system calls
- Examples: Windows threads, Linux pthreads

#### Advantages:
- If one thread blocks, others can continue
- Can run in parallel on multiprocessor systems
- Better concurrency

#### Disadvantages:
- Higher overhead for thread creation and management
- Limited by kernel thread limits
- Slower thread operations

### Many-to-Many Model
- **Structure**: Many user threads mapped to many kernel threads
- **Implementation**: Hybrid approach combining benefits of both models

#### Characteristics:
- Multiple user threads multiplexed onto smaller number of kernel threads
- Number of kernel threads can be adjusted based on application needs
- Examples: Solaris threads (prior to Solaris 9)

#### Advantages:
- Combines benefits of both models
- Can create as many user threads as needed
- Good performance and concurrency
- Flexibility in thread scheduling

#### Disadvantages:
- Complex implementation
- Difficult to manage thread mappings
- Less commonly used in modern systems

## 5. Thread Lifecycle

### Thread States
1. **New/Created**: Thread object created but not yet started
2. **Runnable/Ready**: Thread is ready to run and waiting for CPU
3. **Running**: Thread is currently executing on CPU
4. **Blocked/Waiting**: Thread is waiting for some condition or resource
5. **Terminated/Dead**: Thread has completed execution

### State Transitions
```
New → Runnable → Running → Terminated
         ↕         ↕
      Blocked ←――――――→
```

#### Detailed State Descriptions:
- **New**: Thread created using thread creation API
- **Runnable**: Thread ready to execute, waiting for CPU allocation
- **Running**: Thread actively executing instructions
- **Blocked**: Thread waiting for I/O, synchronization, or other resources
- **Terminated**: Thread execution completed or terminated

## 6. Thread Libraries

### POSIX Pthreads (POSIX Threads)
- **Standard**: IEEE POSIX 1003.1c standard for thread programming
- **Availability**: Unix, Linux, and other POSIX-compliant systems

#### Key Functions:
- `pthread_create()`: Create a new thread
- `pthread_join()`: Wait for thread termination
- `pthread_exit()`: Terminate calling thread
- `pthread_cancel()`: Cancel a thread
- `pthread_mutex_*()`: Mutex operations
- `pthread_cond_*()`: Condition variable operations

#### Example Usage:
```c
#include <pthread.h>

void* thread_function(void* arg) {
    // Thread code here
    return NULL;
}

int main() {
    pthread_t thread_id;
    pthread_create(&thread_id, NULL, thread_function, NULL);
    pthread_join(thread_id, NULL);
    return 0;
}
```

### Other Thread Libraries:
- **Windows Threads**: Win32 API threading
- **Java Threads**: Built-in thread support in Java
- **C++ Threads**: std::thread in C++11 and later

## 7. Thread Synchronization

### Purpose
- Coordinate access to shared resources
- Ensure data consistency
- Prevent race conditions
- Maintain program correctness

### Synchronization Mechanisms

#### Mutex (Mutual Exclusion)
- **Purpose**: Ensures only one thread can access a resource at a time
- **Usage**: Lock before accessing shared resource, unlock after use
- **Types**: Recursive, non-recursive, timed mutexes

#### Semaphores
- **Purpose**: Controls access to a resource with limited instances
- **Types**: Binary semaphore (0 or 1), Counting semaphore (0 to N)
- **Operations**: Wait/P (decrement), Signal/V (increment)

#### Condition Variables
- **Purpose**: Allow threads to wait for specific conditions
- **Usage**: Used with mutexes for efficient waiting
- **Operations**: Wait, Signal, Broadcast

#### Read-Write Locks
- **Purpose**: Allow multiple readers or single writer
- **Benefit**: Better performance for read-heavy workloads

#### Barriers
- **Purpose**: Synchronize multiple threads at a specific point
- **Usage**: All threads must reach barrier before any can proceed

## 8. Issues in Multithreading

### Race Conditions
- **Definition**: Occurs when multiple threads access shared data simultaneously and the outcome depends on timing

#### Characteristics:
- Results are unpredictable and non-deterministic
- Difficult to debug and reproduce
- Can lead to data corruption

#### Example Scenario:
```
Thread 1: Read balance (100)
Thread 2: Read balance (100)
Thread 1: Add 50 → Write 150
Thread 2: Add 30 → Write 130
Final balance: 130 (should be 180)
```

#### Prevention:
- Use synchronization mechanisms (mutexes, semaphores)
- Atomic operations
- Proper critical section management

### Deadlocks
- **Definition**: Situation where two or more threads are permanently blocked, waiting for each other

#### Necessary Conditions (Coffman Conditions):
1. **Mutual Exclusion**: Resources cannot be shared
2. **Hold and Wait**: Threads hold resources while waiting for others
3. **No Preemption**: Resources cannot be forcibly taken
4. **Circular Wait**: Circular chain of threads waiting for resources

#### Example:
```
Thread A: Locks Resource 1, waits for Resource 2
Thread B: Locks Resource 2, waits for Resource 1
```

#### Prevention Strategies:
- **Avoid Circular Wait**: Order resources and acquire in same order
- **Timeout**: Use timeouts when acquiring locks
- **Deadlock Detection**: Monitor and detect deadlock conditions
- **Banker's Algorithm**: Ensure safe resource allocation

#### Recovery:
- Terminate one or more threads
- Rollback thread operations
- Preempt resources from threads

### Starvation
- **Definition**: Situation where a thread is perpetually denied access to resources

#### Causes:
- Unfair scheduling algorithms
- Priority inversion
- Poorly designed synchronization
- High-priority threads monopolizing resources

#### Types:
- **Resource Starvation**: Cannot acquire needed resources
- **CPU Starvation**: Never gets CPU time
- **Lock Starvation**: Cannot acquire locks

#### Prevention:
- Fair scheduling algorithms
- Priority inheritance protocols
- Aging mechanisms (gradually increase priority)
- Fair synchronization primitives

## Key Takeaways

1. **Threads are lighter than processes** and share memory space within a process
2. **Multithreading provides concurrency** and can improve application performance
3. **Different threading models** (Many-to-One, One-to-One, Many-to-Many) have different trade-offs
4. **Proper synchronization is crucial** to prevent race conditions and ensure data integrity
5. **Deadlocks, race conditions, and starvation** are common issues that require careful design to avoid
6. **Thread libraries like pthreads** provide standardized APIs for thread programming
7. **Understanding thread lifecycle** helps in designing efficient multithreaded applications

## Best Practices

1. **Minimize shared data** between threads
2. **Use appropriate synchronization** mechanisms
3. **Avoid nested locks** to prevent deadlocks
4. **Design for thread safety** from the beginning
5. **Test thoroughly** with multiple threads and different timing scenarios
6. **Use thread-safe libraries** when available
7. **Monitor performance** to ensure multithreading provides benefits