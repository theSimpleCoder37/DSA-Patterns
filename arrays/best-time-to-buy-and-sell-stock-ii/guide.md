
# [122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)
---
### **Step 1: Understanding the Problem (Breaking It Down)**

#### **Tell a Simple Story**

Imagine you are a fruit seller at a local market. Every day, the price of apples changes.

You have a special ability: you can buy an apple on one day and sell it on any later day. But here's the most important rule for this specific problem: **you can do this as many times as you want.**

For example, you could:

  - Buy an apple on Monday when the price is low.
  - Sell it on Tuesday when the price is high.
  - Buy *another* apple on Wednesday (if the price is low again).
  - Sell it on Friday (if the price is high).

The only catch is you can't own more than one apple at a time. You **must sell** the apple you are holding before you buy a new one. Your goal is simple: make the biggest total profit you possibly can by the end of the week.

#### **Explain the Parts**

  * **`Inputs`**: We are given a list of numbers, let's call it `prices`. Each number in the list represents the price of a stock on a given day.

      - Example: `prices = [7, 1, 5, 3, 6, 4]`

  * **`Outputs`**: We need to give back a single number, which is the **maximum total profit** we can possibly make.

      - For the example above, the best profit is 7.

  * **`Goal`**: The main task is to find the best moments to buy and sell the stock, maybe even multiple times, to make the total profit as large as possible.

#### **Show with a Drawing**

Let's visualize the example `prices = [7, 1, 5, 3, 6, 4]`. Think of it as a price chart.

```
Price Chart
-----------
Day 1: $7
Day 2: $1  <-- A low point. Let's BUY here.
Day 3: $5  <-- A high point. Let's SELL here. (Profit: $5 - $1 = $4)
Day 4: $3  <-- Another low point. Let's BUY again.
Day 5: $6  <-- Another high point. Let's SELL again. (Profit: $6 - $3 = $3)
Day 6: $4

Total Profit = (First Profit) + (Second Profit) = $4 + $3 = $7
```

This shows one way to get a profit of 7. Our job is to be sure this is the *maximum* possible.

#### **List the Tricky Cases (Edge Cases)**

It's always smart to think about strange inputs we might get.

  * **An empty list `[]`**: If there are no days and no prices, we can't buy or sell. The profit must be **0**.
  * **A list with only one price `[10]`**: We can buy, but there are no future days to sell. So, the profit must be **0**.
  * **Prices are always going down `[10, 8, 4, 1]`**: If we buy, we will always lose money. The best strategy is to do nothing. The profit is **0**.
  * **Prices are always going up `[1, 2, 3, 4, 5]`**: This is an interesting one\! We can buy at 1 and sell at 5 for a profit of `5 - 1 = 4`. Or, we could buy and sell every day: `(2-1) + (3-2) + (4-3) + (5-4) = 1 + 1 + 1 + 1 = 4`. The result is the same\! This is a very important clue.

-----

### **Step 2: The First, Simple Solution (The Brute-Force Way)**

#### **Explain the Simple Idea**

The most basic way to solve this is to try every single possibility. On any given day, we have a choice to make. Let's think about our state: at any point, we are either **holding a stock** or we are **not holding a stock**.

Let's write a function that explores every path based on this idea:

1.  **If we are NOT holding a stock today:**

      * **Choice A: Buy.** We can buy the stock at today's price. Our "account balance" goes down by `prices[today]`. We then move to the next day, and now we are in the "holding a stock" state.
      * **Choice B: Do Nothing.** We can just skip today and move to the next day. We are still in the "not holding a stock" state.

2.  **If we ARE holding a stock today:**

      * **Choice A: Sell.** We can sell the stock at today's price. Our "account balance" goes up by `prices[today]`. We then move to the next day, and now we are back in the "not holding a stock" state (since we can buy again).
      * **Choice B: Do Nothing.** We can keep holding our stock and move to the next day. We are still in the "holding a stock" state.

We can write a program that tries all of these choices for all the days and then tells us which path of choices gives the highest final profit. This is a classic "recursion" problem, where a function calls itself to solve smaller parts of the problem.

#### **Write the C++ Code**

This code explores every path. It's slow, but it correctly checks everything.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

class Solution {
public:
    int maxProfit(std::vector<int>& prices) {
        // WHY: We start our process on the first day (index 0).
        // The 'true' means we are allowed to buy, so we are "not holding" a stock.
        return solve(0, true, prices);
    }

private:
    // WHY: This is a helper function that does all the work.
    // 'day' is the current day we are considering.
    // 'canBuy' tells us our state: true if we can buy, false if we must sell.
    int solve(int day, bool canBuy, std::vector<int>& prices) {
        // WHY: This is our base case. If we've gone past the last day,
        // no more profit can be made. We stop here and return 0.
        if (day >= prices.size()) {
            return 0;
        }

        if (canBuy) {
            // WHY: If we can buy, we explore two futures.

            // Choice 1: Buy the stock today.
            // Our profit is what we get from future days (now we 'must sell')
            // minus the cost of today's stock.
            int buy_profit = solve(day + 1, false, prices) - prices[day];

            // Choice 2: Don't buy today (just wait).
            // Our profit is what we get from future days (we can still buy tomorrow).
            int wait_profit = solve(day + 1, true, prices);

            // WHY: We return the better of the two choices.
            return std::max(buy_profit, wait_profit);
        } else { // We must sell (or hold).
            // WHY: If we are holding a stock, we explore two futures.

            // Choice 1: Sell the stock today.
            // Our profit is today's price plus whatever profit we can make
            // in the future (we can 'buy' again tomorrow).
            int sell_profit = solve(day + 1, true, prices) + prices[day];

            // Choice 2: Don't sell today (hold onto it).
            // Our profit is whatever we can make in the future
            // by continuing to hold the stock.
            int hold_profit = solve(day + 1, false, prices);

            // WHY: We return the better of the two choices.
            return std::max(sell_profit, hold_profit);
        }
    }
};
```

#### **Show a Detailed "Dry Run"**

Let's trace this with a tiny example: `prices = [1, 5]`.
`maxProfit` calls `solve(0, true, [1, 5])`.

1.  **`solve(day=0, canBuy=true)`**: We are at day 0, price is 1. We can buy.

      * **Choice: Buy.** We calculate `solve(1, false) - 1`.
      * **Choice: Wait.** We calculate `solve(1, true)`.

2.  Let's follow the **"Buy"** path first. We need to figure out `solve(1, false)`.

      * **`solve(day=1, canBuy=false)`**: We are at day 1, price is 5. We are holding a stock.
          * **Choice: Sell.** We calculate `solve(2, true) + 5`.
              * `solve(2, true)` sees that `day=2` is past the end, so it returns `0`.
              * The result for selling is `0 + 5 = 5`.
          * **Choice: Hold.** We calculate `solve(2, false)`.
              * `solve(2, false)` also sees `day=2` is past the end, so it returns `0`.
          * `solve(1, false)` returns `max(5, 0) = 5`.
      * So the final result of our original "Buy" choice is `5 - 1 = 4`.

3.  Now let's follow the **"Wait"** path from the start. We need to figure out `solve(1, true)`.

      * **`solve(day=1, canBuy=true)`**: We are at day 1, price is 5. We can buy.
          * **Choice: Buy.** `solve(2, false) - 5`. This gives `0 - 5 = -5`.
          * **Choice: Wait.** `solve(2, true)`. This gives `0`.
          * `solve(1, true)` returns `max(-5, 0) = 0`.
      * So the final result of our original "Wait" choice is `0`.

4.  Finally, back at the very first call, we compare the two main paths: `max(Buy path, Wait path)` which is `max(4, 0)`.

      * The final answer is **4**.

As you can see, we have to go down many paths just for a list with two prices. Imagine a list with 30 prices\!

#### **Analyze Speed and Memory (The Easy Way)**

  * **Time (Speed):** At each day, our function branches into two new calls. If we have `N` days, we are making roughly $2 \\times 2 \\times 2 \\dots$ (N times) calls. This is written as $O(2^N)$. This is called **exponential time**. It is **extremely slow** and will fail on larger inputs.

  * **Space (Memory):** Each function call takes a little bit of memory. Because our function can call itself up to `N` times deep (once for each day), the memory usage grows with the number of prices. This is written as $O(N)$.

-----

### **Step 3: Thinking Towards a Better Way (The Expert's Thought Process)**

#### **Find the Slow Part**

The brute-force solution is slow because it does a massive amount of repeated work. In our dry run, the function `solve(day, canBuy)` was called for the same day and same state (`canBuy`) multiple times through different paths. Imagine a list of 30 prices. The function would calculate the best profit from day 20 onwards over and over again. This is what we need to fix.

#### **Reveal the "Aha\!" Idea**

An expert wouldn't even start with that complex recursive solution. They would look for a simpler pattern in the data itself.

Let's look at our price chart again.
`Prices: [7, 1, 5, 3, 6, 4]`

The goal is to make money. We only make money when the price goes **up**.

*An expert's first thought might be:* "Okay, let's find the low points (valleys) and sell at the next high point (peaks)."

```
      ^
Price |
      |         /---- 6 (Peak)
      |        /
      |   5 --/
      |   |
      |   |        /---- 3 (Valley)
      |   |       /
      |   \      /
      |    \----1 (Valley)
      |
    --7---------------------> Day
```

Following this, we would:

1.  Buy at the valley `1`. Sell at the next peak `5`. Profit = `5 - 1 = 4`.
2.  Buy at the next valley `3`. Sell at the next peak `6`. Profit = `6 - 3 = 3`.
3.  Total Profit = `4 + 3 = 7`.

This seems to work\! But finding all these peaks and valleys can be complicated to code.

*Then comes the **"Aha\!" idea***: "Wait a minute... what if I zoom in and look at the price changes day-by-day?"

Let's just look at the profit we could make if we could buy yesterday and sell today.

  * **Day 1 to 2:** Price goes from 7 to 1. That's a loss. I don't want that.
  * **Day 2 to 3:** Price goes from 1 to 5. That's a **gain of 4**. I'll take it\!
  * **Day 3 to 4:** Price goes from 5 to 3. A loss. No, thanks.
  * **Day 4 to 5:** Price goes from 3 to 6. That's a **gain of 3**. I'll take that too\!
  * **Day 5 to 6:** Price goes from 6 to 4. A loss. Ignore.

If I add up all the gains I found: `4 + 3 = 7`.

It's the exact same answer\! Why does this simple trick work?

Because a long climb in price (like from 1 to 5) is just the sum of all the small daily climbs in between.
`Profit (1 to 5) = 5 - 1 = 4`

This is the same as:
`(Price on Day 3 - Price on Day 2) + (Price on Day 4 - Price on Day 3) ...`
Because we are allowed to buy and sell as many times as we want, we can think of it as "selling" at the end of every day that had a price increase. This means we can simply add up all the day-to-day profits. We don't need to track complex peaks and valleys at all.

**The core "Aha\!" idea is: The total profit is the sum of all positive price changes between consecutive days.**

#### **Draw the New Idea**

This new logic is much simpler to draw. We just march down the list and check the day before.

```
Prices: [7, 1, 5, 3, 6, 4]
Total Profit = 0

Loop 1 (Day 2):
  Is today's price (1) > yesterday's price (7)? No.
  Profit remains 0.

Loop 2 (Day 3):
  Is today's price (5) > yesterday's price (1)? Yes.
  Add profit: 5 - 1 = 4.
  Total Profit is now 4.

Loop 3 (Day 4):
  Is today's price (3) > yesterday's price (5)? No.
  Profit remains 4.

Loop 4 (Day 5):
  Is today's price (6) > yesterday's price (3)? Yes.
  Add profit: 6 - 3 = 3.
  Total Profit is now 4 + 3 = 7.

Loop 5 (Day 6):
  Is today's price (4) > yesterday's price (6)? No.
  Profit remains 7.

End of list. Final answer is 7.
```

This avoids all the confusing recursive calls. It's just one simple loop.

-----

### **Step 4: The Smart Solution (The Optimized Code)**

This solution uses the "Aha\!" idea we just discovered: just add up all the day-to-day gains.

#### **Write the Optimized C++ Code**

```cpp
#include <iostream>
#include <vector>

class Solution {
public:
    int maxProfit(std::vector<int>& prices) {
        // WHY: Handle the edge case where no transaction is possible.
        // If there are 0 or 1 prices, we can't buy and then sell.
        if (prices.size() < 2) {
            return 0;
        }

        int totalProfit = 0; // WHY: A variable to keep track of our total earnings.

        // WHY: We loop through the prices starting from the second day (index 1).
        // This lets us compare each day's price to the day before it.
        for (int i = 1; i < prices.size(); ++i) {
            
            // WHY: This is the core logic. Check if today's price is higher
            // than yesterday's price.
            if (prices[i] > prices[i - 1]) {
                
                // WHY: If the price went up, this is a profit opportunity.
                // We add the difference (today's price - yesterday's price)
                // to our total profit. This is like buying yesterday and selling today.
                totalProfit += prices[i] - prices[i - 1];
            }
        }

        // WHY: After checking all the days, totalProfit holds the sum of
        // all possible day-to-day gains, which is the maximum profit.
        return totalProfit;
    }
};
```

#### **Give a Simple Analogy**

This technique is a perfect example of a **Greedy Algorithm**.

**Analogy:** Imagine you're a hiker climbing a mountain range. Your goal is to maximize your total altitude climbed. Instead of planning a complex route from the lowest valley to the highest peak, you make a simple, "greedy" decision every day: if the path in front of you goes up, you climb it and add that gain to your total. If the path goes down, you just walk it without counting it as a "gain". At the end of your journey, the sum of all your individual climbs is your total altitude gained.

Our code does the same thing: it greedily takes every small, guaranteed profit it sees.

#### **Show the "Dry Run"**

Let's quickly trace our example `prices = [7, 1, 5, 3, 6, 4]` with the new code.

  * Initialize `totalProfit = 0`.

  * The `for` loop starts with `i = 1`.

  * **`i = 1`**:

      * `prices[1]` (which is 1) vs `prices[0]` (which is 7).
      * Is `1 > 7`? No. Do nothing.
      * `totalProfit` is still `0`.

  * **`i = 2`**:

      * `prices[2]` (5) vs `prices[1]` (1).
      * Is `5 > 1`? Yes.
      * `totalProfit = 0 + (5 - 1) = 4`.

  * **`i = 3`**:

      * `prices[3]` (3) vs `prices[2]` (5).
      * Is `3 > 5`? No. Do nothing.
      * `totalProfit` is still `4`.

  * **`i = 4`**:

      * `prices[4]` (6) vs `prices[3]` (3).
      * Is `6 > 3`? Yes.
      * `totalProfit = 4 + (6 - 3) = 7`.

  * **`i = 5`**:

      * `prices[5]` (4) vs `prices[4]` (6).
      * Is `4 > 6`? No. Do nothing.
      * `totalProfit` is still `7`.

The loop finishes. The function returns `7`. This was simple, fast, and gave the correct answer.

#### **Compare Speed and Memory**

Let's see how much better this is.

  * **Time (Speed):**

      * Brute-Force: $O(2^N)$. For N=30, this is over a billion operations. 🐌
      * **Optimized:** We use a single `for` loop that goes through the list once. This is **$O(N)$**. For N=30, this is about 30 operations. 🚀 This is a massive improvement\!

  * **Space (Memory):**

      * Brute-Force: $O(N)$, because of the deep recursive calls.
      * **Optimized:** We only created a couple of variables (`totalProfit`, `i`). The memory we use is constant and doesn't depend on the size of the `prices` list. This is **$O(1)$**. This is also a fantastic improvement.

-----

### **Step 5: How to Explain Your Solution (The Interview Plan)**

Here is a clear, step-by-step template you can follow to explain your solution.

1.  **The Goal:**
    > "The problem asks us to find the maximum possible profit from buying and selling a stock, where we can perform as many transactions as we like. The main rule is that we must sell our current stock before buying a new one."

2.  **My Solution Idea:**
    > "My approach is to use a **Greedy Algorithm**. The key insight is that we don't need to find the highest peaks and lowest valleys. The maximum profit is simply the sum of all the positive price increases between consecutive days."

3.  **How it Works:**
    > "I implement this by iterating through the price list with a single loop, starting from the second day. In each step, I compare the current day's price with the previous day's price. If the price has gone up, I treat that as a small, completed transaction and add that profit (`today's price - yesterday's price`) to a running total. If the price goes down or stays the same, I do nothing and just move to the next day."

4.  **Performance:**
    > "This solution is extremely efficient.
    > * The **time complexity is $O(N)$**, because it only requires one single pass through the array of N prices.
    > * The **space complexity is $O(1)$**, because I only use a single extra variable to store the total profit, no matter how large the input array is."

5.  **Tricky Cases:**
    > "My code also handles the important edge cases. For instance, if the input list has fewer than two prices, no transaction can be made. My code checks for this at the beginning and correctly returns a profit of 0."

***

### **Step 6: Key Lessons and Future Problems**

#### **The Main Pattern**
The key pattern we used here is called a **Greedy Algorithm**.

The main lesson is that sometimes a complex problem has a surprisingly simple solution. We didn't need to track buying and selling states or find complicated peaks and valleys. By making the best *local* choice at each step (i.e., taking any and every small, day-to-day profit), we ended up with the best *global* result.

#### **Clues for Next Time**
How do you spot a problem where a Greedy approach might work? Look for these clues:

* The problem asks you to find a **maximum** or **minimum** value (e.g., "maximum profit", "minimum cost", "shortest path").
* It feels like you can build the final answer piece by piece by making a series of simple choices.
* Ask yourself this question: "If I make the most obvious, best choice for myself *right now*, does that ruin my chances of getting the best overall answer later?" If the answer is "no", a greedy strategy is often a great path to try. In our problem, taking a small profit today never prevented us from taking another profit tomorrow.

#### **One More Problem**
A great problem to practice this same greedy way of thinking is **[55. Jump Game]** on LeetCode.

**Problem Idea:** You're standing at the start of a list of numbers. Each number tells you the maximum distance you can jump forward. The goal is to figure out if you can reach the very end of the list.

**Why it's similar:** At each position, you have to make a "greedy" choice: "Which jump should I take from here to maximize how far I can reach?" Just like our stock problem, making the best local choice helps you find the overall solution.

***

