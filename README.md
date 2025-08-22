# Blind 75 LeetCode: Brute Force vs Optimized Solutions with Test Cases

## Table of Contents
1. [Why Include Test Cases?](#why-include-test-cases)
2. [Array Problems](#array-problems)
3. [String Problems](#string-problems)
4. [Dynamic Programming](#dynamic-programming)
5. [Tree Problems](#tree-problems)
6. [Testing Your Solutions](#testing-your-solutions)

---

## Why Include Test Cases?

Including sample payloads with expected outputs is crucial for beginners because:

1. **Validates Understanding**: Test cases confirm you understand the problem requirements
2. **Edge Case Identification**: Helps identify corner cases you might miss
3. **Debugging**: Makes it easier to spot where your solution fails
4. **Interview Practice**: Real interviews often involve working through examples
5. **Build Confidence**: Passing test cases gives you confidence in your solution

**Testing Strategy**: Always test with:
- Happy path (normal cases)
- Edge cases (empty arrays, single elements)
- Boundary conditions (minimum/maximum values)
- Invalid inputs (where applicable)

---

## Array Problems

### 1. Two Sum (LeetCode 1)

**Problem**: Find two numbers in array that add up to target.

#### Test Cases:
```javascript
// Test Case 1 - Basic case
Input: nums = [2, 7, 11, 15], target = 9
Expected Output: [0, 1]
Explanation: nums[0] + nums[1] = 2 + 7 = 9

// Test Case 2 - Different positions
Input: nums = [3, 2, 4], target = 6
Expected Output: [1, 2]
Explanation: nums[1] + nums[2] = 2 + 4 = 6

// Test Case 3 - Same number, different indices
Input: nums = [3, 3], target = 6
Expected Output: [0, 1]
Explanation: nums[0] + nums[1] = 3 + 3 = 6

// Test Case 4 - Negative numbers
Input: nums = [1, 4, 10, -3], target = 14
Expected Output: [1, 2]
Explanation: nums[1] + nums[2] = 4 + 10 = 14

// Test Case 5 - Negative target
Input: nums = [1, -2, 5, 10], target = -1
Expected Output: [0, 1]
Explanation: nums[0] + nums[1] = 1 + (-2) = -1
```

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

// Test the brute force solution
console.log(twoSumBruteForce([2, 7, 11, 15], 9)); // [0, 1]
console.log(twoSumBruteForce([3, 2, 4], 6));      // [1, 2]
console.log(twoSumBruteForce([3, 3], 6));         // [0, 1]
```

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

// Test the optimized solution
console.log(twoSum([2, 7, 11, 15], 9)); // [0, 1]
console.log(twoSum([3, 2, 4], 6));      // [1, 2]
console.log(twoSum([3, 3], 6));         // [0, 1]
console.log(twoSum([1, 4, 10, -3], 14)); // [1, 2]
console.log(twoSum([1, -2, 5, 10], -1)); // [0, 1]
```
> **Explanation of Optimization:**
> - We use a hash map to store each number's index.
> - For each element, compute required `complement = target - currentValue`.
> - If `complement` already in map, we found our pair in O(1) lookup.
> - Achieves single-pass solution, O(n) time, O(n) space.

---

### 2. Contains Duplicate (LeetCode 217)

**Problem**: Check if any value appears at least twice in array.

#### Test Cases:
```javascript
// Test Case 1 - Contains duplicate
Input: nums = [1, 2, 3, 1]
Expected Output: true
Explanation: 1 appears at index 0 and 3

// Test Case 2 - No duplicates
Input: nums = [1, 2, 3, 4]
Expected Output: false
Explanation: All elements are distinct

// Test Case 3 - All same elements
Input: nums = [1, 1, 1, 3, 3, 4, 3, 2, 4, 2]
Expected Output: true
Explanation: Multiple duplicates exist

// Test Case 4 - Single element
Input: nums = [1]
Expected Output: false
Explanation: Only one element, no duplicates possible

// Test Case 5 - Two identical elements
Input: nums = [1, 2, 4, 3, 1]
Expected Output: true
Explanation: 1 appears twice
```

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

// Test brute force
console.log(containsDuplicateBruteForce([1, 2, 3, 1])); // true
console.log(containsDuplicateBruteForce([1, 2, 3, 4])); // false
console.log(containsDuplicateBruteForce([1]));          // false
```

#### ⚡ Optimized Solution - O(n)
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

// Test optimized solution
console.log(containsDuplicate([1, 2, 3, 1])); // true
console.log(containsDuplicate([1, 2, 3, 4])); // false
console.log(containsDuplicate([1, 1, 1, 3, 3, 4, 3, 2, 4, 2])); // true
console.log(containsDuplicate([1])); // false
console.log(containsDuplicate([1, 2, 4, 3, 1])); // true
```

> **Explanation:**
> - A `Set` stores unique values.
> - As we iterate, if a number is already in the set, it’s a duplicate.
> - Set lookup and insertion are O(1), single pass O(n).

---

### 3. Best Time to Buy and Sell Stock (LeetCode 121)

**Problem**: Find maximum profit from buying and selling stock once.

#### Test Cases:
```javascript
// Test Case 1 - Basic profit case
Input: prices = [7, 1, 5, 3, 6, 4]
Expected Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5

// Test Case 2 - No profit possible (decreasing prices)
Input: prices = [7, 6, 4, 3, 1]
Expected Output: 0
Explanation: Prices only decrease, so no profit can be made

// Test Case 3 - All increasing prices
Input: prices = [1, 2, 3, 4, 5]
Expected Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4

// Test Case 4 - Single price
Input: prices = [1]
Expected Output: 0
Explanation: Need at least 2 days to buy and sell

// Test Case 5 - Two prices
Input: prices = [2, 1]
Expected Output: 0
Explanation: Price decreases, so no profit possible

// Test Case 6 - Two prices with profit
Input: prices = [1, 2]
Expected Output: 1
Explanation: Buy at 1, sell at 2, profit = 1
```

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

// Test brute force
console.log(maxProfitBruteForce([7, 1, 5, 3, 6, 4])); // 5
console.log(maxProfitBruteForce([7, 6, 4, 3, 1]));    // 0
console.log(maxProfitBruteForce([1]));                 // 0
```

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

// Test optimized solution
console.log(maxProfit([7, 1, 5, 3, 6, 4])); // 5
console.log(maxProfit([7, 6, 4, 3, 1]));    // 0
console.log(maxProfit([1, 2, 3, 4, 5]));    // 4
console.log(maxProfit([1]));                 // 0
console.log(maxProfit([2, 1]));              // 0
console.log(maxProfit([1, 2]));              // 1
```

---

### 4. Maximum Subarray (LeetCode 53)

**Problem**: Find contiguous subarray with largest sum.

#### Test Cases:
```javascript
// Test Case 1 - Mixed positive and negative
Input: nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
Expected Output: 6
Explanation: [4, -1, 2, 1] has the largest sum = 6

// Test Case 2 - Single element
Input: nums = [1]
Expected Output: 1
Explanation: Array has only one element

// Test Case 3 - All positive numbers
Input: nums = [5, 4, -1, 7, 8]
Expected Output: 23
Explanation: [5, 4, -1, 7, 8] has the largest sum = 23

// Test Case 4 - All negative numbers
Input: nums = [-2, -4]
Expected Output: -2
Explanation: [-2] has the largest sum = -2

// Test Case 5 - Zero included
Input: nums = [-1, 0, -2]
Expected Output: 0
Explanation: [0] has the largest sum = 0
```

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

// Test brute force
console.log(maxSubArrayBruteForce([-2, 1, -3, 4, -1, 2, 1, -5, 4])); // 6
console.log(maxSubArrayBruteForce([1]));                               // 1
console.log(maxSubArrayBruteForce([-2, -4]));                         // -2
```

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

// Test Kadane's algorithm
console.log(maxSubArray([-2, 1, -3, 4, -1, 2, 1, -5, 4])); // 6
console.log(maxSubArray([1]));                               // 1
console.log(maxSubArray([5, 4, -1, 7, 8]));                // 23
console.log(maxSubArray([-2, -4]));                         // -2
console.log(maxSubArray([-1, 0, -2]));                      // 0
```

---

## String Problems

### 1. Valid Parentheses (LeetCode 20)

**Problem**: Check if parentheses are properly matched.

#### Test Cases:
```javascript
// Test Case 1 - Valid simple case
Input: s = "()"
Expected Output: true
Explanation: Single pair of parentheses, properly matched

// Test Case 2 - Valid multiple types
Input: s = "()[]{}"
Expected Output: true
Explanation: Three different types of brackets, all properly matched

// Test Case 3 - Valid nested brackets
Input: s = "([{}])"
Expected Output: true
Explanation: Properly nested brackets

// Test Case 4 - Invalid - wrong order
Input: s = "(]"
Expected Output: false
Explanation: Opening '(' but closing ']'

// Test Case 5 - Invalid - unmatched opening
Input: s = "([)]"
Expected Output: false
Explanation: '[' is closed by ')' instead of ']'

// Test Case 6 - Invalid - unmatched closing
Input: s = "]"
Expected Output: false
Explanation: No opening bracket for ']'

// Test Case 7 - Empty string
Input: s = ""
Expected Output: true
Explanation: Empty string is considered valid
```
#### Step-by-Step Explanation
1. **Use a stack** to track opening brackets.
2. **Mapping**: `')'→'(', ']'→'[', '}'→'{'`.
3. **Algorithm**:
   - For each char:
     - If opening: push onto stack.
     - If closing: check stack not empty, pop and compare to mapping. If mismatch, invalid.
   - After loop, stack must be empty for valid string.

#### ⚡ Stack Solution - O(n)
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

// Test the solution
console.log(isValid("()"));        // true
console.log(isValid("()[]{}")); // true
console.log(isValid("([{}])"));    // true
console.log(isValid("(]"));        // false
console.log(isValid("([)]"));      // false
console.log(isValid("]"));         // false
console.log(isValid(""));          // true
```

---

### 2. Valid Anagram (LeetCode 242)

**Problem**: Check if two strings are anagrams.

#### Test Cases:
```javascript
// Test Case 1 - Valid anagram
Input: s = "anagram", t = "nagaram"
Expected Output: true
Explanation: Both contain same characters with same frequency

// Test Case 2 - Not anagram
Input: s = "rat", t = "car"
Expected Output: false
Explanation: Different characters

// Test Case 3 - Different lengths
Input: s = "a", t = "ab"
Expected Output: false
Explanation: Different lengths cannot be anagrams

// Test Case 4 - Same string
Input: s = "listen", t = "silent"
Expected Output: true
Explanation: Perfect anagram

// Test Case 5 - Empty strings
Input: s = "", t = ""
Expected Output: true
Explanation: Both empty, considered valid anagram
```

#### 🐌 Brute Force Solution - O(n log n)
```javascript
var isAnagramSort = function(s, t) {
    if (s.length !== t.length) return false;
    
    // Sort both strings and compare
    const sortedS = s.split('').sort().join('');
    const sortedT = t.split('').sort().join('');
    
    return sortedS === sortedT;
};

// Test sorting approach
console.log(isAnagramSort("anagram", "nagaram")); // true
console.log(isAnagramSort("rat", "car"));         // false
console.log(isAnagramSort("a", "ab"));            // false
```

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

// Test optimized solution
console.log(isAnagram("anagram", "nagaram")); // true
console.log(isAnagram("rat", "car"));         // false
console.log(isAnagram("a", "ab"));            // false
console.log(isAnagram("listen", "silent"));   // true
console.log(isAnagram("", ""));               // true
```

---

## Dynamic Programming

### 1. Climbing Stairs (LeetCode 70)

**Problem**: Count ways to climb n stairs (1 or 2 steps at a time).

#### Test Cases:
```javascript
// Test Case 1 - Small number
Input: n = 2
Expected Output: 2
Explanation: Two ways: (1+1) or (2)

// Test Case 2 - Medium number
Input: n = 3
Expected Output: 3
Explanation: Three ways: (1+1+1), (1+2), or (2+1)

// Test Case 3 - Larger number
Input: n = 4
Expected Output: 5
Explanation: Five ways: (1+1+1+1), (1+1+2), (1+2+1), (2+1+1), (2+2)

// Test Case 4 - Base case
Input: n = 1
Expected Output: 1
Explanation: Only one way: (1)

// Test Case 5 - Another larger case
Input: n = 5
Expected Output: 8
Explanation: Follows Fibonacci pattern: 1, 2, 3, 5, 8
```

#### 🐌 Brute Force - Recursive O(2ⁿ)
```javascript
var climbStairsBruteForce = function(n) {
    // Base cases
    if (n <= 2) return n;
    
    // Try both 1 step and 2 steps
    return climbStairsBruteForce(n - 1) + climbStairsBruteForce(n - 2);
};

// Test brute force (warning: slow for large n)
console.log(climbStairsBruteForce(2));  // 2
console.log(climbStairsBruteForce(3));  // 3
console.log(climbStairsBruteForce(4));  // 5
// Don't test large numbers with brute force!
```

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

// Test optimized solution
console.log(climbStairs(1));  // 1
console.log(climbStairs(2));  // 2
console.log(climbStairs(3));  // 3
console.log(climbStairs(4));  // 5
console.log(climbStairs(5));  // 8
console.log(climbStairs(10)); // 89 (works fast even for larger numbers)
```

---

## Testing Your Solutions

### Creating a Test Framework

```javascript
// Simple test framework
function runTests(functionName, testCases) {
    console.log(`\n=== Testing ${functionName} ===`);
    let passed = 0;
    let total = testCases.length;
    
    testCases.forEach((testCase, index) => {
        const { input, expected, description } = testCase;
        let result;
        
        // Handle different input formats
        if (Array.isArray(input)) {
            result = eval(functionName)(...input);
        } else {
            result = eval(functionName)(input);
        }
        
        const success = JSON.stringify(result) === JSON.stringify(expected);
        
        if (success) {
            console.log(`✅ Test ${index + 1}: ${description}`);
            passed++;
        } else {
            console.log(`❌ Test ${index + 1}: ${description}`);
            console.log(`   Expected: ${JSON.stringify(expected)}`);
            console.log(`   Got: ${JSON.stringify(result)}`);
        }
    });
    
    console.log(`\nPassed: ${passed}/${total} tests`);
}

// Example usage for Two Sum
const twoSumTests = [
    {
        input: [[2, 7, 11, 15], 9],
        expected: [0, 1],
        description: "Basic case with target 9"
    },
    {
        input: [[3, 2, 4], 6],
        expected: [1, 2],
        description: "Different positions"
    },
    {
        input: [[3, 3], 6],
        expected: [0, 1],
        description: "Same numbers"
    }
];

// Run tests
runTests('twoSum', twoSumTests);
```

### Edge Case Testing Checklist

For each problem, always test:

**Array Problems:**
- Empty array: `[]`
- Single element: `[1]`
- Two elements: `[1, 2]`
- All same elements: `[5, 5, 5]`
- Negative numbers: `[-1, -2, -3]`
- Large numbers: `[1000000, 999999]`

**String Problems:**
- Empty string: `""`
- Single character: `"a"`
- All same characters: `"aaaa"`
- Mixed case: `"AaBbCc"`

**Numeric Problems:**
- Zero: `0`
- Negative numbers: `-5`
- Large numbers: `1000`
- Boundary values: `1`, `2` for problems with small constraints

### Performance Testing

```javascript
// Simple performance testing
function timeFunction(func, input, iterations = 1000) {
    const start = performance.now();
    
    for (let i = 0; i < iterations; i++) {
        if (Array.isArray(input)) {
            func(...input);
        } else {
            func(input);
        }
    }
    
    const end = performance.now();
    return (end - start) / iterations;
}

// Compare brute force vs optimized
const testArray = Array.from({length: 1000}, (_, i) => i);
const target = 1500;

console.log('Brute Force Average Time:', 
    timeFunction(twoSumBruteForce, [testArray, target], 100), 'ms');
console.log('Optimized Average Time:', 
    timeFunction(twoSum, [testArray, target], 100), 'ms');
```

## Key Testing Principles

1. **Test Early and Often**: Write tests as you develop solutions
2. **Cover Edge Cases**: Don't just test happy paths
3. **Compare Approaches**: Verify both brute force and optimized give same results
4. **Performance Validation**: Confirm optimization actually improves performance
5. **Read Problem Carefully**: Ensure you understand constraints and requirements

Remember: In interviews, walking through test cases demonstrates your understanding and catches bugs before the interviewer does!
