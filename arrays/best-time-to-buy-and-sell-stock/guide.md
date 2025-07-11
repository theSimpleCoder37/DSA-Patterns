### **Step 1: Understanding the Problem (Breaking It Down)**

Let's make sure we understand exactly what the problem is asking us to do.

#### **Tell a Simple Story**

Imagine you're a kid who loves toy cars. The price of a specific popular toy car changes every single day. You have a list of its prices for the next few days.

You want to buy the toy car on one day and sell it on a **later day** to make the biggest possible profit. Your job is to figure out the maximum amount of money you can make. If you can't make any money (because the price only goes down), then your profit is zero.

For example, if the prices for the next few days are `[7, 1, 5, 3, 6, 4]`:

  * You could buy it on Day 2 when the price is **$1**.
  * Then, you could sell it on Day 5 when the price is **$6**.
  * Your profit would be $6 - 1 = 5$. This turns out to be the best you can do\!

#### **Explain the Parts**

  * **`Inputs`:** We get a list of numbers. In C++, this is a `vector<int> prices`. Each number in the list is the price of the stock on a given day.
  * **`Outputs`:** We must return one single number: the highest possible profit we can make.
  * **`Goal`:** Find the best day to buy and the best day to sell (the sell day must be *after* the buy day) to get the maximum profit. If no profit is possible, the goal is to return 0.

#### **Show with a Drawing**

Let's visualize the prices `[7, 1, 5, 3, 6, 4]` on a chart. We want to find the lowest valley (our buy point) and the highest peak that comes *after* it (our sell point).

```
Price
  ^
7 | #
6 | . . . . # .
5 | . . # . . .
4 | . . . . . #
3 | . . . # . .
2 | . . . . . .
1 | . # . . . .
0 +----------------> Day
    1 2 3 4 5 6

Buy here (Price 1) --^
          Sell here (Price 6) --^
```

The difference between the sell point (6) and the buy point (1) is our profit (5).

#### **List the Tricky Cases (Edge Cases)**

We need to be careful about a few special situations:

  * **Prices only go down:** If the prices are `[7, 6, 4, 3, 1]`, we can never make a profit. The best we can do is buy and sell for no gain, so the max profit is `0`.
  * **Empty list:** What if we are given no prices at all, `[]`? We can't buy or sell. The profit is `0`.
  * **Only one price:** If the list is `[5]`, we can't buy on one day and sell on a *later* day. So, the profit is `0`.

-----

### **Step 2: The First, Simple Solution (The Brute-Force Way)**

#### **Explain the Simple Idea**

The most direct way to solve this is to try every possible combination. Think of it like this:

1.  Pick a day to **buy** the stock. Let's call this `buy_day`.
2.  Then, check every single day *after* the `buy_day` as a possible day to **sell**. Let's call this `sell_day`.
3.  For each pair of `buy_day` and `sell_day`, calculate the profit.
4.  Keep a variable, let's call it `max_profit_so_far`, and whenever you find a profit that is bigger than this variable, you update it.

We will do this for every possible `buy_day` in the list. After checking all combinations, `max_profit_so_far` will hold the final answer.

#### **Write the C++ Code**

Here is the C++ code that does exactly what we just described.

```cpp
#include <vector>
#include <algorithm> // We use this for std::max

class Solution {
public:
    int maxProfit(std::vector<int>& prices) {
        // WHY: We need a variable to store the biggest profit we've found so far.
        // We start it at 0 because if we can't make a profit, 0 is the correct answer.
        int max_profit = 0;

        // WHY: This is the outer loop to pick our "buy" day.
        // It goes from the first day (index 0) to the end of the list.
        for (int i = 0; i < prices.size(); ++i) {
            
            // WHY: This is the inner loop to pick our "sell" day.
            // It MUST start on the day *after* our buying day (i + 1).
            for (int j = i + 1; j < prices.size(); ++j) {
                
                // WHY: Calculate the profit for this specific pair of buy and sell days.
                // Profit = Selling Price - Buying Price
                int profit = prices[j] - prices[i];
                
                // WHY: If the profit from this pair is bigger than the best profit
                // we've recorded so far, we have a new best.
                if (profit > max_profit) {
                    // WHY: Update our record to this new, higher profit.
                    max_profit = profit;
                }
            }
        }
        
        // WHY: After checking all possible pairs, this variable holds the
        // single best profit we could find. We return it.
        return max_profit;
    }
};
```

#### **Show a Detailed "Dry Run"**

Let's walk through our example `prices = {7, 1, 5, 3, 6, 4}` to see the code in action.

  - **Initial State:** `max_profit` starts at `0`.

  - **Outer loop starts:** `i = 0` (Buy price is `prices[0]`, which is `7`).

      - **Inner loop starts:**
          - `j = 1` (Sell price `1`): `profit = 1 - 7 = -6`. This is not greater than `max_profit` (0).
          - `j = 2` (Sell price `5`): `profit = 5 - 7 = -2`. Not greater than `0`.
          - `j = 3` (Sell price `3`): `profit = 3 - 7 = -4`. Not greater than `0`.
          - `j = 4` (Sell price `6`): `profit = 6 - 7 = -1`. Not greater than `0`.
          - `j = 5` (Sell price `4`): `profit = 4 - 7 = -3`. Not greater than `0`.

  - **Outer loop continues:** `i = 1` (Buy price is `prices[1]`, which is `1`).

      - **Inner loop starts:**
          - `j = 2` (Sell price `5`): `profit = 5 - 1 = 4`. This is greater than `max_profit` (0). **`max_profit` is now `4`**.
          - `j = 3` (Sell price `3`): `profit = 3 - 1 = 2`. Not greater than `4`.
          - `j = 4` (Sell price `6`): `profit = 6 - 1 = 5`. This is greater than `max_profit` (4). **`max_profit` is now `5`**.
          - `j = 5` (Sell price `4`): `profit = 4 - 1 = 3`. Not greater than `5`.

  - **Outer loop continues:** `i = 2` (Buy price is `prices[2]`, which is `5`).

      - **Inner loop starts:**
          - `j = 3` (Sell price `3`): `profit = 3 - 5 = -2`. Not greater than `5`.
          - `j = 4` (Sell price `6`): `profit = 6 - 5 = 1`. Not greater than `5`.
          - `j = 5` (Sell price `4`): `profit = 4 - 5 = -1`. Not greater than `5`.

The process continues, but no new profit will be greater than 5. Finally, the function returns `max_profit`, which is `5`.

#### **Analyze Speed and Memory (The Easy Way)**

  * **Time (Speed):** The most important thing to notice is the **loop inside another loop**.

      * The outer loop runs about `N` times (where `N` is the number of days).
      * For *each* of those `N` times, the inner loop also runs about `N` times.
      * This means the total number of checks is roughly $N \\times N$, or $O(N^2)$.
      * If we have 10,000 days, this is $10,000 \\times 10,000 = 100,000,000$ operations. This is very **slow**\!

  * **Space (Memory):** We only created a few variables to hold single numbers (`max_profit`, `i`, `j`, `profit`). We didn't create a big new list or any other data structure that grows with the input size.

      * Because the extra memory we use is small and constant, we say the space complexity is **$O(1)$**. This is very **good**.

-----
### **Step 3: Thinking Towards a Better Way (The Expert's Thought Process)**

#### **Find the Slow Part**

Let's look at our brute-force code again. Where is the slowness coming from?

```cpp
for (int i = 0; i < prices.size(); ++i) {      // This loop runs N times
    for (int j = i + 1; j < prices.size(); ++j) {  // This loop ALSO runs up to N times
        // ... work ...
    }
}
```

The slow part is the **inner loop**. For every single buying day we pick, we are forced to scan through *all* the remaining days to find the best selling price. This is a lot of repeated work.

For example, with `[7, 1, 5, 3, 6, 4]`:

  * When we check `i=0` (buy at 7), we scan `[1, 5, 3, 6, 4]` to find the best sale price.
  * When we check `i=1` (buy at 1), we scan `[5, 3, 6, 4]`.
  * When we check `i=2` (buy at 5), we scan `[3, 6, 4]`.

See how we keep re-scanning the end of the list? We need to find a way to get the answer without this repeated scanning.

#### **Reveal the "Aha\!" Idea**

An expert would stop and ask a key question: **"As I process the list one day at a time, what is the *minimum* information I need to remember from the past to make a decision today?"**

Let's think about it. If I'm standing on a particular day, say Day 5 (price is $6), and I want to calculate the best possible profit I can make if I sell *today*, what do I need to know?

I need to know the **lowest possible price I could have bought the stock for on any day *before* today.**

That's the "Aha\!" moment. We don't need to check every previous day. We only need to remember one thing: the lowest price we've seen so far.

This gives us a new, much smarter plan:

1.  Go through the list of prices just **once**, from left to right.
2.  Keep track of two variables as we go:
      * `min_price_so_far`: The lowest stock price we've seen up to the current day.
      * `max_profit_so_far`: The biggest profit we've calculated up to the current day.
3.  On each day, we do two things:
      * First, we ask: "Is today's price lower than my `min_price_so_far`?" If it is, we update our minimum. This becomes our new best time to buy.
      * Second, we ask: "What is the profit if I sell today?" We calculate `today's_price - min_price_so_far`. If this profit is bigger than our `max_profit_so_far`, we update our max profit.

By the time we reach the end of the list, we will have found the answer in just one pass.

#### **Draw the New Idea**

Let's trace this new idea with `prices = {7, 1, 5, 3, 6, 4}`.

```
We have two trackers:
min_price_so_far = ?
max_profit_so_far = 0

Let's go day by day:
------------------------------------------------------------------
Day 1 (Price 7):
  - Is 7 a new minimum? Yes. -> min_price_so_far = 7
  - Profit if we sell today? (7 - 7) = 0. No change to max profit.

  Current state: min_price_so_far = 7, max_profit_so_far = 0
------------------------------------------------------------------
Day 2 (Price 1):
  - Is 1 a new minimum? Yes. -> min_price_so_far = 1
  - Profit if we sell today? (1 - 1) = 0. No change to max profit.

  Current state: min_price_so_far = 1, max_profit_so_far = 0
------------------------------------------------------------------
Day 3 (Price 5):
  - Is 5 a new minimum? No, 1 is still lower.
  - Profit if we sell today? (5 - 1) = 4. This is a new max profit!

  Current state: min_price_so_far = 1, max_profit_so_far = 4
------------------------------------------------------------------
Day 4 (Price 3):
  - Is 3 a new minimum? No.
  - Profit if we sell today? (3 - 1) = 2. Not better than 4.

  Current state: min_price_so_far = 1, max_profit_so_far = 4
------------------------------------------------------------------
Day 5 (Price 6):
  - Is 6 a new minimum? No.
  - Profit if we sell today? (6 - 1) = 5. This is a new max profit!

  Current state: min_price_so_far = 1, max_profit_so_far = 5
------------------------------------------------------------------
Day 6 (Price 4):
  - Is 4 a new minimum? No.
  - Profit if we sell today? (4 - 1) = 3. Not better than 5.

  Current state: min_price_so_far = 1, max_profit_so_far = 5
------------------------------------------------------------------
We reached the end. The answer is 5.
```

See? We got the same answer, but we only walked through the list one time.

-----

### **Step 4: The Smart Solution (The Optimized Code)**

#### **Write the Optimized C++ Code**

This code uses our new one-pass strategy. Notice how much simpler it is—there are no nested loops, just one simple walk through the list.

```cpp
#include <vector>
#include <algorithm> // For std::min and std::max
#include <limits>    // For std::numeric_limits

class Solution {
public:
    int maxProfit(std::vector<int>& prices) {
        // WHY: This variable will keep track of the lowest price we have seen so far.
        // We start it with a very large number so that the first price in our list
        // is guaranteed to be lower, setting our initial minimum.
        int min_price = std::numeric_limits<int>::max();

        // WHY: This will store the biggest profit we find. We start at 0, because
        // if we can't make a profit, 0 is the answer.
        int max_profit = 0;

        // WHY: We use a single, simple loop to go through each price just once.
        // This is much faster than nested loops.
        for (int current_price : prices) {
            
            // WHY: At each day, we first check if the current price is a new minimum.
            // If it is, this is our new best day to "buy". We update our minimum
            // and continue to the next day.
            if (current_price < min_price) {
                min_price = current_price;
            } 
            // WHY: If the current price is NOT a new minimum, it's a potential "sell" day.
            // We calculate the potential profit if we sold today (current_price) after
            // buying at the lowest price we've seen so far (min_price).
            else if (current_price - min_price > max_profit) {
                // WHY: If this potential profit is the biggest one we've ever seen,
                // we update our record for the max profit.
                max_profit = current_price - min_price;
            }
        }

        // WHY: After checking every day, max_profit holds the best possible result.
        return max_profit;
    }
};
```

#### **Give a Simple Analogy**

We can call this pattern the **"Track the Minimum"** approach.

Think of it like walking along a hiking trail with many hills and valleys. You want to find the biggest possible climb from a low point to a later high point. As you walk, you only need to keep two things in your memory:

1.  The lowest valley you've been in so far (`min_price`).
2.  The biggest climb you've made so far (`max_profit`).

At every step, you're either entering a new, deeper valley (updating `min_price`) or you're climbing up from the lowest valley you've seen (potentially updating `max_profit`).

#### **Show the "Dry Run"**

Let's do a quick dry run with `prices = {7, 1, 5, 3, 6, 4}` to see this efficient code in action.

  * **Initial State:** `min_price` is a huge number (like `2147483647`), `max_profit` is `0`.

  * **Loop 1 (current\_price = 7):**

      * `7 < min_price` is true. So, `min_price` becomes `7`.
      * `max_profit` is still `0`.

  * **Loop 2 (current\_price = 1):**

      * `1 < min_price` (which is 7) is true. So, `min_price` becomes `1`.
      * `max_profit` is still `0`.

  * **Loop 3 (current\_price = 5):**

      * `5 < min_price` (which is 1) is false.
      * We check the `else if`. Profit is `5 - 1 = 4`. `4 > max_profit` (which is 0) is true.
      * `max_profit` becomes `4`.

  * **Loop 4 (current\_price = 3):**

      * `3 < min_price` (which is 1) is false.
      * We check the `else if`. Profit is `3 - 1 = 2`. `2 > max_profit` (which is 4) is false. No change.

  * **Loop 5 (current\_price = 6):**

      * `6 < min_price` (which is 1) is false.
      * We check the `else if`. Profit is `6 - 1 = 5`. `5 > max_profit` (which is 4) is true.
      * `max_profit` becomes `5`.

  * **Loop 6 (current\_price = 4):**

      * `4 < min_price` (which is 1) is false.
      * We check the `else if`. Profit is `4 - 1 = 3`. `3 > max_profit` (which is 5) is false. No change.

The loop finishes. The function returns `max_profit`, which is `5`.

#### **Compare Speed and Memory**

Let's see how much better this is:

  * **Time (Speed):**

      * Old Way: $O(N^2)$. Slow.
      * **New Way: $O(N)$.** We only go through the list once. This is extremely fast and efficient. If the list has 10 million prices, we do 10 million operations, not 10 million *squared*.

  * **Space (Memory):**

      * Old Way: $O(1)$. Good.
      * **New Way: $O(1)$.** Still great. We only used a couple of extra variables.

We dramatically improved the speed without using any extra memory. This is a fantastic trade-off and what interviewers love to see.

-----

### **Step 5: How to Explain Your Solution (The Interview Plan)**

Imagine an interviewer asks you to solve this problem. Here is a simple, clear template you can follow to explain your optimized solution.

#### **Provide a Clear Template**

1.  **The Goal:** Start by restating the problem to show you understand it.
    * "The goal of this problem is to find the maximum profit from buying a stock on one day and selling it on a future day. If no profit is possible, the answer is 0."

2.  **My Solution Idea:** Give a high-level summary of your smart approach.
    * "My approach is to solve this in a single pass, which is very efficient. I'll iterate through the prices just once, and as I go, I'll keep track of two key pieces of information: the **lowest price** seen so far and the **maximum profit** found so far."

3.  **How it Works:** Describe the logic of your code step-by-step.
    * "I'll initialize a variable `min_price` to a very large value and `max_profit` to 0."
    * "Then, for each price in the list, I'll do two checks:"
        * "First, I see if the current price is a new `min_price`. If it is, I update my `min_price` because I've found a better day to buy."
        * "If it's not a new minimum, I calculate the potential profit by subtracting `min_price` from the current price. If this profit is greater than my `max_profit`, I update `max_profit`."
    * "After the loop finishes, `max_profit` will hold the final answer."

4.  **Performance:** State the time and space complexity and explain why.
    * "This solution is very fast because its time complexity is **$O(N)$**, as it only requires a single pass through the input array."
    * "The space complexity is **$O(1)$** because I only use a constant amount of extra memory for a couple of variables, regardless of the input size."

5.  **Tricky Cases:** Briefly mention how you handled edge cases.
    * "This approach also handles tricky cases naturally. If the list is empty, the loop doesn't run and it returns 0. If the prices only decrease, the profit calculation will never beat the initial `max_profit` of 0, so the correct answer is returned."

---

### **Step 6: Key Lessons and Future Problems**

#### **The Main Pattern**

The key pattern here is called the **"Single Pass"** or **"Greedy"** approach.

The main idea is that for some problems, you don't need to look at all possible combinations. Instead, you can find the best solution by just going through the list once and making the "greediest" or most obvious best choice at each step. Here, our greedy choice was simple: always remember the lowest buy price found so far. This simple rule was enough to solve the entire problem efficiently.

#### **Clues for Next Time**

How do you know when to think of this pattern for future problems? Look for these clues:

* The problem asks you to find a **maximum or minimum value** over an array (e.g., maximum profit, longest sequence, smallest difference).
* You come up with a slow $O(N^2)$ solution that uses nested loops. This is a huge hint that you should ask yourself: **"Can I get the same result by going through the list just once and keeping track of some important values as I go?"**

This question will often unlock a much faster $O(N)$ solution.

#### **One More Problem**

The best way to make a lesson stick is to practice it again. A classic problem that uses the exact same thinking process is:

* **Problem:** [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)
* **How it's similar:** The simple brute-force solution is $O(N^2)$ (checking the sum of every possible subarray). However, the expert solution is $O(N)$. You solve it by walking through the array once and keeping track of the best sum you've seen so far. It's a perfect next step for practicing this pattern.

---
