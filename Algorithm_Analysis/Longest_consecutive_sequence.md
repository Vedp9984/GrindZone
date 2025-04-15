# Longest Consecutive Sequence (Optimal Approach using Set)

## Problem Statement

Given an unsorted array of integers, find the length of the longest sequence of consecutive elements.

**Example:**
- **Input:** $[100, 4, 200, 1, 3, 2]$
- **Output:** $4$
- **Explanation:** The longest consecutive sequence is $[1, 2, 3, 4]$.

## Intuition

Instead of checking for each element in the array whether it is part of a sequence, we optimize by only checking for starting points of sequences.

**Key Observation:**
A number $x$ can be a starting point of a sequence only if $x - 1$ is not present in the array.

## Algorithm

1. Insert all elements of the array into a set for constant-time lookup.

2. Initialize a variable $longest$ to store the length of the longest sequence.

3. For each number $num$ in the set:
  - If $num - 1$ is not in the set:
    - It means $num$ is the start of a new sequence.
    - Initialize $current\_num = num$ and $cnt = 1$.
    - While $current\_num + 1$ exists in the set:
     - Increment $current\_num$ and $cnt$.
    - Update $longest = max(longest, cnt)$.

4. Return $longest$.

## Pseudo Code

```
function findLongestConsecutiveSequence(arr):
   s = empty set
   for each num in arr:
      add num to s

   longest = 0

   for each num in s:
      if (num - 1) not in s:
        current_num = num
        cnt = 1

        while (current_num + 1) in s:
           current_num = current_num + 1
           cnt = cnt + 1

        longest = max(longest, cnt)

   return longest

## C++ Implementation

```cpp
#include <iostream>
#include <vector>
#include <unordered_set>
#include <algorithm>

int longestConsecutive(std::vector<int>& nums) {
  // Create an unordered set to store all numbers
  std::unordered_set<int> numSet;
  for (int num : nums) {
    numSet.insert(num);
  }
  
  int longest = 0;
  
  // Check each number if it's the start of a sequence
  for (int num : numSet) {
    // Only process if it's the start of a sequence
    if (numSet.find(num - 1) == numSet.end()) {
      int currentNum = num;
      int currentStreak = 1;
      
      // Continue checking for consecutive elements
      while (numSet.find(currentNum + 1) != numSet.end()) {
        currentNum++;
        currentStreak++;
      }
      
      // Update the maximum length found
      longest = std::max(longest, currentStreak);
    }
  }
  
  return longest;
}

int main() {
  std::vector<int> nums = {100, 4, 200, 1, 3, 2};
  std::cout << "Length of longest consecutive sequence: " 
        << longestConsecutive(nums) << std::endl;
  return 0;
}
```

```

## Time and Space Complexity

- **Time Complexity:** $O(n)$, where $n$ is the size of the array
  (Each number is processed only once in the worst case)

- **Space Complexity:** $O(n)$, for storing the elements in the set

## Summary

By ensuring we only begin to search from sequence starting points, and leveraging the power of sets for $O(1)$ lookups, we significantly optimize the brute-force solution to $O(n)$. This approach avoids nested loops and unnecessary computations.
