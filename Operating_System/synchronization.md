# Chapter 7: 🔒 Synchronization in Operating Systems

Synchronization is a fundamental concept in concurrent programming and operating systems. It ensures that multiple processes or threads can operate safely and efficiently when sharing data and resources. This chapter explores the critical section problem, software and hardware solutions, synchronization tools, and classical synchronization problems in detail.

---

## 7.1 ✢ Critical Section Problem

### Definition:

A **Critical Section** is a part of the program where shared resources (like variables, files, or data structures) are accessed. If multiple processes enter the critical section simultaneously, it may lead to **race conditions**, data inconsistency, or corruption.

### The Critical Section Problem:

Design a protocol that ensures that **only one process at a time** can execute its critical section among several competing processes.

---

## 7.2 ⚖️ Requirements for Synchronization

Any correct solution to the critical section problem must satisfy the following three conditions:

1. **Mutual Exclusion**:

   * At any given time, only one process is allowed in the critical section.

2. **Progress**:

   * If no process is in the critical section and one or more processes want to enter, one of them must be allowed to proceed (no indefinite postponement).

3. **Bounded Waiting**:

   * There must be a bound on the number of times other processes can enter the critical section before a waiting process is granted access.

---

## 7.3 💻 Software Solutions

### 7.3.1 Peterson’s Algorithm

A classical solution for two-process synchronization using shared memory.

#### Assumptions:

* Two processes, P0 and P1
* Shared variables:

  * `bool flag[2]` initialized to false
  * `int turn` initialized arbitrarily

#### Pseudocode:

```cpp
// For process Pi (i = 0 or 1)
flag[i] = true;
turn = j;
while (flag[j] == true && turn == j) {
    // wait
}
// critical section
...
flag[i] = false;
```

### 7.3.2 Bakery Algorithm (Lamport)

General solution for multiple processes using ticket-based mechanism.

#### Idea:

* Each process takes a number (like a bakery token)
* Process with the smallest number enters the critical section

#### Pseudocode:

```cpp
choosing[i] = true;
number[i] = 1 + max(number[0..n-1]);
choosing[i] = false;

for (j = 0; j < n; j++) {
    while (choosing[j]) {}
    while (number[j] != 0 && (number[j], j) < (number[i], i)) {}
}

// critical section
...
number[i] = 0;
```

---

## 7.4 🔧 Hardware Solutions

### 7.4.1 Test-and-Set Instruction

An atomic operation that checks and modifies a variable in one go.

```cpp
bool TestAndSet(bool *target) {
    bool rv = *target;
    *target = true;
    return rv;
}
```

### 7.4.2 Compare-and-Swap Instruction

Atomic instruction used in lock-free programming.

```cpp
bool CompareAndSwap(int *reg, int expected, int new_val) {
    if (*reg == expected) {
        *reg = new_val;
        return true;
    } else {
        return false;
    }
}
```

---

## 7.5 ⚖️ Synchronization Tools

### 7.5.1 Mutex Locks

* A **mutex (mutual exclusion)** lock is used to protect critical sections.
* Only one thread can acquire the lock at a time.

#### Pseudocode:

```cpp
mutex lock;

lock();
// critical section
unlock();
```

### 7.5.2 Semaphores

A **semaphore** is a signaling mechanism used to control access.

#### Binary Semaphore:

```cpp
semaphore s = 1;

wait(s);
// critical section
signal(s);
```

#### Counting Semaphore:

```cpp
semaphore s = N; // N = number of resources

wait(s); // acquire resource
// critical section
signal(s); // release resource
```

### 7.5.3 Condition Variables

* Used to wait for certain conditions to become true.
* Always used with a mutex.

#### Pseudocode:

```cpp
mutex m;
condition_variable cv;

lock(m);
while (!condition) {
    wait(cv, m);
}
// critical section
unlock(m);
```

### 7.5.4 Monitors

* A high-level abstraction combining mutual exclusion and condition synchronization.

#### Example (pseudo-Java):

```java
monitor BoundedBuffer {
    condition notFull, notEmpty;
    buffer b;

    procedure insert(item) {
        if (b.isFull()) wait(notFull);
        b.add(item);
        signal(notEmpty);
    }

    procedure remove() {
        if (b.isEmpty()) wait(notEmpty);
        item = b.remove();
        signal(notFull);
        return item;
    }
}
```

---

## 7.6 ⚔️ Classical Synchronization Problems

### 7.6.1 Producer-Consumer Problem

#### Description:

* Producer adds items to a buffer.
* Consumer removes items from buffer.
* Need to prevent producer from adding to a full buffer and consumer from removing from an empty buffer.

#### Pseudocode (Using Semaphore):

```cpp
semaphore full = 0, empty = N, mutex = 1;

Producer:
while (true) {
    produce_item();
    wait(empty);
    wait(mutex);
    insert_item();
    signal(mutex);
    signal(full);
}

Consumer:
while (true) {
    wait(full);
    wait(mutex);
    remove_item();
    signal(mutex);
    signal(empty);
    consume_item();
}
```

---

### 7.6.2 Dining Philosophers Problem

#### Description:

* Each philosopher alternates between thinking and eating.
* Must pick up 2 forks (shared with neighbors) to eat.

#### Pseudocode (Naive):

```cpp
while (true) {
    think();
    wait(fork[i]);
    wait(fork[(i+1)%N]);
    eat();
    signal(fork[i]);
    signal(fork[(i+1)%N]);
}
```

#### Problem:

* Can lead to **deadlock** if all pick left fork simultaneously.

#### Solution:

* Allow at most N-1 philosophers to try eating at the same time.

---

### 7.6.3 Reader-Writer Problem

#### Description:

* Multiple readers can read simultaneously.
* Writers need exclusive access.

#### Pseudocode (First Readers Preference):

```cpp
semaphore mutex = 1, wrt = 1;
int readcount = 0;

Reader:
wait(mutex);
readcount++;
if (readcount == 1)
    wait(wrt);
signal(mutex);

// reading

wait(mutex);
readcount--;
if (readcount == 0)
    signal(wrt);
signal(mutex);

Writer:
wait(wrt);
// writing
signal(wrt);
```

---

### 7.6.4 Sleeping Barber Problem

#### Description:

* Barber sleeps if no customers.
* Customers either wake the barber or wait.

#### Pseudocode:

```cpp
semaphore customers = 0;
semaphore barber = 0;
semaphore accessSeats = 1;
int freeSeats = N;

Barber:
while (true) {
    wait(customers);
    wait(accessSeats);
    freeSeats++;
    signal(barber);
    signal(accessSeats);
    cutHair();
}

Customer:
wait(accessSeats);
if (freeSeats > 0) {
    freeSeats--;
    signal(customers);
    signal(accessSeats);
    wait(barber);
    getHaircut();
} else {
    signal(accessSeats);
    leaveShop();
}
```

---

> These concepts are fundamental for understanding concurrency control in modern operating systems. Proper use of synchronization prevents race conditions, ensures consistency, and enables safe multi-threaded program execution.
