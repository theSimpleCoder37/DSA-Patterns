### Step 1: Understanding the Problem (Breaking It Down)

Before we can solve a problem, we must know *exactly* what it's asking. Let's break it down into simple pieces.

#### Tell a Simple Story

Imagine you are tracking your daily sales for a small shop. You have a list of numbers representing your profit (or loss, if negative) for each day.

`sales = [+$3, +$4, +$7, -$2, +$1]`

Your boss gives you a target, say **$7**, and asks: "How many different continuous periods of days can you find where the total sales for that period exactly matched our target of $7?"

Let's see:

  * Day 1 to Day 2: `$3 + $4 = $7`. That's one period\!
  * Day 3 alone: `$7`. That's a second period\!
  * Day 2 to Day 5: `$4 + $7 + (-$2) + $1 = $10`. Nope.
  * Day 4 to Day 5: `-$2 + $1 = -$1`. Nope.

So, the answer for this example would be 2.

Our problem is the exact same idea. We get a list of numbers and a target value `k`, and we need to count all the *continuous* parts of the list that add up to `k`.

#### Explain the Parts

  * **Inputs (What we get):**

    1.  `nums`: A list of integers. These can be positive, negative, or zero.
    2.  `k`: An integer. This is our target sum.

  * **Output (What we must return):**

      * A single integer, which is the *total count* of subarrays that sum up to `k`.

  * **Goal (The main task):**

      * Find every single **contiguous subarray** whose elements sum up to `k` and count them. A "contiguous subarray" just means a chunk of the array where all the elements are next to each other. You can't skip elements.

#### Show with a Drawing

Let's take an example: `nums = [1, 2, 3, 1, 5]` and `k = 6`.

A subarray is a continuous part of this array.

```
nums: [ 1, 2, 3, 1, 5 ]
        |_____|
         sum = 1+2 = 3 (Not equal to k)

nums: [ 1, 2, 3, 1, 5 ]
        |_______|
         sum = 1+2+3 = 6 (Yes! This is one subarray. count = 1)

nums: [ 1, 2, 3, 1, 5 ]
              |_|
              sum = 3 (Not equal to k)

nums: [ 1, 2, 3, 1, 5 ]
                 |___|
                  sum = 1+5 = 6 (Yes! This is another one. count = 2)
```

So for `nums = [1, 2, 3, 1, 5]` and `k = 6`, the final answer is `2`.

#### List the Tricky Cases (Edge Cases)

We need to be careful about these situations:

  * **Empty `nums` array:** If the input list is empty, there are no subarrays. The answer should be 0.
  * **Numbers can be negative:** This means a subarray can get longer, but its sum might go down. `[5, -2, 3]`. The sum is 6.
  * **Numbers can be zero:** Zeros don't change the sum, but they create new subarrays. If `k = 3`, and `nums = [3, 0]`:
      * `[3]` is one answer.
      * `[3, 0]` is another answer.
  * **`k` itself is zero:** We might need to find subarrays that sum to 0, like `[2, -2]`.

-----

### Step 2: The First, Simple Solution (The Brute-Force Way)

When you're starting a new problem, it's often best to think of the most direct, simple solution first, even if it's not the fastest. We call this the "brute-force" approach.

#### Explain the Simple Idea

The simplest idea is to check every single possible contiguous subarray.

How can we do that?

1.  We can pick a starting point for our subarray. Let's call the start index `i`.
2.  Then, we can pick an ending point. Let's call the end index `j`. The end point must be after or at the same position as the start point.
3.  Once we have a start `i` and an end `j`, we add up all the numbers in `nums` from index `i` to `j`.
4.  We check if this sum equals our target `k`.
5.  If it does, we add one to our counter.
6.  We repeat this process for all possible start and end points.

This guarantees we won't miss any subarrays.

#### Write the C++ Code

Here is the C++ code that does exactly what we just described.

```cpp
#include <iostream>
#include <vector>

class Solution {
public:
    int subarraySum(std::vector<int>& nums, int k) {
        // WHY: This variable will store our final answer, the total count of valid subarrays.
        int count = 0;
        int n = nums.size();

        // WHY: This outer loop picks the starting index 'i' of our subarray.
        // It will go from the first element to the last element.
        for (int i = 0; i < n; ++i) {
            
            // WHY: This inner loop picks the ending index 'j' of our subarray.
            // It starts from our chosen starting index 'i' and goes to the end of the array.
            for (int j = i; j < n; ++j) {
                
                // WHY: This variable will hold the sum of the current subarray from index 'i' to 'j'.
                int currentSum = 0;

                // WHY: This third loop calculates the sum of the elements from the start (i) to the end (j).
                for (int l = i; l <= j; ++l) {
                    currentSum += nums[l];
                }
                
                // WHY: We check if the sum we just calculated is equal to our target 'k'.
                if (currentSum == k) {
                    // WHY: If it is, we've found a valid subarray, so we increment our counter.
                    count++;
                }
            }
        }
        
        // WHY: After checking all possible subarrays, we return the total count.
        return count;
    }
};
```

*Note: We can actually make this a bit cleaner by removing the third loop. We can calculate the `currentSum` as we go in the second loop.* Here is a slightly better, but still brute-force, version:

```cpp
#include <vector>

class Solution {
public:
    int subarraySum(std::vector<int>& nums, int k) {
        int count = 0; // WHY: This will store our final answer.
        int n = nums.size();

        // WHY: This loop selects a starting point 'i' for the subarray.
        for (int i = 0; i < n; ++i) {
            int currentSum = 0; // WHY: Reset the sum for each new starting point.

            // WHY: This loop extends the subarray from the starting point 'i' to the end of the array.
            for (int j = i; j < n; ++j) {
                // WHY: Add the current element to the sum of the subarray starting at 'i'.
                currentSum += nums[j];

                // WHY: Check if the sum of the subarray from 'i' to 'j' equals our target.
                if (currentSum == k) {
                    count++; // WHY: If it does, we increment our counter.
                }
            }
        }
        return count; // WHY: Return the total count found.
    }
};
```

This second version is what we'll analyze. It's still brute-force but more efficient.

#### Show a Detailed "Dry Run"

Let's trace our improved code with the example `nums = [1, 1, 1]` and `k = 2`.

  * **Start:** `count` is `0`. The code starts.

  * **Outer loop `i` begins.** `i` is `0`.

      * `currentSum` is reset to `0`.
      * **Inner loop `j` begins.** `j` is `0`.
          * `currentSum` becomes `0 + nums[0]` which is `0 + 1 = 1`.
          * Is `currentSum == k`? Is `1 == 2`? No.
      * `j` becomes `1`.
          * `currentSum` becomes `1 + nums[1]` which is `1 + 1 = 2`.
          * Is `currentSum == k`? Is `2 == 2`? Yes\!
          * We increment `count`. `count` is now `1`.
      * `j` becomes `2`.
          * `currentSum` becomes `2 + nums[2]` which is `2 + 1 = 3`.
          * Is `currentSum == k`? Is `3 == 2`? No.
      * Inner loop for `i=0` finishes.

  * **Outer loop `i` continues.** `i` is now `1`.

      * `currentSum` is reset to `0`.
      * **Inner loop `j` begins.** `j` is `1`.
          * `currentSum` becomes `0 + nums[1]` which is `0 + 1 = 1`.
          * Is `1 == 2`? No.
      * `j` becomes `2`.
          * `currentSum` becomes `1 + nums[2]` which is `1 + 1 = 2`.
          * Is `2 == 2`? Yes\!
          * We increment `count`. `count` is now `2`.
      * Inner loop for `i=1` finishes.

  * **Outer loop `i` continues.** `i` is now `2`.

      * `currentSum` is reset to `0`.
      * **Inner loop `j` begins.** `j` is `2`.
          * `currentSum` becomes `0 + nums[2]` which is `0 + 1 = 1`.
          * Is `1 == 2`? No.
      * Inner loop for `i=2` finishes.

  * **Outer loop finishes.** The function returns the final `count`, which is `2`.

#### Analyze Speed and Memory (The Easy Way)

  * **Time (Speed):** We have a loop inside another loop.

      * The outer loop runs `N` times (where `N` is the number of items in `nums`).
      * The inner loop also runs many times. When `i` is 0, it runs `N` times. When `i` is 1, it runs `N-1` times, and so on.
      * This is roughly $N \\times N$ operations. We write this as $O(N^2)$. If `nums` has 10,000 items, this is $10,000 \\times 10,000 = 100,000,000$ operations, which is very slow.

  * **Space (Memory):** How much extra memory did we use?

      * We created a few variables like `count`, `n`, `i`, `j`, and `currentSum`. These are just single numbers. They don't depend on the size of the input list `nums`.
      * Because the extra memory is fixed and small, we call this **constant space**, which we write as $O(1)$.

-----

### Step 3: Thinking Towards a Better Way (The Expert's Thought Process)

The brute-force solution works, but it's slow. An expert's first question is always: "Where is the wasted work?"

#### Find the Slow Part

In our brute-force code, the slowness comes from the nested loops.

```cpp
for (int i = 0; i < n; ++i) {
    int currentSum = 0; 
    for (int j = i; j < n; ++j) { // This is the slow part
        currentSum += nums[j];    // We are adding numbers again and again
        if (currentSum == k) {
            count++;
        }
    }
}
```

Think about our `[1, 1, 1]` example.

  * When `i=0`, we calculate `1`, then `1+1`, then `1+1+1`.
  * When `i=1`, we calculate `1`, then `1+1`.

We calculated `1+1` twice\! For a large array, we are re-calculating the sums of the same chunks of the array over and over. This is the wasted work we need to eliminate.

#### Reveal the "Aha\!" Idea

An expert would ask: *"How can we calculate sums just once and reuse that information?"*

Let's think about sums from the beginning of the array. We can call this a "prefix sum" or a running total.

Let's take `nums = [3, 4, 7, 2]`

  * Prefix sum at index 0 is `3`.
  * Prefix sum at index 1 is `3 + 4 = 7`.
  * Prefix sum at index 2 is `3 + 4 + 7 = 14`.
  * Prefix sum at index 3 is `3 + 4 + 7 + 2 = 16`.

Now, how can we get the sum of a subarray, say from index 1 to 2 (`[4, 7]`)?
Sum of `[4, 7]` is `11`.

We can get this using our prefix sums\!
It's `(prefix sum at index 2) - (prefix sum at index 0)`
`14 - 3 = 11`. It works\!

So, the sum of any subarray from `start` to `end` is just `prefixSum[end] - prefixSum[start - 1]`.

Our goal is to find subarrays where the sum is `k`.
So we are looking for: `prefixSum[end] - prefixSum[start - 1] = k`.

Let's rearrange this equation. This is the **"Aha\!" moment**.
`prefixSum[end] - k = prefixSum[start - 1]`

This gives us a brilliant new plan:

1.  Let's walk through the array from left to right, calculating the prefix sum as we go. Let's call it `currentSum`.
2.  At each element, we have our `currentSum`. We need to find if there's a previous prefix sum that equals `currentSum - k`.
3.  If we find one, it means the elements between that previous point and our current position add up to `k`\!

But how do we quickly check for and count all the previous prefix sums we've seen? We need a tool that can store and retrieve numbers instantly. A **hash map** (or dictionary) is perfect for this\!

We can use a hash map where:

  * **Key:** A prefix sum value we have seen before.
  * **Value:** How many times we have seen that prefix sum.

#### Draw the New Idea

Let's try this new method on `nums = [3, 4, -1, 1]` and `k = 6`.

**Our tools:**

  * `count = 0` (our final answer)
  * `currentSum = 0` (our running prefix sum)
  * `map = {}` (our hash map to store previous sums)

**Special First Step:** We must put `0` into our map to start. `map = {0: 1}`.
*Why?* This handles cases where a subarray *starting from the very beginning* of the array adds up to `k`. If `currentSum` itself equals `k`, then `currentSum - k = 0`. We need to find a `0` in our map to make this work.

**Let's start the loop:**

1.  **`num = 3`** (index 0)

      * `currentSum = 0 + 3 = 3`.
      * We need to find `currentSum - k`, which is `3 - 6 = -3`.
      * Is `-3` in our map? No.
      * Add `currentSum` (3) to our map. `map = {0: 1, 3: 1}`.

    <!-- end list -->

    ```
    nums: [ 3,  4, -1,  1 ]
            ^
    currentSum = 3.  map = {0:1, 3:1}
    ```

2.  **`num = 4`** (index 1)

      * `currentSum = 3 + 4 = 7`.
      * We need to find `currentSum - k`, which is `7 - 6 = 1`.
      * Is `1` in our map? No.
      * Add `currentSum` (7) to our map. `map = {0: 1, 3: 1, 7: 1}`.

    <!-- end list -->

    ```
    nums: [ 3,  4, -1,  1 ]
                ^
    currentSum = 7.  map = {0:1, 3:1, 7:1}
    ```

3.  **`num = -1`** (index 2)

      * `currentSum = 7 + (-1) = 6`.
      * We need to find `currentSum - k`, which is `6 - 6 = 0`.
      * Is `0` in our map? YES\! Its value is `1`.
      * So, we add `1` to our `count`. `count` is now `1`.
      * This `1` represents the subarray `[3, 4, -1]`.
      * Add `currentSum` (6) to our map. `map = {0: 1, 3: 1, 7: 1, 6: 1}`.

    <!-- end list -->

    ```
    nums: [ 3,  4, -1,  1 ]
                   ^
    currentSum = 6.  count = 1. map = {0:1, 3:1, 7:1, 6:1}
    ```

4.  **`num = 1`** (index 3)

      * `currentSum = 6 + 1 = 7`.
      * We need to find `currentSum - k`, which is `7 - 6 = 1`.
      * Is `1` in our map? NO. (We have a 7, but not a 1).
      * We've seen the `currentSum` of `7` before. So we increment its count in the map.
      * `map = {0: 1, 3: 1, 7: 2, 6: 1}`.

    <!-- end list -->

    ```
    nums: [ 3,  4, -1,  1 ]
                       ^
    currentSum = 7.  map = {0:1, 3:1, 7:2, 6:1}
    ```

Loop ends. The final answer is `count = 1`.

This new method only requires us to go through the list once\!

-----

### Step 4: The Smart Solution (The Optimized Code)

We will now write the C++ code for our new approach using the prefix sum and hash map.

#### Write the Optimized C++ Code

This code implements the logic we just drew out. It will solve the problem in a single pass through the `nums` array.

```cpp
#include <vector>
#include <unordered_map>

class Solution {
public:
    int subarraySum(std::vector<int>& nums, int k) {
        // WHY: Use an unordered_map (a hash map) to store the frequency of each prefix sum we encounter.
        // Key: The prefix sum. Value: The number of times this prefix sum has occurred.
        std::unordered_map<int, int> prefixSumCount;
        
        // WHY: Initialize the map with a prefix sum of 0 having occurred 1 time.
        // This is a crucial step. It handles the case where a subarray starting from index 0 sums up to k.
        // For example, if nums=[3, 4] and k=7, currentSum becomes 7. We need to find (7-7)=0 in the map.
        prefixSumCount[0] = 1;
        
        int count = 0;        // WHY: Our final result counter.
        int currentSum = 0;   // WHY: The running total (prefix sum) as we iterate through the array.

        // WHY: We only need a single loop through the numbers.
        for (int num : nums) {
            // WHY: Add the current number to our running sum.
            currentSum += num;

            // WHY: This is the core logic. We are looking for a previous prefix sum `P` such that:
            // currentSum - P = k.  Rearranging gives: P = currentSum - k.
            // So, we calculate the `requiredSum` we need to have seen before.
            int requiredSum = currentSum - k;
            
            // WHY: Check if this `requiredSum` exists as a key in our map.
            // The .count() method is a safe way to check for existence in a C++ map.
            if (prefixSumCount.count(requiredSum)) {
                // WHY: If it exists, it means we have found one or more subarrays that end at the current
                // position with a sum of k. The number of such subarrays is exactly the number of times
                // we have seen this `requiredSum` before. So we add that count to our result.
                count += prefixSumCount[requiredSum];
            }
            
            // WHY: Finally, we update the map with the `currentSum` we just calculated.
            // We increment its count, because we have now seen this sum one more time.
            prefixSumCount[currentSum]++;
        }
        
        return count;
    }
};
```

#### Give a Simple Analogy

This pattern is called **Prefix Sum with a Hash Map**.

Think of it like keeping a running ledger of your bank account balance over time.

  * The `nums` array is your list of daily transactions (deposits or withdrawals).
  * `currentSum` is your account balance on any given day.
  * The `hash map` is your history book of all the balances you've ever had and how many days you had that exact balance.
  * To find if you made exactly `$k` over a recent period, you take your `currentBalance`, subtract `$k`, and look in your history book to see if you've *ever* had a balance equal to `currentBalance - $k`. If you have, you know a sequence of transactions summing to `$k` must have occurred.

#### Show the "Dry Run"

Let's trace our smart solution with the same example as before: `nums = [1, 1, 1]` and `k = 2`. Watch how much faster this is.

  * **Start:**

      * `count` = `0`
      * `currentSum` = `0`
      * `prefixSumCount` = `{0: 1}`

  * **Loop 1: `num` is `1`**

      * `currentSum` becomes `0 + 1 = 1`.
      * `requiredSum` = `currentSum - k` = `1 - 2 = -1`.
      * Does the map have key `-1`? No.
      * Update the map with `currentSum`. `prefixSumCount[1]` becomes `1`.
      * Map is now: `{0: 1, 1: 1}`.

  * **Loop 2: `num` is `1`**

      * `currentSum` becomes `1 + 1 = 2`.
      * `requiredSum` = `currentSum - k` = `2 - 2 = 0`.
      * Does the map have key `0`? Yes\! Its value is `1`.
      * `count` = `count + prefixSumCount[0]` = `0 + 1 = 1`.
      * Update the map with `currentSum`. `prefixSumCount[2]` becomes `1`.
      * Map is now: `{0: 1, 1: 1, 2: 1}`.

  * **Loop 3: `num` is `1`**

      * `currentSum` becomes `2 + 1 = 3`.
      * `requiredSum` = `currentSum - k` = `3 - 2 = 1`.
      * Does the map have key `1`? Yes\! Its value is `1`.
      * `count` = `count + prefixSumCount[1]` = `1 + 1 = 2`.
      * Update the map with `currentSum`. `prefixSumCount[3]` becomes `1`.
      * Map is now: `{0: 1, 1: 1, 2: 1, 3: 1}`.

  * **End of Loop:** The loop is finished. We return `count`, which is `2`.
    This gave us the correct answer in just one pass through the array.

#### Compare Speed and Memory

Let's compare the two solutions directly.

| Approach                  | Time (Speed)                               | Space (Memory)                                 |
| ------------------------- | ------------------------------------------ | ---------------------------------------------- |
| **Brute-Force Solution** | $O(N^2)$ - Very slow for large inputs.     | $O(1)$ - Uses almost no extra memory.          |
| **Smart Solution (Hash Map)** | $O(N)$ - Extremely fast, one pass.         | $O(N)$ - Uses memory for the hash map.         |

This is a classic **time-space tradeoff**. We used more memory (the hash map) to dramatically reduce the runtime from quadratic ($N^2$) to linear ($N$). For almost all situations, this is a fantastic improvement.

-----

### Step 5: How to Explain Your Solution (The Interview Plan)

Here is a template you can use to explain your solution for this problem.

#### Provide a Clear Template

**1. The Goal**

* Start by restating the problem in your own words. This shows the interviewer you understand the core task.
* **You can say:** "The goal of this problem is to find the total number of continuous subarrays within a given array of numbers whose elements sum up to a specific target, `k`."

**2. My Solution Idea**

* State your high-level approach clearly. Name the pattern and the main data structure you used.
* **You can say:** "My approach to solve this efficiently is to use a **Hash Map** combined with the concept of **Prefix Sums**. This allows me to find the answer in a single pass through the array, which is much faster than a brute-force approach."

**3. How it Works**

* Walk through your algorithm step-by-step. Explain the "why" behind your logic.
* **You can say:**
    * "First, I initialize a hash map to keep track of the frequencies of the prefix sums I've seen so far. I also initialize a `count` variable to 0 and a `currentSum` variable to 0."
    * "Crucially, I pre-populate the hash map with a prefix sum of `0` having a count of `1`. This is to correctly handle subarrays that start from the very beginning of the array."
    * "Then, I iterate through the array just once. In each step, I add the current number to my `currentSum`."
    * "The key insight is this: if a subarray ending at my current position sums to `k`, then `currentSum - (a previous sum) = k`. This means the previous sum I'm looking for must be equal to `currentSum - k`."
    * "So, I check my hash map to see if I have seen this required `currentSum - k` value before. If I have, I add its frequency from the map to my total `count`."
    * "Finally, regardless of whether I found a match, I update the hash map with the `currentSum` I just calculated, incrementing its frequency count. This way, it's available for future lookups."
    * "After the loop finishes, the `count` variable holds the final answer."

**4. Performance**

* Clearly state the time and space complexity and briefly explain why.
* **You can say:** "This solution has a **time complexity of O(N)**, because I only iterate through the array once. The hash map lookups and insertions take, on average, constant O(1) time. The **space complexity is O(N)**, because in the worst case, the hash map might need to store a unique prefix sum for every element in the array."

**5. Tricky Cases**

* Show that you've thought about edge cases.
* **You can say:** "My solution naturally handles positive and negative numbers, as well as zeros, due to the nature of prefix sums. The important edge case of subarrays starting from index 0 is handled correctly by initializing the map with a sum of 0."

---

### Step 6: Key Lessons and Future Problems

The goal of learning is not just to solve one problem, but to learn a *pattern* that can solve many problems.

#### The Main Pattern

The key pattern we learned today is called **"Prefix Sum with a Hash Map"**.

This is an extremely powerful technique. It's the go-to solution for many problems that ask you to find or count contiguous subarrays with a certain sum property. By trading a bit of memory (for the hash map), we make our solution dramatically faster.

#### Clues for Next Time

How do you know when to use this pattern again? Look for these clues in the problem description:

* **Key Phrases:** "subarray sum", "range sum", "continuous segment", "sum equals k", "sum divisible by k".
* **Problem Type:** The problem involves finding a continuous block of numbers that meets some condition related to its sum.
* **Input Contains Negatives:** This is a huge clue! Simpler patterns like the "sliding window" often don't work correctly when numbers can be negative, because adding a new number could decrease the sum. The prefix sum method works perfectly with positive, negative, and zero values.
* **You Need to Count:** The problem asks for the *total number* of such subarrays. A hash map is perfect for this because it can store frequencies (the count of how many times you've seen a specific sum).

When you see these clues, your first thought should be: "Can I use prefix sums and a hash map here?"

#### One More Problem

The best way to make a lesson stick is to practice it. Here is a very similar problem that uses the exact same core idea. Try solving it on your own:

**[LeetCode 974. Subarray Sums Divisible by K](https://leetcode.com/problems/subarray-sums-divisible-by-k/)**

This problem asks you to find the number of subarrays whose sum is evenly divisible by `k`. You will find that the logic is almost identical to what we just did, with one tiny twist. It's the perfect next step.

---
