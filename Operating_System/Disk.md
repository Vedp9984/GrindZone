# 📁 File Systems & Disk Scheduling

## 🎯 What is a File System?
A file system is like a **digital filing cabinet** that organizes how data is stored, accessed, and managed on storage devices. It's the bridge between your programs and the physical storage hardware.

---

## 📄 File Concepts

### What is a File?
A **file** is a named collection of related data stored on a storage device. Think of it as a digital document or container that holds information.

### File Attributes
Files have **metadata** (data about data) that describes their properties:

#### Essential File Attributes:
1. **Name**: Human-readable identifier
   - Example: "document.txt", "photo.jpg", "program.exe"

2. **Identifier**: Unique number assigned by file system
   - Example: Inode number 12345 in Unix/Linux

3. **Type**: Indicates file format and how to interpret data
   - Example: .txt (text), .jpg (image), .exe (executable)

4. **Location**: Physical address on storage device
   - Example: Track 5, Sector 20 on hard disk

5. **Size**: Current file size and maximum size
   - Example: 2.5 MB current, 100 MB maximum

6. **Protection**: Access permissions and security
   - Example: Read-Write-Execute permissions (rwx)

7. **Time/Date**: Creation, modification, and access timestamps
   - Example: Created: 2024-01-15, Modified: 2024-01-20

8. **User Information**: Owner and group information
   - Example: Owner: john, Group: developers

#### Example File Attributes:
```
File: report.docx
- Name: report.docx
- Identifier: 156789
- Type: Microsoft Word Document
- Location: Disk 1, Block 2500-2520
- Size: 1.2 MB
- Protection: rw-r--r-- (owner can read/write, others can read)
- Created: 2024-01-10 09:30:00
- Modified: 2024-01-15 14:45:00
- Owner: alice
```

---

## 🔧 File Operations

### 1. Create Operation
**Purpose**: Make a new file in the file system

**Steps**:
```
1. Find space in directory for new file entry
2. Allocate space on disk for file data
3. Initialize file attributes
4. Update directory structure
```

**Example**:
```
create("newfile.txt") →
- Allocate directory entry
- Set initial attributes (size=0, created=now)
- Return file handle/identifier
```

### 2. Open Operation
**Purpose**: Prepare file for reading/writing

**Steps**:
```
1. Search directory for file name
2. Check access permissions
3. Load file attributes into memory
4. Create file handle/descriptor
5. Return handle to process
```

**Example**:
```
file_handle = open("document.txt", "read-write")
- Locate document.txt in directory
- Check if user has read-write permission
- Load file control block into memory
- Return handle #5 to process
```

### 3. Read Operation
**Purpose**: Get data from file

**Steps**:
```
1. Use file handle to locate file
2. Check if read permission exists
3. Read data from specified position
4. Update file position pointer
5. Return data to process
```

**Example**:
```
data = read(file_handle, 1024) →
- Read 1024 bytes from current position
- Move file pointer forward by 1024 bytes
- Return data to program
```

### 4. Write Operation
**Purpose**: Store data to file

**Steps**:
```
1. Use file handle to locate file
2. Check if write permission exists
3. Write data to specified position
4. Update file size if necessary
5. Update modification time
```

**Example**:
```
write(file_handle, "Hello World") →
- Write "Hello World" at current position
- Update file size to reflect new data
- Update modification timestamp
```

### 5. Delete Operation
**Purpose**: Remove file from system

**Steps**:
```
1. Locate file in directory
2. Check if delete permission exists
3. Release allocated disk space
4. Remove directory entry
5. Update free space information
```

**Example**:
```
delete("oldfile.txt") →
- Find oldfile.txt in directory
- Mark disk blocks as free
- Remove directory entry
- Update free space bitmap
```

---

## 📖 File Types

### 1. Regular Files
**Purpose**: Store user data
- **Text Files**: Human-readable content (documents, code)
- **Binary Files**: Machine-readable content (executables, images)

**Examples**:
```
Text: report.txt, code.java, config.ini
Binary: photo.jpg, music.mp3, program.exe
```

### 2. Directory Files
**Purpose**: Store information about other files
- Contains list of files and subdirectories
- Maintains file attributes and locations

**Example**:
```
Directory: /home/user/documents/
Contains: report.txt, photo.jpg, subfolder/
```

### 3. Special Files
**Purpose**: Interface with hardware devices
- **Character Special**: Handle character-by-character I/O
- **Block Special**: Handle block-oriented I/O

**Examples**:
```
Character: /dev/keyboard, /dev/mouse
Block: /dev/sda1 (hard disk), /dev/cdrom
```

---

## 🔍 File Access Methods

### 1. Sequential Access
**How it works**: Read/write data in order from beginning to end, like reading a book page by page.

**Characteristics**:
- File pointer moves automatically
- Cannot skip to arbitrary positions
- Simple and efficient for sequential processing

**Example**:
```
File: [A][B][C][D][E][F]
Reading: A → B → C → D → E → F
Cannot jump directly to E without reading A,B,C,D first
```

**Use Cases**:
- Reading log files
- Processing large datasets
- Backup operations

**Advantages**:
- Simple implementation
- Good for magnetic tapes
- Efficient for sequential processing

**Disadvantages**:
- Slow for random access
- Must read entire file to reach end
- Cannot update middle efficiently

### 2. Direct Access (Random Access)
**How it works**: Jump directly to any position in file, like jumping to any page in a book.

**Characteristics**:
- Can specify exact position to read/write
- File treated as numbered sequence of blocks
- Requires disk address calculation

**Example**:
```
File: [A][B][C][D][E][F]
       0  1  2  3  4  5
Direct access: read(position=4) → gets E directly
```

**Operations**:
```
read(position)     - Read from specific position
write(position)    - Write to specific position  
seek(position)     - Move file pointer to position
```

**Use Cases**:
- Database files
- Virtual memory systems
- Random access to large files

**Advantages**:
- Fast access to any position
- Efficient for databases
- Good for file updates

**Disadvantages**:
- More complex implementation
- Requires position calculation
- Not suitable for all storage types

### 3. Indexed Access
**How it works**: Use an index to quickly locate data, like using book's index to find topics.

**Characteristics**:
- Maintains index structure with pointers
- Index contains key-value pairs
- Allows fast searching by key

**Example**:
```
Index Table:
Key    | Position
-------|----------
"John" | Block 5
"Mary" | Block 2  
"Bob"  | Block 8

To find "Mary": Look in index → Go to Block 2
```

**Structure**:
```
Index File: [Index Entries]
Data File:  [Actual Data Blocks]

Index Entry: [Key][Pointer to Data Block]
```

**Use Cases**:
- Database index files
- Phone directories
- Dictionary applications

**Advantages**:
- Very fast search operations
- Efficient for large files
- Good for key-based access

**Disadvantages**:
- Extra storage for index
- Index maintenance overhead
- Complex implementation

---

## 🗂️ Directory Structures

### 1. Single-Level Directory
**Structure**: All files in one directory (flat structure)

**Example**:
```
Root Directory
├── file1.txt
├── file2.doc
├── program.exe
├── photo.jpg
└── music.mp3
```

**Advantages**:
- Simple to implement
- Fast file lookup
- Easy to understand

**Disadvantages**:
- No file organization
- Name conflicts (all files must have unique names)
- No user separation
- Difficult to manage many files

**Use Case**: Simple systems with few files

### 2. Two-Level Directory
**Structure**: Separate directory for each user

**Example**:
```
Root Directory
├── User1/
│   ├── file1.txt
│   └── program.exe
├── User2/
│   ├── file2.doc
│   └── photo.jpg
└── User3/
    ├── music.mp3
    └── data.csv
```

**Advantages**:
- User isolation
- Same filenames allowed for different users
- Better organization than single-level

**Disadvantages**:
- Still limited organization within user directory
- No subdirectories
- Difficult to share files between users

**Use Case**: Multi-user systems with simple file organization

### 3. Tree-Structured Directory
**Structure**: Hierarchical structure like an upside-down tree

**Example**:
```
Root (/)
├── home/
│   ├── user1/
│   │   ├── documents/
│   │   │   ├── report.txt
│   │   │   └── notes.doc
│   │   └── pictures/
│   │       └── photo.jpg
│   └── user2/
│       └── code/
│           └── program.c
├── system/
│   ├── bin/
│   └── lib/
└── temp/
```

**Advantages**:
- Excellent organization
- Natural file grouping
- Easy to navigate
- Most common structure

**Disadvantages**:
- More complex implementation
- Longer path names
- Cannot share files easily

**Use Case**: Most modern file systems (Windows, Unix/Linux)

### 4. Acyclic Graph Directory
**Structure**: Tree structure + file sharing (no cycles)

**Example**:
```
Root
├── user1/
│   ├── documents/
│   │   └── shared.txt ─────┐
│   └── personal/           │
└── user2/                  │
    └── work/               │
        └── shared.txt ←────┘ (same file, different paths)
```

**Link Types**:
- **Hard Links**: Multiple directory entries point to same file
- **Soft Links**: Directory entry points to another directory entry

**Advantages**:
- File sharing between users
- Saves disk space
- Maintains tree benefits

**Disadvantages**:
- Complex implementation
- Link management overhead
- Backup/deletion complexity

**Use Case**: Unix/Linux systems with file sharing

### 5. General Graph Directory
**Structure**: Acyclic graph + cycles allowed

**Example**:
```
Root
├── dir1/
│   ├── file1.txt
│   └── link_to_dir2/ ─────┐
├── dir2/                  │
│   ├── file2.txt          │
│   └── link_to_dir1/ ←────┘ (creates cycle)
└── dir3/
```

**Advantages**:
- Maximum flexibility
- Complex file relationships
- Advanced sharing capabilities

**Disadvantages**:
- Very complex implementation
- Cycle detection required
- Difficult garbage collection
- Risk of infinite loops

**Use Case**: Research systems, specialized applications

---

## 💾 File Allocation Methods

### 1. Contiguous Allocation
**How it works**: Store file in consecutive disk blocks, like parking cars in consecutive parking spots.

**Structure**:
```
Directory Entry: [File Name][Start Block][Length]
Example: report.txt, Block 5, Length 4

Disk Layout:
Block: 0  1  2  3  4  5  6  7  8  9  10
File:  -  -  -  -  - [A][A][A][A] -  -
                      ↑file starts here
```

**Example**:
```
File: document.txt (3 blocks)
Directory: document.txt, Start=10, Length=3
Disk: Blocks 10, 11, 12 contain the file

To read: Go to block 10, read 3 consecutive blocks
```

**Advantages**:
- **Simple implementation** (only need start + length)
- **Fast sequential access** (no seeking between blocks)
- **Minimal disk head movement** (all blocks together)
- **Good performance** for sequential files

**Disadvantages**:
- **External fragmentation** (free blocks scattered)
- **File size limitation** (cannot grow beyond allocated space)
- **Difficult to find large contiguous space**
- **Compaction required** to reduce fragmentation

**Use Cases**:
- Read-only files (CD-ROM)
- Database systems with known file sizes
- Multimedia files requiring sequential access

### 2. Linked Allocation
**How it works**: Each block contains pointer to next block, like a treasure hunt with clues.

**Structure**:
```
Directory Entry: [File Name][First Block]
Each Block: [Data][Pointer to Next Block]

Example:
Directory: report.txt, First Block=5
Block 5: [Data][→8]
Block 8: [Data][→2]  
Block 2: [Data][→NULL]
```

**Example**:
```
File: document.txt
Directory: document.txt, Start=10
Block 10: [Data][→15]
Block 15: [Data][→3]
Block 3:  [Data][→NULL]

To read: Start at block 10 → follow chain → block 15 → block 3
```

**Advantages**:
- **No external fragmentation** (any free block can be used)
- **Files can grow dynamically** (just add more blocks)
- **No space waste** (allocate exactly what's needed)
- **Simple free space management**

**Disadvantages**:
- **Poor random access** (must follow chain from beginning)
- **Pointer overhead** (space lost to pointers)
- **Reliability issues** (broken link breaks entire file)
- **Sequential access only** efficient

**Variations**:
- **File Allocation Table (FAT)**: Keep all pointers in separate table
- **Linked List with Index**: Combine benefits of both approaches

**Use Cases**:
- Sequential access files
- Files with unknown final size
- Simple file systems (FAT)

### 3. Indexed Allocation
**How it works**: Use index block to store pointers to data blocks, like a table of contents.

**Structure**:
```
Directory Entry: [File Name][Index Block]
Index Block: [Pointer1][Pointer2][Pointer3]...[PointerN]

Example:
Directory: report.txt, Index Block=10
Block 10: [→5][→8][→2][→NULL]
Block 5: [Data]
Block 8: [Data]
Block 2: [Data]
```

**Example**:
```
File: document.txt (3 blocks of data)
Directory: document.txt, Index=20
Block 20: [→10][→15][→3][→NULL]...
Block 10: [Data Block 1]
Block 15: [Data Block 2]
Block 3:  [Data Block 3]

To read block 2: Go to index block 20 → read pointer 2 → go to block 3
```

**Advantages**:
- **Fast random access** (direct access via index)
- **No external fragmentation** (blocks can be anywhere)
- **Dynamic file growth** (add pointers to index)
- **Efficient for large files**

**Disadvantages**:
- **Index block overhead** (extra block for small files)
- **Limited file size** (index block size limits pointers)
- **Complex implementation**
- **Two disk accesses** (index + data)

**Multi-level Indexing**:
For very large files, use multiple levels of indexing:
```
Level 1 Index: Points to Level 2 Index blocks
Level 2 Index: Points to actual data blocks
```

**Use Cases**:
- Database systems
- Large files requiring random access
- Modern file systems (ext3, NTFS)

---

## 🆓 Free Space Management

### 1. Bitmaps (Bit Vector)
**How it works**: Use one bit per block to track free/occupied status.

**Structure**:
```
Bit = 0: Block is free
Bit = 1: Block is occupied

Example (8 blocks):
Blocks:  0  1  2  3  4  5  6  7
Status:  F  O  F  F  O  O  F  F
Bitmap:  0  1  0  0  1  1  0  0
```

**Example**:
```
Disk with 16 blocks:
Bitmap: 1100101001010100
        ||||||||||||||||
Blocks: 0123456789ABCDEF

Block 0: Occupied (bit = 1)
Block 1: Occupied (bit = 1)  
Block 2: Free (bit = 0)
Block 3: Free (bit = 0)
...and so on
```

**Operations**:
```
Find free block: Scan bitmap for first 0 bit
Allocate block: Set bit to 1
Deallocate block: Set bit to 0
```

**Advantages**:
- **Simple implementation**
- **Fast free block detection**
- **Space efficient** for large disks
- **Easy to find contiguous blocks**

**Disadvantages**:
- **Bitmap size grows** with disk size
- **Must keep entire bitmap in memory**
- **Sequential search** for free blocks

**Use Cases**:
- Unix/Linux file systems
- Systems with large disks
- When fast allocation is important

### 2. Linked List
**How it works**: Link all free blocks together in a chain.

**Structure**:
```
Free Block Header: Points to first free block
Each Free Block: [Next Free Block Pointer][Unused Space]

Example:
Free List Head: Points to Block 5
Block 5: [→Block 12][Free Space]
Block 12: [→Block 3][Free Space]
Block 3: [→NULL][Free Space]
```

**Example**:
```
Disk layout:
Block 0: [Occupied]
Block 1: [Occupied]
Block 2: [→Block 7][Free Space]  ← First free block
Block 3: [Occupied]
Block 4: [Occupied]
Block 5: [Occupied]
Block 6: [Occupied]
Block 7: [→Block 9][Free Space]  ← Second free block
Block 8: [Occupied]
Block 9: [→NULL][Free Space]     ← Last free block

Free List: Head → Block 2 → Block 7 → Block 9 → NULL
```

**Operations**:
```
Allocate: Remove first block from free list
Deallocate: Add block to front of free list
```

**Advantages**:
- **No space waste** (no bitmap storage)
- **Dynamic size** (grows/shrinks with free blocks)
- **Simple allocation** (take first block)

**Disadvantages**:
- **Sequential access** to find free blocks
- **No knowledge of block size** (can't find contiguous space easily)
- **Link overhead** in each free block
- **Traversal time** increases with free blocks

**Use Cases**:
- Systems with many free blocks
- Simple file systems
- When memory is limited

### 3. Grouping
**How it works**: Store addresses of free blocks in groups, with each group pointing to next group.

**Structure**:
```
Group Block: [Address1][Address2]...[AddressN][Next Group]

Example:
Group 1: [Block 5][Block 12][Block 8][→Group 2]
Group 2: [Block 15][Block 3][Block 20][→NULL]
```

**Example**:
```
First Group Block (Block 100):
[Block 5][Block 12][Block 8][Block 25][→Block 200]

Second Group Block (Block 200):
[Block 15][Block 3][Block 20][Block 45][→NULL]

Free blocks available: 5, 12, 8, 25, 15, 3, 20, 45
```

**Advantages**:
- **Faster than linked list** (multiple addresses per access)
- **Good for large free spaces**
- **Balanced space usage**

**Disadvantages**:
- **More complex than simple linked list**
- **Still requires traversal** for large numbers
- **Fixed group size** limitations

**Use Cases**:
- Systems with moderate free space
- Hybrid approaches in modern file systems

---

## 💿 Disk Scheduling Algorithms

### Why Disk Scheduling?
Hard disks have **mechanical parts** (read/write head, spinning platters). Moving the head between tracks (seeking) is slow. Good scheduling minimizes seek time.

**Disk Structure**:
```
Tracks: Concentric circles (0, 1, 2, ..., 199)
Sectors: Pie slices of each track
Cylinders: Same track number across all platters
```

### 1. FCFS (First Come First Served)
**How it works**: Process requests in the order they arrive, like a simple queue.

**Example**:
```
Request Queue: 98, 183, 37, 122, 14, 124, 65, 67
Current Head Position: 53

Processing Order: 53 → 98 → 183 → 37 → 122 → 14 → 124 → 65 → 67

Seek Distance:
53 → 98:  |98-53|  = 45
98 → 183: |183-98| = 85
183 → 37: |37-183| = 146
37 → 122: |122-37| = 85
122 → 14: |14-122| = 108
14 → 124: |124-14| = 110
124 → 65: |65-124| = 59
65 → 67:  |67-65|  = 2

Total Seek Distance: 45+85+146+85+108+110+59+2 = 640
Average Seek Distance: 640/8 = 80
```

**Advantages**:
- **Simple to implement**
- **Fair** (no request starvation)
- **Low overhead**

**Disadvantages**:
- **Poor performance** (lots of random seeking)
- **High seek time** for random requests
- **Inefficient** for high-load systems

### 2. SSTF (Shortest Seek Time First)
**How it works**: Always process the request closest to current head position.

**Example**:
```
Request Queue: 98, 183, 37, 122, 14, 124, 65, 67
Current Head Position: 53

Step-by-step processing:
Position 53: Closest requests are 65(12), 67(14), 37(16), 98(45)
Choose 65 (distance 12)

Position 65: Closest requests are 67(2), 37(28), 98(33), 122(57)
Choose 67 (distance 2)

Position 67: Closest requests are 37(30), 98(31), 122(55), 183(116)
Choose 98 (distance 31)

Position 98: Closest requests are 122(24), 37(61), 183(85), 124(26)
Choose 122 (distance 24)

Position 122: Closest requests are 124(2), 183(61), 37(85), 14(108)
Choose 124 (distance 2)

Position 124: Closest requests are 183(59), 37(87), 14(110)
Choose 183 (distance 59)

Position 183: Closest requests are 37(146), 14(169)
Choose 37 (distance 146)

Position 37: Remaining request is 14
Choose 14 (distance 23)

Processing Order: 53 → 65 → 67 → 98 → 122 → 124 → 183 → 37 → 14
Total Seek Distance: 12+2+31+24+2+59+146+23 = 299
Average: 299/8 = 37.4
```

**Advantages**:
- **Better performance** than FCFS
- **Reduces seek time** significantly
- **Good for moderate loads**

**Disadvantages**:
- **Starvation possible** (far requests may never be served)
- **Favors middle tracks** over outer tracks
- **Complex implementation**

### 3. SCAN (Elevator Algorithm)
**How it works**: Head moves in one direction, serving requests, then reverses direction.

**Example**:
```
Request Queue: 98, 183, 37, 122, 14, 124, 65, 67
Current Head Position: 53
Direction: Moving towards higher tracks (upward)
Disk Range: 0 to 199

Requests in upward direction from 53: 65, 67, 98, 122, 124, 183
Requests in downward direction from 53: 37, 14

Processing Order:
53 → 65 → 67 → 98 → 122 → 124 → 183 → 199 (end) → 37 → 14

Seek Distance:
53 → 65:  12
65 → 67:  2
67 → 98:  31
98 → 122: 24
122 → 124: 2
124 → 183: 59
183 → 199: 16  (go to end of disk)
199 → 37:  162 (reverse direction)
37 → 14:   23

Total Seek Distance: 12+2+31+24+2+59+16+162+23 = 331
Average: 331/8 = 41.4
```

**Advantages**:
- **No starvation** (every request eventually served)
- **Good performance** for heavy loads
- **Predictable** service time

**Disadvantages**:
- **Higher seek time** than SSTF
- **Unfair to requests** just missed by head
- **Must go to disk ends** even if no requests

### 4. LOOK Algorithm
**How it works**: Like SCAN but only goes to the furthest request, not disk end.

**Example**:
```
Request Queue: 98, 183, 37, 122, 14, 124, 65, 67
Current Head Position: 53
Direction: Moving upward

Highest request in upward direction: 183 (not 199)
Lowest request in downward direction: 14 (not 0)

Processing Order:
53 → 65 → 67 → 98 → 122 → 124 → 183 → 37 → 14

Seek Distance:
53 → 65:  12
65 → 67:  2
67 → 98:  31
98 → 122: 24
122 → 124: 2
124 → 183: 59
183 → 37:  146 (reverse direction)
37 → 14:   23

Total Seek Distance: 12+2+31+24+2+59+146+23 = 299
Average: 299/8 = 37.4
```

**Advantages**:
- **More efficient** than SCAN (no unnecessary movement)
- **No starvation**
- **Better response time** than SCAN

**Disadvantages**:
- **Still some unfairness** to requests just missed
- **More complex** than SCAN

### 5. C-SCAN (Circular SCAN)
**How it works**: Head moves in one direction only, returns to start after reaching end.

**Example**:
```
Request Queue: 98, 183, 37, 122, 14, 124, 65, 67
Current Head Position: 53
Direction: Moving upward only

Processing Order:
53 → 65 → 67 → 98 → 122 → 124 → 183 → 199 → 0 → 14 → 37

Seek Distance:
53 → 65:  12
65 → 67:  2
67 → 98:  31
98 → 122: 24
122 → 124: 2
124 → 183: 59
183 → 199: 16  (go to end)
199 → 0:   199 (jump to start)
0 → 14:    14
14 → 37:   23

Total Seek Distance: 12+2+31+24+2+59+16+199+14+23 = 382
Average: 382/8 = 47.8
```

**Advantages**:
- **Uniform response time** (all requests treated equally)
- **No starvation**
- **Good for heavy loads**

**Disadvantages**:
- **Higher seek time** due to return jump
- **Waste of movement** during return
- **More complex** implementation

### 6. C-LOOK Algorithm
**How it works**: Like C-SCAN but only goes to furthest request, then jumps to nearest request in opposite direction.

**Example**:
```
Request Queue: 98, 183, 37, 122, 14, 124, 65, 67
Current Head Position: 53
Direction: Moving upward

Processing Order:
53 → 65 → 67 → 98 → 122 → 124 → 183 → 14 → 37

Seek Distance:
53 → 65:  12
65 → 67:  2
67 → 98:  31
98 → 122: 24
122 → 124: 2
124 → 183: 59
183 → 14:  169 (jump to lowest request)
14 → 37:   23

Total Seek Distance: 12+2+31+24+2+59+169+23 = 322
Average: 322/8 = 40.3
```

**Advantages**:
- **More efficient** than C-SCAN
- **Uniform response time**
- **No starvation**

**Disadvantages**:
- **Still has jump penalty**
- **Complex implementation**

### Algorithm Comparison:
```
Algorithm | Total Seek Distance | Average | Starvation Risk
----------|--------------------|---------|-----------------
FCFS      | 640                | 80.0    | No
SSTF      | 299                | 37.4    | Yes
SCAN      | 331                | 41.4    | No
LOOK      | 299                | 37.4    | No
C-SCAN    | 382                | 47.8    | No
C-LOOK    | 322                | 40.3    | No
```

---

# RAID (Redundant Array of Independent Disks) – Overview

**RAID** is a data storage virtualization technology that combines multiple physical disk drives into one logical unit for improved performance, fault tolerance, or both.

---

## Why RAID?

- ✅ Improved performance (read/write)
- ✅ Redundancy (data protection)
- ✅ High availability
- ✅ Scalable storage

---

## RAID Levels: RAID 0 to RAID 6

### 🔹 RAID 0 – Striping

- **No redundancy**
- Data is split (striped) across multiple disks.
- Improves performance (parallel reads/writes).
- **If one disk fails, all data is lost.**

**Use Case:** High-speed storage where data loss is acceptable (e.g., video editing).

| Feature       | Value           |
|---------------|-----------------|
| Redundancy    | ❌ None          |
| Min Disks     | 2               |
| Fault Tolerance | ❌             |
| Performance   | 🔼 High          |
| Storage Efficiency | 100%       |

---

### 🔹 RAID 1 – Mirroring

- **Data is mirrored** (duplicated) across two or more disks.
- If one disk fails, the other still holds the data.
- Slower writes (written to both), fast reads (read from either).

**Use Case:** Critical data needing full redundancy.

| Feature       | Value           |
|---------------|-----------------|
| Redundancy    | ✅ Yes           |
| Min Disks     | 2               |
| Fault Tolerance | ✅ 1 disk       |
| Performance   | 🔼 Read boost    |
| Storage Efficiency | 50%       |

---

### 🔹 RAID 2 – Bit-level Striping with Hamming Code

- Rarely used.
- Data is striped at bit level.
- Uses Hamming code for error correction.
- High overhead and complexity.

**Use Case:** Theoretical/academic, not practical today.

| Feature       | Value           |
|---------------|-----------------|
| Redundancy    | ✅ Yes           |
| Min Disks     | ≥ 3 (often 10+) |
| Fault Tolerance | ✅             |
| Performance   | Medium          |
| Storage Efficiency | Low        |

---

### 🔹 RAID 3 – Byte-level Striping with Dedicated Parity

- Data is striped at **byte** level.
- One disk stores parity for recovery.
- Can recover from single disk failure.

**Use Case:** Systems with large sequential data (e.g., video streaming).

| Feature       | Value           |
|---------------|-----------------|
| Redundancy    | ✅ Yes           |
| Min Disks     | 3               |
| Fault Tolerance | ✅ 1 disk       |
| Performance   | Fast read, slow write |
| Storage Efficiency | (n-1)/n     |

---

### 🔹 RAID 4 – Block-level Striping with Dedicated Parity

- Like RAID 3 but with **block-level** striping.
- One dedicated parity disk.
- Parity disk bottleneck during writes.

**Use Case:** Rarely used, replaced by RAID 5.

| Feature       | Value           |
|---------------|-----------------|
| Redundancy    | ✅ Yes           |
| Min Disks     | 3               |
| Fault Tolerance | ✅ 1 disk       |
| Performance   | Read: Good, Write: Bottleneck |
| Storage Efficiency | (n-1)/n     |

---

### 🔹 RAID 5 – Block-level Striping with Distributed Parity

- Data and parity are striped across all disks.
- Can withstand 1 disk failure.
- Most popular RAID level.

**Use Case:** Balanced solution for performance + redundancy.

| Feature       | Value           |
|---------------|-----------------|
| Redundancy    | ✅ Yes           |
| Min Disks     | 3               |
| Fault Tolerance | ✅ 1 disk       |
| Performance   | Good for read and write |
| Storage Efficiency | (n-1)/n     |

---

### 🔹 RAID 6 – Striping with Dual Distributed Parity

- Like RAID 5, but with **two parity blocks**.
- Can tolerate **2 simultaneous disk failures**.
- Higher write overhead due to dual parity.

**Use Case:** Critical systems needing high fault tolerance.

| Feature       | Value           |
|---------------|-----------------|
| Redundancy    | ✅ Yes           |
| Min Disks     | 4               |
| Fault Tolerance | ✅ 2 disks      |
| Performance   | Good read, slower write |
| Storage Efficiency | (n-2)/n     |

---

## Summary Table

| RAID | Redundancy | Min Disks | Tolerates Failure | Performance | Efficiency |
|------|------------|-----------|-------------------|-------------|------------|
| 0    | ❌ No       | 2         | 0 disks           | 🔼 High      | 100%       |
| 1    | ✅ Yes      | 2         | 1 disk            | 🔼 Read      | 50%        |
| 2    | ✅ Yes      | ≥ 3       | Multiple          | Medium      | Low        |
| 3    | ✅ Yes      | 3         | 1 disk            | Good Read   | ~67%       |
| 4    | ✅ Yes      | 3         | 1 disk            | Bottlenecks | ~67%       |
| 5    | ✅ Yes      | 3         | 1 disk            | Balanced    | ~67–80%    |
| 6    | ✅ Yes      | 4         | 2 disks           | Slower Write| ~50–75%    |

---

## Note:

- RAID is **not a substitute for backups**.
- RAID improves **availability**, not **disaster recovery**.



