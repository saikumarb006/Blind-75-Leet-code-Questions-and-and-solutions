# Blind 75 LeetCode: Brute Force vs Optimized Solutions 

## Table of Contents
1. [Why Start with Brute Force?](#why-start-with-brute-force)
2. [Array Problems](#array-problems)
3. [String Problems](#string-problems)
4. [Linked List Problems](#linked-list-problems)
5. [Dynamic Programming](#dynamic-programming)
6. [Tree Problems](#tree-problems)
7. [General Optimization Patterns](#general-optimization-patterns)

---

## Why Start with Brute Force?

As a beginner, **always start with brute force** when solving coding problems[40][43][51]. Here's why:

1. **Builds Understanding**: Helps you truly understand the problem before optimizing
2. **Shows Progress**: A working solution is better than no solution
3. **Interview Strategy**: Demonstrates problem-solving approach to interviewers
4. **Foundation for Optimization**: Understanding the naive approach helps identify bottlenecks

**Remember**: In interviews, mention "I can start with brute force and then optimize" to show your thought process[40].

---

## Array Problems

### 1. Two Sum (LeetCode 1)

**Problem**: Find two numbers in array that add up to target.

#### 🐌 Brute Force Solution - O(n²)
```javascript
var twoSumBruteForce = function(nums, target) {
    // Check every possible pair of numbers
    for (let i = 0; i < nums.length; i++) {
        for (let j = i + 1; j < nums.length; j++) {
            // If we find a pair that adds up to target
            if (nums[i] + nums[j] === target) {
                return [i, j];
            }
        }
    }
    return []; // No solution found
};
```

**Why it's slow**: We check every possible pair. For each number, we compare it with ALL other numbers.
- First number: compared with n-1 numbers
- Second number: compared with n-2 numbers  
- Total comparisons: (n-1) + (n-2) + ... + 1 = n²/2 ≈ O(n²)

#### ⚡ Optimized Solution - O(n)
```javascript
var twoSum = function(nums, target) {
    const numMap = {}; // Hash map to store numbers we've seen
    
    for (let i = 0; i < nums.length; i++) {
        const complement = target - nums[i]; // What we need to find
        
        // Check if complement exists in our map (O(1) lookup!)
        if (complement in numMap) {
            return [numMap[complement], i];
        }
        
        // Store current number and its index
        numMap[nums[i]] = i;
    }
    return [];
};
```

**Why it's fast**: Instead of checking all numbers for each element, we use a hash map for instant lookup[41].
- Hash map lookup: O(1) time
- One pass through array: O(n) time
- **Key insight**: Trade space for time!

---

### 2. Contains Duplicate (LeetCode 217)

**Problem**: Check if any value appears at least twice in array.

#### 🐌 Brute Force Solution - O(n²)
```javascript
var containsDuplicateBruteForce = function(nums) {
    // For each number, check if it appears later in the array
    for (let i = 0; i < nums.length; i++) {
        for (let j = i + 1; j < nums.length; j++) {
            // If we find the same number later, it's a duplicate
            if (nums[i] === nums[j]) {
                return true;
            }
        }
    }
    return false; // No duplicates found
};
```

**Why it's slow**: For each element, we scan the entire rest of the array[52][55].

#### ⚡ Optimized Solution 1 - O(n log n)
```javascript
var containsDuplicateSort = function(nums) {
    // Sort the array first
    nums.sort((a, b) => a - b);
    
    // Check adjacent elements (duplicates will be next to each other)
    for (let i = 0; i < nums.length - 1; i++) {
        if (nums[i] === nums[i + 1]) {
            return true;
        }
    }
    return false;
};
```

**Why it's better**: After sorting, duplicates are adjacent. One pass to check neighbors[55].

#### ⚡ Optimized Solution 2 - O(n)
```javascript
var containsDuplicate = function(nums) {
    const seen = new Set();
    
    for (let num of nums) {
        // If we've seen this number before, it's a duplicate
        if (seen.has(num)) {
            return true;
        }
        // Remember this number
        seen.add(num);
    }
    return false;
};

// One-liner alternative:
var containsDuplicateOneLiner = function(nums) {
    return new Set(nums).size !== nums.length;
};
```

**Why it's fastest**: Set operations (add/has) are O(1). Single pass through array[58].

---

### 3. Best Time to Buy and Sell Stock (LeetCode 121)

**Problem**: Find maximum profit from buying and selling stock once.

#### 🐌 Brute Force Solution - O(n²)
```javascript
var maxProfitBruteForce = function(prices) {
    let maxProfit = 0;
    
    // Try every possible buy day
    for (let i = 0; i < prices.length; i++) {
        // Try every possible sell day after buy day
        for (let j = i + 1; j < prices.length; j++) {
            // Calculate profit if we buy on day i and sell on day j
            const profit = prices[j] - prices[i];
            maxProfit = Math.max(maxProfit, profit);
        }
    }
    
    return maxProfit;
};
```

**Why it's slow**: We check every possible buy-sell combination[42][45].
- For each buy day, check all future sell days
- Total combinations: n²/2 ≈ O(n²)

#### ⚡ Optimized Solution - O(n)
```javascript
var maxProfit = function(prices) {
    let minPrice = prices[0];  // Track cheapest price so far
    let maxProfit = 0;         // Track best profit so far
    
    for (let i = 1; i < prices.length; i++) {
        // Update minimum price if current price is lower
        minPrice = Math.min(minPrice, prices[i]);
        
        // Calculate profit if we sell today
        const todayProfit = prices[i] - minPrice;
        
        // Update maximum profit
        maxProfit = Math.max(maxProfit, todayProfit);
    }
    
    return maxProfit;
};
```

**Why it's fast**: We realize we only need to track the minimum price seen so far. For each day, we calculate profit if selling that day[1][24].
- **Key insight**: We don't need to check all combinations, just track minimum and calculate profit for each day!

---

### 4. Maximum Subarray (LeetCode 53)

**Problem**: Find contiguous subarray with largest sum.

#### 🐌 Brute Force Solution - O(n²)
```javascript
var maxSubArrayBruteForce = function(nums) {
    let maxSum = nums[0];
    
    // Try every possible starting position
    for (let i = 0; i < nums.length; i++) {
        let currentSum = 0;
        
        // Try every possible ending position from current start
        for (let j = i; j < nums.length; j++) {
            currentSum += nums[j];
            maxSum = Math.max(maxSum, currentSum);
        }
    }
    
    return maxSum;
};
```

**Even worse O(n³) brute force**:
```javascript
var maxSubArrayWorst = function(nums) {
    let maxSum = nums[0];
    
    for (let i = 0; i < nums.length; i++) {
        for (let j = i; j < nums.length; j++) {
            // Calculate sum of subarray from i to j
            let currentSum = 0;
            for (let k = i; k <= j; k++) {
                currentSum += nums[k];
            }
            maxSum = Math.max(maxSum, currentSum);
        }
    }
    
    return maxSum;
};
```

**Why O(n³) is worse**: For each start-end pair, we recalculate the entire sum[53][56].

#### ⚡ Optimized Solution - Kadane's Algorithm O(n)
```javascript
var maxSubArray = function(nums) {
    let maxSoFar = nums[0];      // Best sum found so far
    let maxEndingHere = nums[0]; // Best sum ending at current position
    
    for (let i = 1; i < nums.length; i++) {
        // Either extend existing subarray or start new one
        maxEndingHere = Math.max(nums[i], maxEndingHere + nums[i]);
        
        // Update overall maximum
        maxSoFar = Math.max(maxSoFar, maxEndingHere);
    }
    
    return maxSoFar;
};
```

**Why it's fast**: Kadane's algorithm makes a key observation: at each position, we only need to decide whether to extend the previous subarray or start fresh[53][62].
- **Key insight**: If previous sum + current element < current element, start fresh!

---

## String Problems

### 1. Valid Parentheses (LeetCode 20)

**Problem**: Check if parentheses are properly matched.

#### 🐌 Brute Force Approach - Not Really Applicable
```javascript
// There's no obvious brute force for this problem
// You might try removing matched pairs repeatedly:
var isValidBruteForce = function(s) {
    let str = s;
    let changed = true;
    
    while (changed) {
        changed = false;
        let newStr = str.replace(/\(\)|\[\]|\{\}/g, '');
        if (newStr !== str) {
            changed = true;
            str = newStr;
        }
    }
    
    return str === '';
};
```

**Why this is bad**: We might need to scan the string multiple times[54].

#### ⚡ Optimized Solution - Stack O(n)
```javascript
var isValid = function(s) {
    const stack = [];
    const mapping = {
        ')': '(',
        '}': '{',
        ']': '['
    };
    
    for (let char of s) {
        if (char in mapping) {
            // Closing bracket - check if it matches
            const topElement = stack.pop();
            if (topElement !== mapping[char]) {
                return false;
            }
        } else {
            // Opening bracket - push to stack
            stack.push(char);
        }
    }
    
    // Valid if stack is empty
    return stack.length === 0;
};
```

**Why stack is perfect**: Stack follows LIFO (Last In, First Out) - perfect for matching nested structures[54][57].
- **Key insight**: Most recent opening bracket should match current closing bracket!

---

### 2. Valid Anagram (LeetCode 242)

**Problem**: Check if two strings are anagrams.

#### 🐌 Brute Force Solution - O(n log n)
```javascript
var isAnagramSort = function(s, t) {
    if (s.length !== t.length) return false;
    
    // Sort both strings and compare
    const sortedS = s.split('').sort().join('');
    const sortedT = t.split('').sort().join('');
    
    return sortedS === sortedT;
};
```

**Why it works but is slow**: Sorting takes O(n log n) time.

#### ⚡ Optimized Solution - O(n)
```javascript
var isAnagram = function(s, t) {
    if (s.length !== t.length) return false;
    
    const charCount = {};
    
    // Count characters in first string
    for (let char of s) {
        charCount[char] = (charCount[char] || 0) + 1;
    }
    
    // Subtract character counts for second string
    for (let char of t) {
        if (!charCount[char]) return false;
        charCount[char]--;
    }
    
    return true;
};
```

**Why it's faster**: Count characters in O(n) time instead of sorting[41].

---

## Dynamic Programming

### 1. Climbing Stairs (LeetCode 70)

**Problem**: Count ways to climb n stairs (1 or 2 steps at a time).

#### 🐌 Brute Force - Recursive O(2ⁿ)
```javascript
var climbStairsBruteForce = function(n) {
    // Base cases
    if (n <= 2) return n;
    
    // Try both 1 step and 2 steps
    return climbStairsBruteForce(n - 1) + climbStairsBruteForce(n - 2);
};
```

**Why it's extremely slow**: We recalculate the same values many times!
- climbStairs(5) = climbStairs(4) + climbStairs(3)
- climbStairs(4) = climbStairs(3) + climbStairs(2)  
- We calculate climbStairs(3) twice!

#### ⚡ Optimized Solution - O(n)
```javascript
var climbStairs = function(n) {
    if (n <= 2) return n;
    
    let prev2 = 1; // Ways to reach step i-2
    let prev1 = 2; // Ways to reach step i-1
    
    for (let i = 3; i <= n; i++) {
        let current = prev1 + prev2; // Ways to reach current step
        prev2 = prev1;
        prev1 = current;
    }
    
    return prev1;
};
```

**Why it's fast**: Calculate each value once and reuse results[35][37].
- **Key insight**: This is Fibonacci sequence - f(n) = f(n-1) + f(n-2)!

---

## Tree Problems

### 1. Maximum Depth of Binary Tree (LeetCode 104)

**Problem**: Find maximum depth of binary tree.

#### 🐌 Brute Force - Not Really Applicable
```javascript
// There's no "brute force" for tree traversal
// The recursive solution IS the natural approach
```

#### ⚡ Optimized Solutions

**Recursive DFS - O(n)**:
```javascript
var maxDepth = function(root) {
    if (root === null) return 0;
    
    const leftDepth = maxDepth(root.left);
    const rightDepth = maxDepth(root.right);
    
    return 1 + Math.max(leftDepth, rightDepth);
};
```

**Iterative BFS - O(n)**:
```javascript
var maxDepthBFS = function(root) {
    if (root === null) return 0;
    
    const queue = [[root, 1]]; // [node, depth]
    let maxDepth = 0;
    
    while (queue.length > 0) {
        const [node, depth] = queue.shift();
        maxDepth = Math.max(maxDepth, depth);
        
        if (node.left) queue.push([node.left, depth + 1]);
        if (node.right) queue.push([node.right, depth + 1]);
    }
    
    return maxDepth;
};
```

**Both are optimal**: Tree problems often don't have "brute force" - the natural traversal IS the solution.

---

## General Optimization Patterns

### 1. **Time vs Space Trade-offs**

**Pattern**: Use extra memory to avoid repeated calculations
- **Two Sum**: Hash map for O(1) lookup instead of O(n) search
- **Contains Duplicate**: Set for O(1) membership test

### 2. **Avoid Nested Loops**

**Pattern**: Find ways to process data in single pass
- **Two Sum**: One pass with hash map instead of nested loops
- **Best Time to Buy/Sell**: Track minimum while iterating

### 3. **Sort First**

**Pattern**: Sometimes sorting helps reduce complexity
- **Contains Duplicate**: O(n log n) sort + O(n) check vs O(n²) nested loops
- **Valid Anagram**: Sort both strings and compare

### 4. **Use Appropriate Data Structures**

**Pattern**: Choose data structure that matches access pattern
- **Valid Parentheses**: Stack for LIFO matching
- **Contains Duplicate**: Set for membership testing

### 5. **Dynamic Programming**

**Pattern**: Break down problem into subproblems, avoid recalculation
- **Climbing Stairs**: Store previous results instead of recalculating
- **Maximum Subarray**: Use Kadane's algorithm instead of checking all subarrays

---

## Study Strategy for Beginners

### Phase 1: Understand the Brute Force
1. **Always start with brute force** - even if it's slow
2. **Understand WHY it's slow** - identify the bottleneck
3. **Code it first** - get a working solution

### Phase 2: Identify Optimization Opportunities
1. **Look for repeated work** - are you calculating the same thing multiple times?
2. **Check your data access patterns** - are you searching through arrays repeatedly?
3. **Consider trade-offs** - can you use more memory to save time?

### Phase 3: Apply Common Patterns
1. **Hash maps/Sets** for fast lookups
2. **Two pointers** for array problems
3. **Stack/Queue** for order-dependent problems
4. **Dynamic Programming** for optimization problems

### Phase 4: Practice Implementation
1. **Code both versions** - brute force AND optimized
2. **Analyze complexity** - time and space for both
3. **Test edge cases** - empty arrays, single elements, etc.

---

## Complexity Analysis Cheat Sheet

### Common Time Complexities (Best to Worst):
- **O(1)** - Hash map lookup, array access
- **O(log n)** - Binary search, balanced tree operations  
- **O(n)** - Single loop, linear search
- **O(n log n)** - Efficient sorting, heap operations
- **O(n²)** - Nested loops, bubble sort
- **O(2ⁿ)** - Recursive without memoization

### Space Complexity:
- **O(1)** - Few variables, in-place algorithms
- **O(n)** - Hash map, additional array
- **O(h)** - Recursion depth (h = height of tree)

Remember: Start simple, then optimize. Understanding the brute force approach is crucial for building intuition about more efficient solutions!
