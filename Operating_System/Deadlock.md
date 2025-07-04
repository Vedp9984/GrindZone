# Complete Guide to Deadlocks in Operating Systems

## What is a Deadlock?

A **deadlock** is a situation where two or more processes are blocked forever, waiting for each other to release resources. It's like a traffic jam where cars are stuck in a circle, each waiting for the other to move first.

## The Four Necessary Conditions for Deadlock

For a deadlock to occur, ALL four conditions must be present simultaneously:

### 1. Mutual Exclusion
- Only one process can use a resource at any given time
- Resources cannot be shared simultaneously
- Example: A printer can only print one job at a time

### 2. Hold and Wait
- A process holding at least one resource is waiting to acquire additional resources held by other processes
- Process doesn't release its current resources while waiting
- Example: Process A holds Resource 1 and waits for Resource 2

### 3. No Preemption
- Resources cannot be forcibly taken away from processes
- Resources can only be released voluntarily by the process holding them
- Example: You can't force a process to give up a file it has open

### 4. Circular Wait
- There exists a circular chain of processes where each process waits for a resource held by the next process in the chain
- Example: P1 → P2 → P3 → P1 (forms a circle)

## Resource Allocation Graph (RAG)

A RAG is a directed graph used to represent resource allocation and requests in a system.

### Components:
- **Process nodes:** Represented by circles (P1, P2, P3, ...)
- **Resource nodes:** Represented by rectangles (R1, R2, R3, ...)
- **Request edges:** Process → Resource (process requests resource)
- **Assignment edges:** Resource → Process (resource assigned to process)

### Example RAG:
```
P1 ──requests──→ R1
 ↑                ↓
 │              assigned
 │                ↓
R2 ←──assigned── P2
 ↑                ↓
 │              requests
 │                ↓
P3 ←──assigned── R2
```

### Detecting Deadlock in RAG:
- **Single instance resources:** Deadlock exists if there's a cycle
- **Multiple instance resources:** Cycle is necessary but not sufficient

## Deadlock Handling Techniques

### 1. Prevention
Break at least one of the four necessary conditions:

#### a) Eliminate Mutual Exclusion
```python
# Example: Making resources shareable
class SharedResource:
    def __init__(self):
        self.readers = 0
        self.lock = threading.Lock()
    
    def read(self):
        with self.lock:
            self.readers += 1
            # Multiple readers can access simultaneously
            return "Reading data"
    
    def write(self):
        with self.lock:
            if self.readers == 0:
                # Only one writer at a time
                return "Writing data"
            else:
                return "Wait for readers to finish"
```

#### b) Eliminate Hold and Wait
```python
# Method 1: Request all resources at once
def process_with_all_resources():
    resources_needed = [resource1, resource2, resource3]
    
    # Try to acquire all resources at once
    if acquire_all_resources(resources_needed):
        # Do work with all resources
        perform_task()
        # Release all resources
        release_all_resources(resources_needed)
    else:
        # Wait and try again later
        retry_later()

# Method 2: Release all resources before requesting new ones
def process_with_release():
    acquire_resource(resource1)
    
    # Need resource2, so release resource1 first
    release_resource(resource1)
    
    # Now request both resources
    acquire_all_resources([resource1, resource2])
```

#### c) Allow Preemption
```python
class PreemptibleResource:
    def __init__(self):
        self.owner = None
        self.priority = 0
    
    def acquire(self, process, priority):
        if self.owner is None:
            self.owner = process
            self.priority = priority
        elif priority > self.priority:
            # Preempt lower priority process
            self.owner.save_state()
            self.owner = process
            self.priority = priority
        else:
            return False
        return True
```

#### d) Eliminate Circular Wait
```python
# Resource ordering: Always request resources in a fixed order
RESOURCE_ORDER = {'disk': 1, 'printer': 2, 'scanner': 3, 'network': 4}

def ordered_resource_request(process_id, resources):
    # Sort resources by their order number
    sorted_resources = sorted(resources, key=lambda r: RESOURCE_ORDER[r])
    
    acquired = []
    for resource in sorted_resources:
        if acquire_resource(resource):
            acquired.append(resource)
        else:
            # Release all acquired resources and fail
            for r in acquired:
                release_resource(r)
            return False
    
    return True
```

### 2. Avoidance - Banker's Algorithm

The Banker's Algorithm ensures the system never enters an unsafe state.

```python
class BankersAlgorithm:
    def __init__(self, processes, resources):
        self.processes = processes
        self.resources = resources
        self.allocation = [[0] * resources for _ in range(processes)]
        self.max_need = [[0] * resources for _ in range(processes)]
        self.available = [0] * resources
        self.need = [[0] * resources for _ in range(processes)]
    
    def set_max_need(self, process, resource, amount):
        self.max_need[process][resource] = amount
        self.need[process][resource] = amount - self.allocation[process][resource]
    
    def set_available(self, available):
        self.available = available[:]
    
    def is_safe_state(self):
        work = self.available[:]
        finish = [False] * self.processes
        safe_sequence = []
        
        while len(safe_sequence) < self.processes:
            found = False
            
            for i in range(self.processes):
                if not finish[i]:
                    # Check if process i can complete
                    can_complete = True
                    for j in range(self.resources):
                        if self.need[i][j] > work[j]:
                            can_complete = False
                            break
                    
                    if can_complete:
                        # Process i can complete
                        for j in range(self.resources):
                            work[j] += self.allocation[i][j]
                        finish[i] = True
                        safe_sequence.append(i)
                        found = True
                        break
            
            if not found:
                # No process can complete - unsafe state
                return False, []
        
        return True, safe_sequence
    
    def request_resources(self, process, request):
        # Check if request is valid
        for i in range(self.resources):
            if request[i] > self.need[process][i]:
                return False, "Request exceeds maximum need"
            if request[i] > self.available[i]:
                return False, "Request exceeds available resources"
        
        # Try granting the request
        for i in range(self.resources):
            self.available[i] -= request[i]
            self.allocation[process][i] += request[i]
            self.need[process][i] -= request[i]
        
        # Check if resulting state is safe
        is_safe, sequence = self.is_safe_state()
        
        if is_safe:
            return True, f"Request granted. Safe sequence: {sequence}"
        else:
            # Rollback the allocation
            for i in range(self.resources):
                self.available[i] += request[i]
                self.allocation[process][i] -= request[i]
                self.need[process][i] += request[i]
            return False, "Request would lead to unsafe state"

# Example usage
banker = BankersAlgorithm(processes=3, resources=3)

# Set maximum needs
banker.set_max_need(0, 0, 10)  # Process 0 needs max 10 of resource 0
banker.set_max_need(0, 1, 5)   # Process 0 needs max 5 of resource 1
banker.set_max_need(0, 2, 7)   # Process 0 needs max 7 of resource 2

# Set available resources
banker.set_available([3, 3, 2])

# Request resources
success, message = banker.request_resources(0, [1, 0, 2])
print(f"Request result: {success}, Message: {message}")
```

### 3. Detection

#### Using Resource Allocation Graph
```python
class DeadlockDetector:
    def __init__(self):
        self.graph = {}  # adjacency list
    
    def add_edge(self, from_node, to_node):
        if from_node not in self.graph:
            self.graph[from_node] = []
        self.graph[from_node].append(to_node)
    
    def has_cycle(self):
        """Detect cycle using DFS"""
        visited = set()
        rec_stack = set()
        
        def dfs(node):
            if node in rec_stack:
                return True  # Back edge found - cycle exists
            if node in visited:
                return False
            
            visited.add(node)
            rec_stack.add(node)
            
            for neighbor in self.graph.get(node, []):
                if dfs(neighbor):
                    return True
            
            rec_stack.remove(node)
            return False
        
        for node in self.graph:
            if node not in visited:
                if dfs(node):
                    return True
        return False

# Example usage
detector = DeadlockDetector()

# Build wait-for graph
detector.add_edge("P1", "P2")  # P1 waits for P2
detector.add_edge("P2", "P3")  # P2 waits for P3
detector.add_edge("P3", "P1")  # P3 waits for P1 (creates cycle)

if detector.has_cycle():
    print("Deadlock detected!")
else:
    print("No deadlock found")
```

#### Wait-for Graph Algorithm
```python
class WaitForGraph:
    def __init__(self):
        self.processes = set()
        self.resources = set()
        self.allocation = {}  # resource -> process
        self.request = {}     # process -> [resources]
        self.wait_for = {}    # process -> [processes]
    
    def allocate_resource(self, resource, process):
        self.resources.add(resource)
        self.processes.add(process)
        self.allocation[resource] = process
    
    def request_resource(self, process, resource):
        self.processes.add(process)
        self.resources.add(resource)
        
        if process not in self.request:
            self.request[process] = []
        self.request[process].append(resource)
    
    def build_wait_for_graph(self):
        """Build wait-for graph from allocation and request"""
        self.wait_for = {}
        
        for process in self.processes:
            if process not in self.wait_for:
                self.wait_for[process] = []
            
            # Check what resources this process is waiting for
            for resource in self.request.get(process, []):
                if resource in self.allocation:
                    holder = self.allocation[resource]
                    if holder != process:
                        self.wait_for[process].append(holder)
    
    def detect_deadlock(self):
        self.build_wait_for_graph()
        
        # Use DFS to detect cycles
        visited = set()
        rec_stack = set()
        
        def dfs(node):
            if node in rec_stack:
                return True
            if node in visited:
                return False
            
            visited.add(node)
            rec_stack.add(node)
            
            for neighbor in self.wait_for.get(node, []):
                if dfs(neighbor):
                    return True
            
            rec_stack.remove(node)
            return False
        
        for process in self.processes:
            if process not in visited:
                if dfs(process):
                    return True
        return False

# Example usage
wfg = WaitForGraph()

# Set up scenario
wfg.allocate_resource("R1", "P1")
wfg.allocate_resource("R2", "P2")
wfg.request_resource("P1", "R2")
wfg.request_resource("P2", "R1")

if wfg.detect_deadlock():
    print("Deadlock detected in wait-for graph!")
```

### 4. Recovery

#### Process Termination
```python
class DeadlockRecovery:
    def __init__(self):
        self.processes = {}  # process_id -> {priority, resources, cost}
    
    def add_process(self, process_id, priority, resources, cost):
        self.processes[process_id] = {
            'priority': priority,
            'resources': resources,
            'cost': cost
        }
    
    def terminate_all_deadlocked(self, deadlocked_processes):
        """Terminate all processes in deadlock"""
        for process_id in deadlocked_processes:
            self.terminate_process(process_id)
        return f"Terminated all deadlocked processes: {deadlocked_processes}"
    
    def terminate_one_by_one(self, deadlocked_processes):
        """Terminate processes one by one until deadlock is broken"""
        # Sort by termination cost (ascending)
        sorted_processes = sorted(deadlocked_processes, 
                                key=lambda p: self.processes[p]['cost'])
        
        terminated = []
        for process_id in sorted_processes:
            self.terminate_process(process_id)
            terminated.append(process_id)
            
            # Check if deadlock is broken (simplified check)
            if self.is_deadlock_broken(deadlocked_processes, terminated):
                break
        
        return f"Terminated processes: {terminated}"
    
    def terminate_process(self, process_id):
        if process_id in self.processes:
            # Release all resources held by the process
            resources = self.processes[process_id]['resources']
            for resource in resources:
                self.release_resource(resource)
            
            # Remove process
            del self.processes[process_id]
            print(f"Process {process_id} terminated")
    
    def is_deadlock_broken(self, original_deadlocked, terminated):
        # Simplified check - in reality, you'd run deadlock detection again
        return len(terminated) >= len(original_deadlocked) // 2
    
    def release_resource(self, resource):
        print(f"Resource {resource} released")

# Example usage
recovery = DeadlockRecovery()
recovery.add_process("P1", priority=1, resources=["R1", "R2"], cost=100)
recovery.add_process("P2", priority=2, resources=["R3"], cost=50)
recovery.add_process("P3", priority=3, resources=["R4"], cost=200)

deadlocked = ["P1", "P2", "P3"]
result = recovery.terminate_one_by_one(deadlocked)
print(result)
```

#### Resource Preemption
```python
class ResourcePreemption:
    def __init__(self):
        self.processes = {}
        self.resources = {}
    
    def preempt_resources(self, victim_process, resources_to_preempt):
        """Preempt resources from victim process"""
        if victim_process not in self.processes:
            return False
        
        # Save the state of victim process
        self.save_process_state(victim_process)
        
        # Remove resources from victim
        for resource in resources_to_preempt:
            if resource in self.processes[victim_process]['resources']:
                self.processes[victim_process]['resources'].remove(resource)
                
                # Make resource available
                self.resources[resource] = None
                print(f"Resource {resource} preempted from {victim_process}")
        
        # Rollback victim process to safe state
        self.rollback_process(victim_process)
        
        return True
    
    def save_process_state(self, process_id):
        """Save process state for later rollback"""
        state = {
            'resources': self.processes[process_id]['resources'][:],
            'execution_state': 'saved',
            'timestamp': time.time()
        }
        self.processes[process_id]['saved_state'] = state
        print(f"State saved for process {process_id}")
    
    def rollback_process(self, process_id):
        """Rollback process to previous safe state"""
        if 'saved_state' in self.processes[process_id]:
            # Restore previous state
            print(f"Rolling back process {process_id}")
            self.processes[process_id]['status'] = 'rolled_back'
    
    def select_victim(self, deadlocked_processes):
        """Select victim process based on various factors"""
        # Factors to consider:
        # 1. Number of resources held
        # 2. Process priority
        # 3. How long process has been running
        # 4. How easy it is to rollback
        
        best_victim = None
        min_cost = float('inf')
        
        for process_id in deadlocked_processes:
            cost = self.calculate_preemption_cost(process_id)
            if cost < min_cost:
                min_cost = cost
                best_victim = process_id
        
        return best_victim
    
    def calculate_preemption_cost(self, process_id):
        """Calculate cost of preempting this process"""
        process = self.processes[process_id]
        
        # Simple cost calculation
        resource_cost = len(process.get('resources', []))
        priority_cost = 10 - process.get('priority', 5)  # Lower priority = higher cost
        rollback_cost = process.get('execution_time', 0) * 0.1
        
        return resource_cost + priority_cost + rollback_cost

# Example usage
preemption = ResourcePreemption()

# Add processes
preemption.processes["P1"] = {
    'resources': ["R1", "R2"],
    'priority': 3,
    'execution_time': 100
}
preemption.processes["P2"] = {
    'resources': ["R3"],
    'priority': 1,
    'execution_time': 50
}

# Select victim and preempt resources
victim = preemption.select_victim(["P1", "P2"])
print(f"Selected victim: {victim}")

preemption.preempt_resources(victim, ["R1"])
```

## Complete Example: Deadlock Scenario

```python
import threading
import time
import random

class DeadlockExample:
    def __init__(self):
        self.resource1 = threading.Lock()
        self.resource2 = threading.Lock()
    
    def process1(self):
        print("Process 1: Acquiring resource 1...")
        self.resource1.acquire()
        print("Process 1: Got resource 1")
        
        # Simulate some work
        time.sleep(random.uniform(0.1, 0.5))
        
        print("Process 1: Trying to acquire resource 2...")
        self.resource2.acquire()  # This may cause deadlock
        print("Process 1: Got resource 2")
        
        # Do work with both resources
        time.sleep(0.1)
        
        print("Process 1: Releasing resources...")
        self.resource2.release()
        self.resource1.release()
        print("Process 1: Done")
    
    def process2(self):
        print("Process 2: Acquiring resource 2...")
        self.resource2.acquire()
        print("Process 2: Got resource 2")
        
        # Simulate some work
        time.sleep(random.uniform(0.1, 0.5))
        
        print("Process 2: Trying to acquire resource 1...")
        self.resource1.acquire()  # This may cause deadlock
        print("Process 2: Got resource 1")
        
        # Do work with both resources
        time.sleep(0.1)
        
        print("Process 2: Releasing resources...")
        self.resource1.release()
        self.resource2.release()
        print("Process 2: Done")
    
    def run_deadlock_scenario(self):
        print("Starting deadlock scenario...")
        
        t1 = threading.Thread(target=self.process1)
        t2 = threading.Thread(target=self.process2)
        
        t1.start()
        t2.start()
        
        # Wait for threads with timeout
        t1.join(timeout=5)
        t2.join(timeout=5)
        
        if t1.is_alive() or t2.is_alive():
            print("DEADLOCK DETECTED! Threads are still running after timeout.")
        else:
            print("No deadlock occurred.")

# Example of deadlock prevention using resource ordering
class DeadlockPrevention:
    def __init__(self):
        self.resource1 = threading.Lock()
        self.resource2 = threading.Lock()
    
    def process1(self):
        print("Process 1: Acquiring resources in order...")
        # Always acquire resource1 first, then resource2
        self.resource1.acquire()
        print("Process 1: Got resource 1")
        
        self.resource2.acquire()
        print("Process 1: Got resource 2")
        
        # Do work
        time.sleep(0.1)
        
        print("Process 1: Releasing resources...")
        self.resource2.release()
        self.resource1.release()
        print("Process 1: Done")
    
    def process2(self):
        print("Process 2: Acquiring resources in order...")
        # Always acquire resource1 first, then resource2
        self.resource1.acquire()
        print("Process 2: Got resource 1")
        
        self.resource2.acquire()
        print("Process 2: Got resource 2")
        
        # Do work
        time.sleep(0.1)
        
        print("Process 2: Releasing resources...")
        self.resource2.release()
        self.resource1.release()
        print("Process 2: Done")
    
    def run_prevention_scenario(self):
        print("Starting deadlock prevention scenario...")
        
        t1 = threading.Thread(target=self.process1)
        t2 = threading.Thread(target=self.process2)
        
        t1.start()
        t2.start()
        
        t1.join()
        t2.join()
        
        print("Both processes completed successfully!")

# Run examples
if __name__ == "__main__":
    # Uncomment to see deadlock in action (may hang)
    # deadlock_demo = DeadlockExample()
    # deadlock_demo.run_deadlock_scenario()
    
    # Run deadlock prevention example
    prevention_demo = DeadlockPrevention()
    prevention_demo.run_prevention_scenario()
```

## Key Takeaways

1. **Deadlock occurs when all four conditions are met simultaneously**
2. **Prevention** breaks one of the four conditions before deadlock occurs
3. **Avoidance** uses algorithms like Banker's to ensure safe states
4. **Detection** finds deadlocks after they occur using graph algorithms
5. **Recovery** removes deadlocks through process termination or resource preemption

Each approach has trade-offs in terms of system performance, resource utilization, and complexity. The choice depends on your specific system requirements and constraints.