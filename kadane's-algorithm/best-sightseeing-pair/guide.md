### **Step 1: Understanding the Problem (Breaking It Down)**

Before we write any code, we must understand exactly what the problem is asking us to do.

#### **Tell a Simple Story**

Imagine you're planning a vacation. You have a list of sightseeing spots, and each spot has a "beauty score". You want to visit two spots, spot `i` and spot `j`.

The total enjoyment of your trip depends on three things:
1.  The beauty score of the first spot (`values[i]`).
2.  The beauty score of the second spot (`values[j]`).
3.  The travel between them. The farther apart they are, the more tired you get, which *reduces* your enjoyment. We'll say this travel penalty is equal to the distance `j - i`.

So, the total score for a pair of spots `i` and `j` is: `(beauty of i) + (beauty of j) - (distance between them)`.

Our job is to pick the two spots that give the absolute best possible total score. A key rule is that you must visit spot `i` *before* you visit spot `j`.

#### **Explain the Parts**

* **Inputs:** We are given a list of positive numbers called `values`. For example: `[8, 1, 5, 2, 6]`. Each number is the beauty score of a spot. The position in the list is the location of the spot (e.g., spot 0, spot 1, spot 2, etc.).

* **Outputs:** We need to return a single number, which is the highest possible sightseeing score we can find.

* **Goal:** The main task is to find two indices, `i` and `j`, such that `i < j`, which makes the formula `values[i] + values[j] + i - j` as large as possible.

Let's look at the formula again: `values[i] + values[j] + i - j`. This looks a bit strange. The problem uses `i - j`, but since `i` is always smaller than `j`, `i - j` will always be a negative number. This is just like our travel penalty `-(j - i)`. So, the formula is exactly what we described in our story.

#### **Show with a Drawing**

Let's use the example `values = [8, 1, 5, 2, 6]`.

Let's say we pick spot `i = 0` and spot `j = 2`.
* Spot `i` is at index 0, its beauty is `values[0] = 8`.
* Spot `j` is at index 2, its beauty is `values[2] = 5`.

`i = 0`
`j = 2`
`values = [ 8,  1,  5,  2,  6 ]`
           `^       ^`
           `i       j`

The score would be:
`score = values[i] + values[j] + i - j`
`score = 8         + 5         + 0 - 2`
`score = 11`

What if we picked `i = 1` and `j = 4`?
* Spot `i` is at index 1, beauty `values[1] = 1`.
* Spot `j` is at index 4, beauty `values[4] = 6`.

`i = 1`
`j = 4`
`values = [ 8,  1,  5,  2,  6 ]`
               `^           ^`
               `i           j`

The score would be:
`score = values[i] + values[j] + i - j`
`score = 1         + 6         + 1 - 4`
`score = 7 - 3 = 4`

Our goal is to find the pair `(i, j)` that produces the highest score out of all possible pairs.

#### **List the Tricky Cases (Edge Cases)**

* **Array Size:** The problem states the list will have at least 2 spots (`2 <= values.length`). This is good news! It means we don't have to worry about an empty list or a list with only one spot, which would make finding a pair impossible.
* **Value Range:** The beauty scores are always positive (`1 <= values[i] <= 1000`). This is also simple, no zeros or negative scores to worry about.
* **Index Order:** The most important rule is that we must pick `i` before `j` (`i < j`).

---

### **Step 2: The First, Simple Solution (The Brute-Force Way)**

The most natural way to start is with the simplest, most obvious method. It might not be the fastest, but it will work correctly and help us understand the problem better.

#### **Explain the Simple Idea**

The problem asks us to find the best *pair* of spots. The simplest way to do that is to check every single possible pair.

Here's the step-by-step logic:

1.  Pick the first spot, `i`.
2.  Then, pick a second spot, `j`, that comes *after* `i`.
3.  Calculate the score for this pair: `values[i] + values[j] + i - j`.
4.  If this score is better than the best score we've seen so far, we save it.
5.  We repeat this process for every single possible pair of `(i, j)` where `i` comes before `j`.

How do we do this in code? We can use two loops:

  * An "outer" loop will go through the list and pick our `i`.
  * An "inner" loop will go through the list *starting from `i + 1`* and pick our `j`.

This guarantees that `j` is always greater than `i`.

#### **Write the C++ Code**

Here is the C++ code that does exactly what we just described.

```cpp
#include <iostream>
#include <vector>
#include <algorithm> // For std::max

class Solution {
public:
    int maxScoreSightseeingPair(std::vector<int>& values) {
        // WHY: We need a variable to keep track of the highest score found so far.
        // We initialize it to a very small number to make sure the first calculated score
        // will be higher.
        int maxScore = 0; 

        // WHY: We get the number of sightseeing spots to use in our loops.
        int n = values.size();

        // WHY: The outer loop picks our first spot, `i`.
        // It goes from the first spot (index 0) up to the second-to-last spot.
        // It can't go to the last spot, because `j` must be `i + 1`.
        for (int i = 0; i < n - 1; ++i) {

            // WHY: The inner loop picks our second spot, `j`.
            // It MUST start from `i + 1` to satisfy the rule that `i < j`.
            for (int j = i + 1; j < n; ++j) {
                
                // WHY: We calculate the score for the current pair (i, j).
                int currentScore = values[i] + values[j] + i - j;
                
                // WHY: We compare the score of this pair with the best score we've found so far.
                // If the current one is better, we update our maxScore.
                maxScore = std::max(maxScore, currentScore);
            }
        }
        
        // WHY: After checking every single possible pair, maxScore will hold the highest value.
        // We return it as the answer.
        return maxScore;
    }
};
```

#### **Show a Detailed "Dry Run"**

Let's trace our code with the example `values = [8, 1, 5, 2, 6]`.
`n = 5`. `maxScore` starts at `0`.

**Outer loop: `i = 0`** (`values[0] = 8`)

  * **Inner loop: `j = 1`** (`values[1] = 1`)
      * `currentScore` = `values[0] + values[1] + 0 - 1` = `8 + 1 - 1` = `8`.
      * `maxScore` = `std::max(0, 8)` which is `8`.
  * **Inner loop: `j = 2`** (`values[2] = 5`)
      * `currentScore` = `values[0] + values[2] + 0 - 2` = `8 + 5 - 2` = `11`.
      * `maxScore` = `std::max(8, 11)` which is `11`.
  * **Inner loop: `j = 3`** (`values[3] = 2`)
      * `currentScore` = `values[0] + values[3] + 0 - 3` = `8 + 2 - 3` = `7`.
      * `maxScore` remains `11`.
  * **Inner loop: `j = 4`** (`values[4] = 6`)
      * `currentScore` = `values[0] + values[4] + 0 - 4` = `8 + 6 - 4` = `10`.
      * `maxScore` remains `11`.

**Outer loop: `i = 1`** (`values[1] = 1`)

  * **Inner loop: `j = 2`** (`values[2] = 5`)
      * `currentScore` = `values[1] + values[2] + 1 - 2` = `1 + 5 - 1` = `5`.
      * `maxScore` remains `11`.
  * **Inner loop: `j = 3`** (`values[3] = 2`)
      * `currentScore` = `values[1] + values[3] + 1 - 3` = `1 + 2 - 2` = `1`.
      * `maxScore` remains `11`.
  * ...and so on.

The code will continue this process, checking every pair: `(2,3)`, `(2,4)`, `(3,4)`. In the end, it will find that the pair `(i=0, j=2)` gave the highest score, which is `11`.

#### **Analyze Speed and Memory (The Easy Way)**

  * **Time (Speed):** We have a loop inside another loop.

      * The outer loop runs about `N` times (where `N` is the number of spots).
      * The inner loop also runs about `N` times for each step of the outer loop.
      * This means the total number of calculations is roughly $N \\times N$, or $N^2$.
      * If we have 50,000 spots, $50,000 \\times 50,000$ is 2.5 billion operations. This is way too slow and will time out on most platforms. We call this **Time Complexity $O(N^2)$**.

  * **Space (Memory):** How much extra memory did we use? We created a few variables like `maxScore`, `n`, `i`, `j`, and `currentScore`. The amount of memory for these variables doesn't change no matter how big the `values` list is. It's a small, constant amount of memory. We call this **Space Complexity $O(1)$**.

This brute-force solution is correct, but it's too slow for a large number of spots.

-----

### **Step 3: Thinking Towards a Better Way (The Expert's Thought Process)**

#### **Find the Slow Part**

First, let's pinpoint what makes our simple solution slow. It's the **nested loop**.

```cpp
for (int i = 0; i < n - 1; ++i) {
    // This inner loop is the problem!
    for (int j = i + 1; j < n; ++j) { 
        // ...
    }
}
```

For each `i`, we are looping all over again through the rest of the list to find a `j`. This is a huge amount of repeated work. For example, when `i = 0`, we check `j = 1, 2, 3, ...`. When `i = 1`, we check `j = 2, 3, ...` again. We are visiting the same spots `j` over and over.

An expert would ask: "How can I get the answer by visiting each spot only once?"

#### **Reveal the "Aha\!" Idea**

The key to unlocking a faster solution is often hidden in the formula itself.

The score formula is: `score = values[i] + values[j] + i - j`

Let's try to rearrange it by putting the `i` parts together and the `j` parts together.

`score = (values[i] + i) + (values[j] - j)`

This is the **"Aha\!" moment\!** Look at what we have now. The total score is just the sum of two separate pieces:

  * **Part A:** `(values[i] + i)`. This part only depends on the first spot, `i`.
  * **Part B:** `(values[j] - j)`. This part only depends on the second spot, `j`.

Now, let's think about this from the perspective of spot `j`. Imagine we are at a specific spot `j`. We want to calculate the best possible score we can get if `j` is our *second* spot.

The value of `Part B` (`values[j] - j`) is fixed because we are at `j`.
To make the total score `Part A + Part B` as big as possible, we need to find the biggest possible `Part A`.

And what is the biggest `Part A`? It's the maximum value of `(values[i] + i)` for all the spots `i` that came *before* `j` (i.e., for all `i < j`).

This leads to a new, much smarter plan:

1.  We can loop through the list just **once**. Let's use our loop variable `j` to represent the second spot in the pair.
2.  As we travel along the list, we need to keep track of the best `Part A` (`values[i] + i`) we have seen so far.
3.  At each new spot `j`, we calculate the potential total score by combining `Part B` for `j` with the *best `Part A` we've found so far*.
4.  Then, we update our "best `Part A` so far" for future spots to use.

This way, we never have to look backward. We just carry the information about the best previous spot with us.

#### **Draw the New Idea**

Let's visualize this with `values = [8, 1, 5, 2, 6]`.

We will have one pointer, `j`, that moves from left to right.
We also need a variable to remember the best `Part A` found so far. Let's call it `best_i_score`.

**Start:**
We need a starting `i` to begin. So let's say our first spot `i` is at index 0.
`best_i_score = values[0] + 0 = 8 + 0 = 8`.
Our main loop for `j` will start at index 1.

**`j = 1`** (`values[1] = 1`)
`values = [ 8,  1,  5,  2,  6 ]`
`i`  `j`
`best_i_score = 8` (from i=0)

  * Calculate score for this pair:
      * `Part A` comes from `best_i_score` = `8`
      * `Part B` is `values[j] - j` = `1 - 1` = `0`
      * Total score = `8 + 0 = 8`. Our max score so far is 8.
  * Now, update `best_i_score` for the *next* step. We check if the spot we just visited (`j=1`) would make a better *first* spot for the future.
      * Current spot's `Part A` value is `values[1] + 1 = 1 + 1 = 2`.
      * Is `2` better than our current `best_i_score` (`8`)? No. So `best_i_score` remains `8`.

**`j = 2`** (`values[2] = 5`)
`values = [ 8,  1,  5,  2,  6 ]`
`i`      `j`   (best `i` is still at index 0)
`best_i_score = 8`

  * Calculate score for this pair:
      * `Part A` comes from `best_i_score` = `8`
      * `Part B` is `values[j] - j` = `5 - 2` = `3`
      * Total score = `8 + 3 = 11`. Our max score so far is 11.
  * Update `best_i_score` for the future:
      * Current spot's `Part A` value is `values[2] + 2 = 5 + 2 = 7`.
      * Is `7` better than `8`? No. `best_i_score` remains `8`.

**`j = 3`** (`values[3] = 2`)
...and so on.

By doing this, we just march forward through the list once. We never need a second loop\!

-----

### **Step 4: The Smart Solution (The Optimized Code)**

Now we'll write the C++ code for our new, efficient approach.

#### **Write the Optimized C++ Code**

This code implements the single-pass strategy we just designed.

```cpp
#include <iostream>
#include <vector>
#include <algorithm> // For std::max

class Solution {
public:
    int maxScoreSightseeingPair(std::vector<int>& values) {
        // WHY: We get the number of sightseeing spots.
        int n = values.size();

        // WHY: This variable stores the best "Part A" (values[i] + i) we've seen so far.
        // We initialize it with the first spot (i=0), since that's the only
        // possible first spot for our second spot (j=1).
        int max_i_plus_val = values[0] + 0;

        // WHY: This will store our final answer. Initialize to a small number.
        int maxScore = 0;

        // WHY: We loop through the array just ONCE, starting from the second spot (j=1).
        // Each element in this loop is treated as the second spot 'j'.
        for (int j = 1; j < n; ++j) {
            
            // WHY: Calculate the score for the current 'j' paired with the BEST 'i' we've seen so far.
            // current j's value (Part B) = values[j] - j
            // best i's value (Part A) = max_i_plus_val
            int currentScore = max_i_plus_val + (values[j] - j);
            
            // WHY: Update our overall best score if the current one is better.
            maxScore = std::max(maxScore, currentScore);
            
            // WHY: IMPORTANT! After using the current `j`, we see if it can serve as a
            // better *first* spot for the spots that come AFTER it.
            // We update our best "Part A" for future iterations.
            max_i_plus_val = std::max(max_i_plus_val, values[j] + j);
        }
        
        // WHY: After checking all spots as 'j', maxScore holds the highest possible value.
        return maxScore;
    }
};
```

#### **Give a Simple Analogy**

The technique we used here is a form of **Dynamic Programming**.

A simple analogy is the **"Running Champion"** method.
Imagine you're walking along a line of athletes. Your goal is to form the best possible two-person team.

  * As you meet each new athlete (`j`), you pair them with the current "champion" (`max_i_plus_val`) you've seen so far to see what team score you get.
  * After you're done with athlete `j`, you check if they are strong enough to become the new "champion" for the rest of the athletes you haven't met yet.
    You never have to look back at the whole line, you just need to remember who the champion is right now.

#### **Show the "Dry Run"**

Let's trace the optimized code with `values = [8, 1, 5, 2, 6]`.

**Before the loop:**

  * `n = 5`
  * `max_i_plus_val` = `values[0] + 0` = `8`.
  * `maxScore = 0`.

**Loop starts: `j = 1`** (`values[1] = 1`)

  * `currentScore` = `max_i_plus_val + (values[1] - 1)` = `8 + (1 - 1)` = `8`.
  * `maxScore` = `std::max(0, 8)` = `8`.
  * Update `max_i_plus_val`: `std::max(8, values[1] + 1)` = `std::max(8, 2)` = `8`.

**`j = 2`** (`values[2] = 5`)

  * `currentScore` = `max_i_plus_val + (values[2] - 2)` = `8 + (5 - 2)` = `8 + 3` = `11`.
  * `maxScore` = `std::max(8, 11)` = `11`.
  * Update `max_i_plus_val`: `std::max(8, values[2] + 2)` = `std::max(8, 7)` = `8`.

**`j = 3`** (`values[3] = 2`)

  * `currentScore` = `max_i_plus_val + (values[3] - 3)` = `8 + (2 - 3)` = `8 - 1` = `7`.
  * `maxScore` = `std::max(11, 7)` = `11`.
  * Update `max_i_plus_val`: `std::max(8, values[3] + 3)` = `std::max(8, 5)` = `8`.

**`j = 4`** (`values[4] = 6`)

  * `currentScore` = `max_i_plus_val + (values[4] - 4)` = `8 + (6 - 4)` = `8 + 2` = `10`.
  * `maxScore` = `std::max(11, 10)` = `11`.
  * Update `max_i_plus_val`: `std::max(8, values[4] + 4)` = `std::max(8, 10)` = `10`.

**End of loop.** The function returns `maxScore`, which is `11`.
It found the same correct answer, but so much faster\!

#### **Compare Speed and Memory**

  * **Time (Speed):**

      * **Brute-Force:** $O(N^2)$. For 50,000 spots, this was \~2.5 billion steps.
      * **Optimized:** $O(N)$. For 50,000 spots, this is just 50,000 steps.
      * This is a massive improvement. The optimized solution is thousands of times faster.

  * **Space (Memory):**

      * **Brute-Force:** $O(1)$.
      * **Optimized:** $O(1)$.
      * Our new solution is just as memory-efficient as the simple one. We only added a couple of variables.

-----

### **Step 5: How to Explain Your Solution (The Interview Plan)**

Having a clear, concise explanation is as important as finding the solution itself. Here is a simple template you can follow.

#### **Provide a Clear Template**

Here is a step-by-step guide to explain your optimized solution.

1.  **The Goal:**
    "The goal of this problem is to find the maximum score from a pair of sightseeing spots, `i` and `j`, where `i` must come before `j`. The score is calculated as `values[i] + values[j] + i - j`."

2.  **My Solution Idea:**
    "My approach is to optimize this from a slow $O(N^2)$ brute-force solution to a much faster $O(N)$ linear solution. The key insight comes from rearranging the scoring formula. I broke `values[i] + values[j] + i - j` into two parts: one that depends only on `i`, and one that depends only on `j`. The rearranged formula is `(values[i] + i) + (values[j] - j)`."

3.  **How it Works:**
    "This rearrangement allows me to solve the problem in a single pass. I iterate through the array, treating each element as the second spot in the pair, `j`. As I iterate, I maintain a variable that keeps track of the best possible first part of the score, which is the maximum `values[i] + i` I have seen so far.

    At each spot `j`, I do two things:
    * First, I calculate the potential score by adding the `j` part (`values[j] - j`) to the best `i` part I've found so far. I update my overall maximum score if this new score is higher.
    * Second, I update my 'best `i` part' variable for the *next* iteration by seeing if the current spot `j` offers a better `values[j] + j` value."

4.  **Performance:**
    "This algorithm is very efficient.
    * The **time complexity is $O(N)$** because it only requires a single pass through the input array.
    * The **space complexity is $O(1)$** because I only use a few extra variables to store the running maximums, regardless of the input size."

5.  **Tricky Cases:**
    "The problem constraints state that the input array will always have at least two elements, which simplifies the logic as I don't need to handle edge cases like empty or single-element arrays."

---
Excellent. Let's wrap up with the final and most important step: understanding the key lessons from this problem so you can solve other problems like it in the future.

---

### **Step 6: Key Lessons and Future Problems**

#### **The Main Pattern**

The key pattern here has two parts:
1.  **Algebraic Manipulation:** The first breakthrough was to rearrange the formula `values[i] + values[j] + i - j` into `(values[i] + i) + (values[j] - j)`. This separated the problem into two independent parts.
2.  **Dynamic Programming (Keeping Track of the Best So Far):** The second breakthrough was realizing you can solve this in one pass by keeping a running record of the best possible "first part" of the pair (`values[i] + i`).

This combination is a powerful technique for optimization.

#### **Clues for Next Time**

How do you spot when to use this pattern in the future? Look for these clues in the problem description:

* The problem asks you to find the **maximum or minimum value** that comes from a **pair of elements** (`i` and `j`) in an array.
* The scoring formula involves both `i` and `j`. Your first instinct should be: "Can I rearrange this formula to separate the `i` parts from the `j` parts?"
* If you can separate the formula, it often means you can solve it by iterating through the array once as `j` while remembering the "best" `i` found so far.

#### **One More Problem**

A perfect problem to practice this exact pattern on is:

* **[121. Best Time to Buy and Sell Stock]**

In that problem, you're given an array of stock prices. You want to find the maximum profit you can make by buying on one day and selling on a later day. The profit is `price[j] - price[i]`. This is another "best pair" problem. You can solve it in one pass by iterating through the prices and "keeping track of the best so far" (in this case, the minimum price you've seen).

---
