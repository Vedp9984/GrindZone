# C Fundamentals - Interview Questions & Answers

This README contains a collection of the most important and frequently asked interview questions on **C programming fundamentals**, along with clear and concise answers.

---

## 📘 Table of Contents

1. [Data Types & Storage](#1-data-types--storage)
2. [Operators & Expressions](#2-operators--expressions)
3. [Pointers & Memory](#3-pointers--memory)
4. [Arrays & Strings](#4-arrays--strings)
5. [Functions](#5-functions)
6. [Storage Classes](#6-storage-classes)
7. [Control Flow](#7-control-flow)
8. [Structures & Unions](#8-structures--unions)
9. [File Handling](#9-file-handling)
10. [Compilation & Execution](#10-compilation--execution)
11. [Miscellaneous](#11-miscellaneous)
12. [Common Coding Questions](#12-common-coding-questions)

---

## 1. Data Types & Storage

**Q: What are the basic data types in C?**
A: `int`, `float`, `double`, `char`, `void`.

**Q: Difference between float and double?**
A: `float` is 4 bytes (single precision), `double` is 8 bytes (double precision).

**Q: What is the size of int, char, float, and pointer in 32-bit vs 64-bit systems?**
A:

* `int`: 4 bytes
* `char`: 1 byte
* `float`: 4 bytes
* `pointer`: 4 bytes (32-bit), 8 bytes (64-bit)

**Q: What is the use of typedef?**
A: Used to create alias names for existing data types. Example: `typedef unsigned int uint;`

---

## 2. Operators & Expressions

**Q: ++i vs i++?**
A: `++i` increments before use, `i++` increments after use.

**Q: Bitwise operators?**
A: `&` (AND), `|` (OR), `^` (XOR), `~` (NOT), `<<` (left shift), `>>` (right shift).

**Q: Result of sizeof('A')?**
A: 4 (as `'A'` is treated as an `int` in C).

**Q: Use of comma operator?**
A: Evaluates multiple expressions, returns the value of the last one.

---

## 3. Pointers & Memory

**Q: What is a pointer?**
A: A variable that stores the address of another variable.

\**Q: *ptr vs \&ptr?**
A: `*ptr` dereferences the pointer; `&ptr` gives the address of the pointer.

**Q: Dangling pointer?**
A: Pointer pointing to a freed memory location.

**Q: malloc vs calloc vs realloc vs free?**

* `malloc(size)`: Allocates uninitialized memory.
* `calloc(n, size)`: Allocates zero-initialized memory.
* `realloc(ptr, new_size)`: Resizes previously allocated memory.
* `free(ptr)`: Deallocates memory.

**Q: What is a memory leak?**
A: Memory not freed properly, leading to resource wastage.

---

## 4. Arrays & Strings

**Q: How are arrays stored in memory?**
A: Contiguously.

**Q: Difference between `char str[] = "abc";` and `char *str = "abc";`?**
A:

* `char[]` allocates on stack.
* `char*` points to a string literal (read-only).

**Q: strlen() vs sizeof()?**
A: `strlen()` counts characters before `\0`. `sizeof()` gives total allocated size.

---

## 5. Functions

**Q: Call by value vs call by reference?**
A:

* Call by value: Copy of data passed.
* Call by reference: Address passed (via pointers).

**Q: What is recursion?**
A: Function calling itself. E.g., factorial, Fibonacci.

**Q: Can you pass an array to a function?**
A: Yes, as a pointer.

---

## 6. Storage Classes

**Q: Storage classes?**
A: `auto`, `extern`, `static`, `register`.

**Q: Use of static in functions/variables?**
A:

* Inside function: Retains value between calls.
* Outside function: Limits scope to file.

---

## 7. Control Flow

**Q: break vs continue vs goto?**
A:

* `break`: Exits loop/switch.
* `continue`: Skips current iteration.
* `goto`: Jumps to a labeled statement.

**Q: Can switch be used with strings?**
A: No, only integral types (char/int).

**Q: Use of default in switch?**
A: Executes when no case matches.

---

## 8. Structures & Unions

**Q: struct vs union?**
A:

* `struct`: All members occupy separate memory.
* `union`: All members share the same memory.

**Q: Memory allocation difference?**
A: `struct` = sum of all members; `union` = size of largest member.

**Q: Can structure contain pointer to itself?**
A: Yes, typically used in linked lists.

---

## 9. File Handling

**Q: File opening modes?**
A: `"r"`, `"w"`, `"a"`, `"r+"`, `"w+"`, etc.

**Q: fscanf vs fgets?**
A: `fscanf` reads formatted input; `fgets` reads a line (safer for strings).

---

## 10. Compilation & Execution

**Q: Compile-time vs run-time error?**
A:

* Compile-time: Syntax/semantic errors.
* Run-time: Errors during execution (e.g., divide by zero).

**Q: What happens during compilation?**
A: Preprocessing → Compilation → Assembly → Linking → Executable.

**Q: Preprocessor directives?**
A:

* `#define`, `#include`, `#ifdef`, `#ifndef`, `#undef`

---

## 11. Miscellaneous

**Q: const keyword?**
A: Declares constant values; can't be modified after initialization.

**Q: volatile keyword?**
A: Tells compiler variable can change unexpectedly (used in hardware access, multithreading).

**Q: extern and register?**

* `extern`: Declare a variable defined elsewhere.
* `register`: Suggests variable be stored in CPU register.

**Q: exit() vs return?**
A:

* `return`: Exits function.
* `exit()`: Terminates program.

---

## 12. Common Coding Questions

```c
// 1. Reverse a string
void reverse(char *str) {
    int i = 0, j = strlen(str) - 1;
    while (i < j) {
        char temp = str[i];
        str[i] = str[j];
        str[j] = temp;
        i++; j--;
    }
}

// 2. Check palindrome
int isPalindrome(char *str) {
    int i = 0, j = strlen(str) - 1;
    while (i < j) {
        if (str[i++] != str[j--]) return 0;
    }
    return 1;
}

// 3. Count vowels
int countVowels(char *str) {
    int count = 0;
    for (int i = 0; str[i]; i++) {
        if (strchr("aeiouAEIOU", str[i])) count++;
    }
    return count;
}

// 4. Swap without temp
void swap(int *a, int *b) {
    *a = *a + *b;
    *b = *a - *b;
    *a = *a - *b;
}
```

---

