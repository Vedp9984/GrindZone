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

**Q: *ptr vs &ptr?**
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


**Q: What is pointer arithmetic and its limitations?**
A: Operations like addition/subtraction on pointers. Limited to same data type; cannot multiply/divide pointers.

**Q: What are void pointers and their limitations?**
A: Generic pointers that can point to any data type. Cannot be dereferenced without casting; pointer arithmetic not possible.

**Q: Explain the concept of function pointers?**
A: Pointers that point to functions instead of data. Used for callbacks, creating dispatch tables, etc.

**Q: What happens when you dereference a NULL pointer?**
A: Results in a segmentation fault/access violation during runtime.

**Q: What is a near, far, and huge pointer?**
A: Memory model concepts in older C implementations. Near (16-bit), far (32-bit segment:offset), huge (like far but normalized).

**Q: How would you implement a memory pool/allocator?**
A: Pre-allocate a large chunk of memory and manage it manually to reduce malloc/free overhead.

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

**Q: What happens when you access an array out of bounds?**
A: Undefined behavior - might cause segmentation fault, corrupt memory, or work incorrectly.

**Q: Can array name be used as a pointer?**
A: Yes, array name decays into a pointer to the first element, but cannot be reassigned.

**Q: What are multi-dimensional arrays in C?**
A: Arrays of arrays, stored in row-major order (e.g., `int arr[3][4]`).

**Q: How to dynamically allocate a 2D array?**
A: Either as array of pointers (`int **arr`) or as contiguous block with pointer arithmetic.

**Q: What is the difference between arrays and linked lists?**
A: Arrays are contiguous with O(1) access; linked lists have non-contiguous elements with O(n) access but flexible size.

**Q: How would you efficiently find duplicate elements in an array?**
A: Using hash tables, sorting followed by linear scan, or bit manipulation (depending on constraints).

**Q: How to pass a 2D array to a function?**
A: Using pointer to array: `void func(int (*arr)[COLS])` or with explicit column size: `void func(int arr[][COLS])`.

**Q: Explain string interning in C?**
A: Compiler optimization where identical string literals share the same memory location.

---

## 5. Functions

**Q: Call by value vs call by reference?**
A:

* Call by value: Copy of data passed.
* Call by reference: Address passed (via pointers).


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
A: `"r"`, `"w"`, `"a"`, `"r+"`, `"w+"`, `"a+"`, `"rb"`, `"wb"`, `"ab"`, etc.

**Q: fscanf vs fgets?**
A: `fscanf` reads formatted input; `fgets` reads a line (safer for strings).

**Q: What's the difference between text and binary file modes?**
A: Text mode translates newlines; binary mode doesn't perform any translations.

**Q: How do you check for end-of-file?**
A: Using the `feof()` function or checking return values of `fread()`, `fscanf()`, etc.

**Q: What is a file pointer?**
A: A pointer to the FILE structure that contains information about the file being accessed.

**Q: How do you move within a file?**
A: Using `fseek()`, `ftell()`, and `rewind()` functions.

**Q: What's the difference between fwrite() and fputs()?**
A: `fwrite()` writes binary data; `fputs()` writes strings.

**Q: How to flush a file stream?**
A: Using `fflush()` function to clear the buffer.

**Q: What happens if you don't close a file?**
A: Resources remain allocated until program termination, potentially causing memory leaks or data loss.

**Q: How to read a file character by character?**
A: Using `fgetc()` or `getc()` functions.

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

### 1. What are tokens in C?

Tokens are the smallest elements of a C program, including:

* Keywords
* Identifiers
* Constants
* Operators
* Punctuators

---

### 2. Difference between declaration and definition?

* **Declaration:** Announces the existence of a variable or function.

  ```c
  extern int x; // declaration only
  ```
* **Definition:** Allocates memory (and may initialize).

  ```c
  int x = 10;  // definition
  ```

---

### 3. Print without semicolon?

```c
#include <stdio.h>
int main() {
    if (printf("Hello World")) {}
    return 0;
}
```

---

### 4. What is a NULL pointer?

A pointer that doesn't point to any valid memory location. It is defined as:

```c
#define NULL ((void*)0)
```

---

### 5. What is a dangling pointer?

A pointer pointing to a memory location that has been freed or goes out of scope.

---

### 6. What is a wild pointer?

A pointer that is declared but not initialized.

```c
int *ptr; // wild pointer
```

---

### 7. What are local static variables?

They retain their values between function calls.

```c
void fun() {
    static int x = 0;
    x++;
    printf("%d ", x);
}
```

---

### 8. What is a static function?

Functions declared with `static` are accessible only within the file.

---

### 9. Use of `_Noreturn`?

Indicates that a function does not return:

```c
_Noreturn void fatal_error() {
    exit(1);
}
```

---

### 10. What is `__func__`?

A predefined identifier containing the name of the function as a string literal.

---

### 11. Can you pass an array by value in C?

Not directly. Arrays decay to pointers. Use `struct` to wrap the array if needed.

---

### 12. What are variadic functions?

Functions that accept a variable number of arguments, like `printf()`.

```c
#include <stdarg.h>
void example(int count, ...) {
    va_list args;
    va_start(args, count);
    // access args using va_arg
    va_end(args);
}
```

---

### 13. Are nested functions allowed in C?

Not in standard C. GCC allows it as an extension.

---

### 14. What is a function pointer?

A variable that stores the address of a function.

```c
int add(int a, int b) { return a + b; }
int (*fp)(int, int) = add;
printf("%d", fp(2, 3));
```

---

### 15. Difference between `malloc` and `calloc`?

* `malloc(size)` – allocates memory of `size` bytes.
* `calloc(n, size)` – allocates and initializes to zero.

---

### 16. What are memory leaks?

Memory allocated but not freed, causing resource wastage.

---

### 17. What are lvalue and rvalue?

* **lvalue** – refers to an object with an address
* **rvalue** – data value that doesn’t persist (e.g., literals)

---

### 18. What is typecasting?

Converting a variable from one type to another:

```c
float x = 5.5;
int y = (int)x; // y becomes 5
```

---

### 19. What are volatile variables?

Tells the compiler not to optimize the variable; it may change externally.

```c
volatile int flag;
```

---

### 20. What is the difference between `const` and `#define`?

* `const int x = 10;` has type checking
* `#define X 10` is a macro replaced at preprocessor stage

---

=

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


##  malloc/calloc/realloc Coding Questions

### 1. Write a program to allocate memory dynamically using malloc for an array of `n` integers and initialize them.

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int n;
    printf("Enter number of elements: ");
    scanf("%d", &n);

    int *arr = (int *)malloc(n * sizeof(int));
    if (arr == NULL) {
        printf("Memory allocation failed!\n");
        return 1;
    }

    for (int i = 0; i < n; i++) {
        arr[i] = i + 1;
    }

    printf("Array: ");
    for (int i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }
    free(arr);
    return 0;
}
```

---

### 2. Modify the above program using calloc and explain the difference.

```c
int *arr = (int *)calloc(n, sizeof(int));
```

* `calloc` initializes memory to zero, unlike `malloc`.

---

### 3. Write a C program to increase the size of an allocated array using `realloc`.

```c
arr = (int *)realloc(arr, 2 * n * sizeof(int));
if (arr == NULL) {
    printf("Memory reallocation failed!\n");
    return 1;
}
for (int i = n; i < 2 * n; i++) {
    arr[i] = i + 1;
}
```

---

### 4. What happens if you pass NULL to `realloc`?

It behaves like `malloc` and allocates new memory.

```c
int *ptr = realloc(NULL, sizeof(int) * 10); // same as malloc
```

---

### 5. What happens if you `realloc` to size 0?

Implementation-defined: may return NULL or a pointer that must not be dereferenced.

```c
ptr = realloc(ptr, 0); // May behave like free(ptr)
```

---

### 6. Use malloc to create a matrix dynamically.

```c
int **matrix = malloc(rows * sizeof(int*));
for (int i = 0; i < rows; i++) {
    matrix[i] = malloc(cols * sizeof(int));
}
```

---

### 7. Freeing nested dynamic memory.

```c
for (int i = 0; i < rows; i++) {
    free(matrix[i]);
}
free(matrix);
```

---

### 8. Detecting memory leaks.

* Use tools like `valgrind` on Linux:

```sh
valgrind ./a.out
```

---



