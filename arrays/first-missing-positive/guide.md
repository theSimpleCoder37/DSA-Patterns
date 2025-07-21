># 41. First Missing Positive
---
### Step 1: Understanding the Problem (Breaking It Down)

#### Tell a Simple Story
Imagine you’re organizing a big party, and you’re assigning numbered wristbands to guests, starting from 1 (like 1, 2, 3, …). You have a list of wristband numbers that were handed out, but some numbers might be missing, and some might be negative or very large (like -5 or 1000). Your job is to find the **smallest positive number** (like 1, 2, 3, …) that wasn’t given to any guest. For example, if the wristbands are [3, 4, -1, 1], the smallest missing positive number is 2 because it’s not in the list.

#### Explain the Parts
- **Inputs**: You get an array of integers (e.g., `nums = [1, 2, 0]` or `nums = [3, 4, -1, 1]`). The array can contain positive numbers, negative numbers, zeros, or duplicates, and it might not be sorted.
- **Outputs**: You need to return a single integer—the **smallest positive integer** (1, 2, 3, …) that is *not* present in the array.
- **Goal**: Find the first positive number that is missing from the array. For example:
  - If `nums = [1, 2, 0]`, the output is 3 (because 1 and 2 are in the array, but 3 is not).
  - If `nums = [3, 4, -1, 1]`, the output is 2 (because 1, 3, and 4 are present, but 2 is missing).

#### Show with a Drawing
Let’s visualize the array `nums = [3, 4, -1, 1]` as a number line to see which positive numbers are present:

```
Number line:  1  2  3  4  5  ...
Present?    [✔]    [✔][✔]    ...
```

- The array has 1, 3, and 4.
- The number 2 is missing, and it’s the smallest positive number not in the array.
- So, the answer is 2.

#### List the Tricky Cases
We need to be careful about these edge cases:
1. **Empty array**: If `nums = []`, the smallest positive number is 1 (since no numbers are present).
2. **All negative numbers or zeros**: If `nums = [-1, -2, 0]`, the answer is 1 (no positive numbers exist).
3. **No gaps in sequence**: If `nums = [1, 2, 3, 4]`, the answer is 5 (the next number after 4).
4. **Duplicates**: If `nums = [1, 1, 1]`, the answer is 2 (1 is present, but 2 is missing).
5. **Large numbers**: If `nums = [1, 2, 1000]`, we ignore 1000 because we only care about small positive numbers, so the answer is 3.
6. **Unsorted array**: The array might be in any order, like `[3, 1, 4, -1]`, but the answer is still 2.

#### Key Insight
Since we’re looking for the *smallest* positive integer, we only care about numbers like 1, 2, 3, … up to the size of the array (`n`) or slightly more. Any number larger than `n + 1` or negative numbers can’t be the answer because if all numbers from 1 to `n` are present, the answer is `n + 1`.

--- 

### Step 2: The First, Simple Solution (The Brute-Force Way)

#### Explain the Simple Idea
The simplest way to find the smallest missing positive integer is to:
1. Ignore all negative numbers, zeros, and numbers larger than the array size (`n`), because the smallest missing positive number must be between 1 and `n + 1` (if all numbers from 1 to `n` are present, the answer is `n + 1`).
2. Check every positive integer starting from 1 (i.e., 1, 2, 3, …) to see if it exists in the array.
3. The first number we check that is *not* in the array is our answer.

To make checking faster, we can use a **set** to store all positive numbers from the array. Then, we can quickly check if each number (1, 2, 3, …) is in the set.

#### Write the C++ Code
Here’s the brute-force solution using a set:

```cpp
#include <vector>
#include <unordered_set>
using namespace std;

int firstMissingPositive(vector<int>& nums) {
    // WHY: Create a set to store all positive numbers from the array for fast lookup.
    unordered_set<int> seen;
    
    // WHY: Go through the array and add only positive numbers to the set.
    for (int num : nums) {
        if (num > 0) {
            seen.insert(num); // WHY: Store positive numbers to check later.
        }
    }
    
    // WHY: Check numbers 1, 2, 3, ... until we find one not in the set.
    int i = 1;
    while (true) {
        if (seen.find(i) == seen.end()) {
            // WHY: If i is not in the set, it's the smallest missing positive number.
            return i;
        }
        i++; // WHY: Move to the next number to check.
    }
}
```

#### Explain Every Line
- **`unordered_set<int> seen;`**: We create a set to store all positive numbers from the array. A set allows us to check if a number exists in constant time (very fast).
- **`for (int num : nums)`**: We loop through each number in the array.
- **`if (num > 0)`**: We only care about positive numbers because negative numbers and zero can’t be the answer (the answer is a positive integer).
- **`seen.insert(num);`**: If the number is positive, we add it to the set. The set automatically handles duplicates (e.g., if `[1, 1, 1]` is in the array, it only stores 1 once).
- **`int i = 1; while (true)`**: We start checking from 1 (the smallest possible answer) and keep going (2, 3, 4, …) until we find a missing number.
- **`if (seen.find(i) == seen.end())`**: If `i` is not in the set, it’s the first missing positive number, so we return it.
- **`i++;`**: If `i` is in the set, we check the next number.

#### Show a Detailed "Dry Run"
Let’s walk through the example `nums = [3, 4, -1, 1]` step-by-step to see how the code works.

1. **Initialize the set**:
   - `seen = {}` (empty set).

2. **Loop through the array**:
   - `num = 3`: Positive, so add to set. Now `seen = {3}`.
   - `num = 4`: Positive, so add to set. Now `seen = {3, 4}`.
   - `num = -1`: Negative, skip it. `seen = {3, 4}`.
   - `num = 1`: Positive, so add to set. Now `seen = {1, 3, 4}`.

3. **Check for the first missing positive**:
   - Check `i = 1`: Is 1 in `seen`? Yes (1 is in `{1, 3, 4}`), so continue.
   - Check `i = 2`: Is 2 in `seen`? No (2 is not in `{1, 3, 4}`), so return 2.

**Result**: The smallest missing positive number is 2.

Let’s try an edge case: `nums = [1, 2, 3, 4]` (no gaps).
- **Loop through array**:
  - `num = 1`: Add to set. `seen = {1}`.
  - `num = 2`: Add to set. `seen = {1, 2}`.
  - `num = 3`: Add to set. `seen = {1, 2, 3}`.
  - `num = 4`: Add to set. `seen = {1, 2, 3, 4}`.
- **Check numbers**:
  - `i = 1`: In set, continue.
  - `i = 2`: In set, continue.
  - `i = 3`: In set, continue.
  - `i = 4`: In set, continue.
  - `i = 5`: Not in set, return 5.

**Result**: The answer is 5.

#### Analyze Speed and Memory (The Easy Way)
- **Time (Speed)**:
  - **Step 1: Building the set**: We loop through the array once to add positive numbers to the set. This takes `O(n)` time, where `n` is the array size.
  - **Step 2: Checking numbers**: In the worst case, we check numbers 1, 2, 3, … up to `n + 1`. If the array has all numbers from 1 to `n` (e.g., `[1, 2, 3, 4]`), we check up to `n + 1`. Each check in the set is `O(1)` (constant time), but we might do up to `n + 1` checks. So, this step is `O(n)`.
  - **Total time**: `O(n)` (building the set) + `O(n)` (checking numbers) = `O(n)`. However, in practice, the checking step depends on the answer, so we’ll call it `O(n)` for simplicity, but it’s not ideal because we’re doing extra work.

- **Space (Memory)**:
  - We create a set to store positive numbers. In the worst case, the array could have up to `n` positive numbers (e.g., `[1, 2, 3, ..., n]`). So, the set uses `O(n)` space.
  - We also use a few variables (like `i`), which are `O(1)` (constant space).
  - **Total space**: `O(n)` because of the set.

This solution is simple but uses extra memory for the set. The good news is that it’s fairly fast (`O(n)` time), but we can do better by avoiding extra space, which we’ll explore in the next step.

---


### Step 3: Thinking Towards a Better Way (The Expert's Thought Process)

#### Find the Slow Part
Let’s look at the brute-force solution and identify what’s not ideal:
- **Extra Memory**: The biggest issue is that we’re using an `unordered_set` to store all positive numbers, which takes `O(n)` extra space. For an array of size `n`, we’re creating a set that could store up to `n` numbers. This is wasteful because we’re using extra memory when the array itself could hold the information we need.
- **Checking Numbers**: While the time complexity is `O(n)` (looping through the array once to build the set and checking up to `n + 1` numbers), we’re doing extra work by checking numbers one by one in the `while` loop. An expert would ask, “Can we find the missing number by working *within* the array itself, without needing a separate set or extra checks?”

The slow part isn’t the time (since `O(n)` is decent), but the **space usage** (`O(n)`) is a problem. We want a solution that uses **constant space** (`O(1)`), meaning we only use a few extra variables, not a whole set.

#### Reveal the "Aha!" Idea
An expert would think: “The array has `n` numbers, so the smallest missing positive integer must be between 1 and `n + 1`. For example, if the array has 4 numbers (`n = 4`), the answer is either 1, 2, 3, 4, or 5. Can we use the array itself to mark which numbers from 1 to `n` are present, without using extra space?”

Here’s the key insight:
- We can use the **array indices** to track which positive numbers are present. For example, if the number 1 is in the array, we can mark index 0 (since index 0 corresponds to 1). If the number 2 is present, we mark index 1, and so on.
- How do we “mark” an index? We can modify the array in-place by changing the sign of the number at that index to negative (e.g., if we see 1, make the number at index 0 negative). This way, a negative number at index `i` means the number `i + 1` is present.
- After marking, we scan the array again. The first index `i` where the number is positive (or not marked) means `i + 1` is the missing number.

This approach is clever because:
- It uses the array itself as a “hash table” to mark presence, so we don’t need extra space.
- We only care about numbers from 1 to `n`, so we can ignore negative numbers, zeros, and numbers larger than `n`.

#### Draw the New Idea
Let’s use ASCII art to visualize how we’ll modify the array `nums = [3, 4, -1, 1]` to mark numbers:

**Step 1: Ignore irrelevant numbers**
- We only care about numbers from 1 to `n` (here, `n = 4`, so 1 to 4). Replace negative numbers, zeros, and numbers > `n` with `n + 1` (which is 5 here, since it can’t be the answer unless all 1 to 4 are present).
- Array: `[3, 4, -1, 1]` → Replace `-1` with `5`: `[3, 4, 5, 1]`.

```
Index:  0  1  2  3
Value: [3][4][5][1]
```

**Step 2: Mark presence**
- For each number, if it’s between 1 and `n`, mark its corresponding index by making the number at that index negative.
- Number 3: Mark index 2 (3-1) by making `nums[2]` negative: `[3, 4, -5, 1]`.
- Number 4: Mark index 3 (4-1) by making `nums[3]` negative: `[3, 4, -5, -1]`.
- Number 5: Ignore (it’s > `n`).
- Number 1: Mark index 0 (1-1) by making `nums[0]` negative: `[-3, 4, -5, -1]`.

```
Index:  0  1  2  3
Value: [-3][4][-5][-1]
Marks:   1     3   4  (negative means number is present)
```

**Step 3: Find the first positive index**
- Scan the array: `[-3, 4, -5, -1]`.
- Index 0 is negative (means 1 is present).
- Index 1 is positive (means 2 is missing).
- Stop here. The answer is `1 + 1 = 2`.

This approach uses the array itself to “remember” which numbers are present, saving space!

---

### Step 4: The Smart Solution (The Optimized Code)

#### Write the Optimized C++ Code
Here’s the optimized solution that uses the array itself to mark numbers and achieves `O(1)` extra space:

```cpp
#include <vector>
using namespace std;

int firstMissingPositive(vector<int>& nums) {
    int n = nums.size(); // WHY: Get the size of the array.
    
    // Step 1: Replace negative numbers, zeros, and numbers > n with n + 1
    for (int i = 0; i < n; i++) {
        // WHY: We only care about numbers 1 to n. Replace others with n + 1.
        if (nums[i] <= 0 || nums[i] > n) {
            nums[i] = n + 1;
        }
    }
    
    // Step 2: Mark presence of each number by making the corresponding index negative
    for (int i = 0; i < n; i++) {
        // WHY: Use absolute value since the number might already be negative from prior marking.
        int num = abs(nums[i]);
        // WHY: Only mark if num is between 1 and n.
        if (num <= n) {
            // WHY: Mark index (num - 1) as negative to indicate num is present.
            nums[num - 1] = -abs(nums[num - 1]);
        }
    }
    
    // Step 3: Find the first positive number's index
    for (int i = 0; i < n; i++) {
        // WHY: If nums[i] is positive, then i + 1 is not marked, so it's missing.
        if (nums[i] > 0) {
            return i + 1;
        }
    }
    
    // WHY: If all numbers 1 to n are present, the answer is n + 1.
    return n + 1;
}
```

#### Explain the "Why"
- **`int n = nums.size();`**: We store the array size `n` for easy reference. The answer will be between 1 and `n + 1`.
- **Step 1: Replace irrelevant numbers**:
  - **`for (int i = 0; i < n; i++)`**: Loop through the array.
  - **`if (nums[i] <= 0 || nums[i] > n)`**: We only care about numbers from 1 to `n`. Negative numbers, zeros, and numbers larger than `n` can’t be the answer, so we replace them with `n + 1` (a number we know can’t be the answer unless all 1 to `n` are present).
  - **`nums[i] = n + 1;`**: Set these numbers to `n + 1` to ignore them later.
- **Step 2: Mark presence**:
  - **`for (int i = 0; i < n; i++)`**: Loop through the array again.
  - **`int num = abs(nums[i]);`**: Use the absolute value because `nums[i]` might already be negative from prior marking, but we need the original number.
  - **`if (num <= n)`**: Only process numbers from 1 to `n` (ignore `n + 1`).
  - **`nums[num - 1] = -abs(nums[num - 1]);`**: Mark the index `num - 1` as negative to indicate that `num` is present. We use `abs` again to ensure we don’t flip a negative number back to positive.
- **Step 3: Find the first missing number**:
  - **`for (int i = 0; i < n; i++)`**: Loop through the array one last time.
  - **`if (nums[i] > 0)`**: If the number at index `i` is positive, it wasn’t marked, meaning `i + 1` is missing, so return `i + 1`.
- **`return n + 1;`**: If we exit the loop, all numbers from 1 to `n` are present, so the answer is `n + 1`.

#### Give a Simple Analogy
This solution uses the **In-Place Hashing** technique. It’s like using a notebook (the array) where each page (index) represents a number (index + 1). When you see a number, you put a checkmark (negative sign) on its page. After checking all numbers, you look for the first page without a checkmark—that’s the missing number. The notebook itself holds all the information, so you don’t need an extra list.

#### Show the "Dry Run"
Let’s walk through `nums = [3, 4, -1, 1]` (n = 4) step-by-step:

1. **Step 1: Replace irrelevant numbers**:
   - Array: `[3, 4, -1, 1]`.
   - Check each number:
     - `nums[0] = 3`: Keep (it’s between 1 and 4).
     - `nums[1] = 4`: Keep.
     - `nums[2] = -1`: Replace with `n + 1 = 5` (since -1 is negative).
     - `nums[3] = 1`: Keep.
   - Array becomes: `[3, 4, 5, 1]`.

2. **Step 2: Mark presence**:
   - Loop through the array: `[3, 4, 5, 1]`.
   - `i = 0, num = abs(nums[0]) = 3`: Since 3 ≤ 4, mark index `3-1 = 2` by making `nums[2]` negative: `nums[2] = -abs(5) = -5`.
     - Array: `[3, 4, -5, 1]`.
   - `i = 1, num = abs(nums[1]) = 4`: Since 4 ≤ 4, mark index `4-1 = 3`: `nums[3] = -abs(1) = -1`.
     - Array: `[3, 4, -5, -1]`.
   - `i = 2, num = abs(nums[2]) = 5`: Since 5 > 4, skip.
   - `i = 3, num = abs(nums[3]) = 1`: Since 1 ≤ 4, mark index `1-1 = 0`: `nums[0] = -abs(3) = -3`.
     - Array: `[-3, 4, -5, -1]`.

3. **Step 3: Find the first positive**:
   - Loop through: `[-3, 4, -5, -1]`.
   - `i = 0`: `nums[0] = -3` (negative, 1 is present).
   - `i = 1`: `nums[1] = 4` (positive, 2 is missing). Return `1 + 1 = 2`.

**Result**: The answer is 2.

**Edge Case: `nums = [1, 2, 3, 4]`**:
- Step 1: All numbers are between 1 and 4, so no changes: `[1, 2, 3, 4]`.
- Step 2: Mark each number:
  - `num = 1`: Mark index 0: `[-1, 2, 3, 4]`.
  - `num = 2`: Mark index 1: `[-1, -2, 3, 4]`.
  - `num = 3`: Mark index 2: `[-1, -2, -3, 4]`.
  - `num = 4`: Mark index 3: `[-1, -2, -3, -4]`.
- Step 3: All numbers are negative, so return `n + 1 = 4 + 1 = 5`.

**Result**: The answer is 5.

#### Compare Speed and Memory
- **Time Complexity**:
  - **Brute-Force**: `O(n)` (build set) + `O(n)` (check numbers) = `O(n)`. However, the checking step depends on the answer, which could be up to `n + 1`.
  - **Optimized**: Three passes through the array:
    - Step 1: `O(n)` to replace irrelevant numbers.
    - Step 2: `O(n)` to mark indices.
    - Step 3: `O(n)` to find the first positive.
    - Total: `O(n)` (3 passes, but still linear). It’s comparable to brute-force but more predictable since it’s always exactly three passes.
- **Space Complexity**:
  - **Brute-Force**: `O(n)` for the set to store positive numbers.
  - **Optimized**: `O(1)` because we only use a few variables (`n`, `i`, `num`) and modify the array in-place. This is a huge improvement!

The optimized solution is better because it uses **constant space** instead of `O(n)` space, while maintaining `O(n)` time.

---


### Step 5: How to Explain Your Solution (The Interview Plan)

Here’s a clear, numbered list to follow when explaining the optimized solution to someone else, like an interviewer. This template keeps your explanation concise, logical, and professional.

1. **The Goal**:
   - "The problem asks us to find the smallest positive integer (1, 2, 3, …) that is missing from an unsorted array of integers, which may contain negatives, zeros, duplicates, or large numbers. For example, if the array is [3, 4, -1, 1], the answer is 2 because it’s the smallest positive number not present."

2. **My Solution Idea**:
   - "My approach is to use the array itself as a hash table to mark the presence of numbers from 1 to `n`, where `n` is the array size. We do this in-place to avoid extra space, achieving `O(1)` space complexity."

3. **How it Works**:
   - "The solution has three steps:
     1. First, I go through the array and replace any number that is negative, zero, or greater than `n` with `n + 1`, since we only care about numbers from 1 to `n`.
     2. Next, I loop through the array again. For each number `num` (if it’s between 1 and `n`), I mark its corresponding index `num - 1` by making the number at that index negative. This shows that `num` is present.
     3. Finally, I scan the array. The first index `i` where the number is positive means `i + 1` was not marked, so `i + 1` is the missing number. If all indices are negative, the answer is `n + 1`."

4. **Performance**:
   - "This solution is fast because its time complexity is `O(n)`—we make three passes through the array: one to clean it, one to mark indices, and one to find the answer. The space complexity is `O(1)` because we only modify the array in-place and use a few variables, with no extra data structures."

5. **Tricky Cases**:
   - "I made sure it works for edge cases like:
     - Empty array: Returns 1.
     - All negative numbers or zeros (e.g., [-1, -2, 0]): Returns 1.
     - No gaps (e.g., [1, 2, 3, 4]): Returns 5.
     - Duplicates (e.g., [1, 1, 1]): Returns 2.
     - Large numbers (e.g., [1, 2, 1000]): Ignores 1000 and returns 3.
     By replacing irrelevant numbers and using absolute values when marking, we handle all these cases correctly."

#### Tips for Delivery
- **Be Clear and Concise**: Start with the goal, then explain the idea briefly before diving into details.
- **Use Examples**: If the interviewer asks, walk through a small example (like `[3, 4, -1, 1]`) to show how the array changes.
- **Mention Trade-offs**: Note that we sacrifice the original array (by modifying it) to save space, but this is okay since the problem doesn’t require preserving the input.

---

### Step 6: Key Lessons and Future Problems

#### The Main Pattern
The key pattern here was **In-Place Hashing**. We used the array itself as a hash table to mark the presence of numbers from 1 to `n` by modifying the values at specific indices (making them negative). This allowed us to track which numbers are present without using extra space, achieving `O(1)` space complexity while maintaining `O(n)` time complexity.

#### Clues for Next Time
You can use the **In-Place Hashing** pattern when you see problems with these characteristics:
- The problem involves an unsorted array of integers, and you need to find or check for numbers in a specific range (e.g., 1 to `n`).
- The problem emphasizes minimizing space usage (e.g., requiring `O(1)` extra space).
- The array can be modified (the problem doesn’t require preserving the original array).
- Keywords like “find the smallest/largest,” “missing number,” “duplicate number,” or “numbers from 1 to n” are clues.
- Examples of problems where this applies:
  - Finding duplicates in an array where numbers are from 1 to `n`.
  - Checking if all numbers from 1 to `n` are present.
  - Problems involving marking or tracking presence without extra data structures.

#### One More Problem
To practice this pattern, try **LeetCode #442: Find All Duplicates in an Array**. In this problem, you’re given an array of integers where each integer is in the range `[1, n]` and some numbers appear twice. You need to find all numbers that appear twice, using `O(1)` extra space. This problem uses a similar in-place marking technique (e.g., making numbers negative to mark indices) to solve it efficiently.
