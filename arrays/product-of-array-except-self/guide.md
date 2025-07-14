### **Step 1: Understanding the Problem (Breaking It Down)**

Before we even think about code, let's make sure we understand exactly what the problem is asking.

#### **Tell a Simple Story**

Imagine you and your friends are playing a game. Each of you has a card with a number on it. The goal is for every person to figure out the product of everyone else's numbers, *except their own*.

For example, if the numbers on the cards are 2, 3, 4, and 5:

  - The person with the '2' needs to calculate `3 * 4 * 5 = 60`.
  - The person with the '3' needs to calculate `2 * 4 * 5 = 40`.
  - The person with the '4' needs to calculate `2 * 3 * 5 = 30`.
  - The person with the '5' needs to calculate `2 * 3 * 4 = 24`.

Our job is to create a list of these final results for everyone.

#### **Explain the Parts**

  - **`Inputs`:** We are given a list of numbers. Let's call it `nums`. For example: `[1, 2, 3, 4]`.
  - **`Outputs`:** We must return a new list of the same size, where each position holds the product of all other numbers. Let's call this list `answer`.
  - **`Goal`:** For each number in the original `nums` list, we need to calculate the product of every *other* number in that list.

There are two very important rules given in the problem description:

1.  You must do this without using the division `/` operator.
2.  Your solution should be fast, running in what's called $O(N)$ time (we'll cover this later, but it basically means you can only go through the list a fixed number of times, not a loop inside a loop).

#### **Show with a Drawing**

Let's visualize the example `nums = [1, 2, 3, 4]`. We want to produce a new `answer` array.

```
Input:  nums = [ 1,  2,  3,  4 ]
              |   |   |   |
              V   V   V   V
Output: answer = [ ?,  ?,  ?,  ? ]
```

To get the first element of `answer`, we ignore `nums[0]` (which is 1) and multiply everything else:
`? = 2 * 3 * 4 = 24`

```
Input:  nums = [ 1,  2,  3,  4 ]
              ^
              |
           (ignore)

Output: answer = [ 24, ?,  ?,  ? ]
```

To get the second element, we ignore `nums[1]` (which is 2) and multiply everything else:
`? = 1 * 3 * 4 = 12`

```
Input:  nums = [ 1,  2,  3,  4 ]
                  ^
                  |
               (ignore)

Output: answer = [ 24, 12, ?,  ? ]
```

And so on, until the `answer` array is filled.
The final result should be `[24, 12, 8, 6]`.

#### **List the Tricky Cases (Edge Cases)**

We should always think about what could go wrong or what special inputs we might get.

  - **What if there's a zero?** If `nums = [1, 2, 0, 4]`, the product for any non-zero element will include that `0`, making their result `0`. But for the position where `0` is, the product is `1 * 2 * 4 = 8`. So the result would be `[0, 0, 8, 0]`.
  - **What if there are two or more zeros?** If `nums = [1, 0, 3, 0]`, then every single product will include at least one `0`. This means every single element in our `answer` array will be `0`.
  - **What about negative numbers?** These should work just fine with multiplication. `[-1, 2, -3, 4]` will have products that might be positive or negative. The logic doesn't change.
  - **What about an array with only two numbers?** If `nums = [2, 5]`, the answer should be `[5, 2]`. Our logic must work for this simple case.

The most important constraints are the rules: **no division** and it must be **fast** ($O(N)$). This hints that a simple, slow approach might be easy to think of, but a cleverer one is needed.

---

### **Step 2: The First, Simple Solution (The Brute-Force Way)**

Let's start with the most obvious solution that comes to mind. It might not be the best or fastest, but it's a great starting point because it directly follows the problem's definition.

#### **Explain the Simple Idea**

The problem asks us to find the product of *all other elements* for each element in the list. The simplest way to do this is to follow that instruction literally.

Here's the step-by-step logic:

1.  Create a new `answer` list of the same size as the input `nums` list, and fill it with ones to start.
2.  Go through each number in the `nums` list, one by one. Let's call the current number's position `i`.
3.  For this number at position `i`, we need to calculate the product of all *other* numbers. So, we will start another loop that goes through the `nums` list *again*, from beginning to end. Let's call this second loop's position `j`.
4.  Inside this second loop, we check if the position `j` is different from our main position `i`.
5.  If `j` is not the same as `i`, we multiply the number `nums[j]` into a running product for position `i`.
6.  After the inner loop finishes, we will have the final product for position `i`. We store this result in `answer[i]`.
7.  We repeat this for every single number in the `nums` list.

This is called a "brute-force" approach because we are using straightforward, repetitive logic without any clever tricks.

#### **Write the C++ Code**

Here is the C++ code that does exactly what we just described.

```cpp
#include <iostream>
#include <vector>

std::vector<int> productExceptSelf_bruteForce(std::vector<int>& nums) {
    // WHY: Get the total number of elements in the input list.
    // We need this to create our answer list and to set the boundaries for our loops.
    int n = nums.size();

    // WHY: Create the answer vector that we will eventually return.
    // It must be the same size as the input vector.
    std::vector<int> answer(n);

    // WHY: This is the main loop. It iterates through each element of the input `nums` array,
    // from the first (index 0) to the last. `i` is the index of the element
    // we are currently calculating the product for.
    for (int i = 0; i < n; ++i) {

        // WHY: For each `i`, we need to calculate a new product from scratch.
        // We initialize it to 1 because 1 is the multiplicative identity
        // (anything multiplied by 1 is itself).
        int product = 1;

        // WHY: This is the inner loop. For a given `i`, this loop goes through
        // EVERY element of `nums` again, from start to finish, using index `j`.
        for (int j = 0; j < n; ++j) {

            // WHY: This is the most important condition. We only want to multiply numbers
            // that are NOT at the current index `i`. If `j` and `i` are the same,
            // we skip the number at that position.
            if (i != j) {
                // WHY: We multiply the number at index `j` into our running `product`.
                product = product * nums[j];
            }
        }
        // WHY: After the inner loop has finished, `product` holds the total product
        // of all elements except `nums[i]`. We store this result in our answer array.
        answer[i] = product;
    }

    // WHY: After the outer loop finishes, the `answer` array is completely filled,
    // and we can return it.
    return answer;
}
```

#### **Show a Detailed "Dry Run"**

Let's walk through our example `nums = [1, 2, 3, 4]` to see this code in action.

**Outer loop starts: `i = 0`** (for `nums[0]` which is 1)

  - Create `product = 1`.
  - **Inner loop starts (`j` from 0 to 3):**
      - `j = 0`: `i` is equal to `j` (0 == 0). Do nothing.
      - `j = 1`: `i` is not equal to `j`. `product = product * nums[1]` -\> `product = 1 * 2 = 2`.
      - `j = 2`: `i` is not equal to `j`. `product = product * nums[2]` -\> `product = 2 * 3 = 6`.
      - `j = 3`: `i` is not equal to `j`. `product = product * nums[3]` -\> `product = 6 * 4 = 24`.
  - Inner loop finishes. Set `answer[0] = 24`.
  - `answer` is now `[24, ?, ?, ?]`

**Outer loop continues: `i = 1`** (for `nums[1]` which is 2)

  - Create `product = 1`.
  - **Inner loop starts (`j` from 0 to 3):**
      - `j = 0`: `i` is not equal to `j`. `product = product * nums[0]` -\> `product = 1 * 1 = 1`.
      - `j = 1`: `i` is equal to `j` (1 == 1). Do nothing.
      - `j = 2`: `i` is not equal to `j`. `product = product * nums[2]` -\> `product = 1 * 3 = 3`.
      - `j = 3`: `i` is not equal to `j`. `product = product * nums[3]` -\> `product = 3 * 4 = 12`.
  - Inner loop finishes. Set `answer[1] = 12`.
  - `answer` is now `[24, 12, ?, ?]`

...and so on for `i = 2` and `i = 3`. This process correctly fills the array.

#### **Analyze Speed and Memory (The Easy Way)**

  - **Time (Speed):** To figure out the speed, we count the loops. We have an outer loop that runs `N` times (where `N` is the number of items in `nums`). Inside that loop, we have *another* inner loop that also runs `N` times.
    For each of the `N` items, we are doing `N` steps of work.
    The total work is roughly $N \\times N$, which we write as **$O(N^2)$**.
    If `nums` has 10 items, we do about 100 operations. If it has 1,000 items, we do 1,000,000 operations. This can get very slow\!

  - **Space (Memory):** To figure out the memory, we count the extra storage we created.

    1.  `answer`: A new list of size `N`.
    2.  `n`, `i`, `j`, `product`: A few single variables.
        The problem statement usually says that the output array (`answer`) does not count as "extra" space. So, besides the `answer` array, we only used a few small variables. This amount of memory doesn't grow when the input list gets bigger. We call this **constant space**, or **$O(1)$**.

This solution works, but it fails one of the problem's rules: it's not $O(N)$. We need a faster way.

---

### **Step 3a: Thinking Towards a Different Better Way (Using Division)**

#### **Find the Slow Part**

As we identified before, the slow part of the brute-force method is the nested loop, which recalculates products over and over.

#### **Reveal the "Aha!" Idea**

An expert might ask: *"Instead of multiplying all *other* numbers, what if I just find the total product of *all* numbers first?"*

Let's say our array is `nums = [a, b, c, d]`.
The total product is `P = a * b * c * d`.

How do we find the product of everything *except* `a`? With the total product `P`, it's easy! It's just `P / a`.
-   Result for `a` is `(a * b * c * d) / a = b * c * d`.
-   Result for `b` is `(a * b * c * d) / b = a * c * d`.

This seems much faster! We would just need to:
1.  Go through the list once to calculate the total product `P`.
2.  Go through the list a second time, and for each `nums[i]`, calculate `answer[i] = P / nums[i]`.

**The Giant "BUT"**: This great idea relies entirely on the division operator, which is forbidden.

Also, what happens if there's a zero in the list?
If `nums = [1, 2, 0, 4]`, the total product `P` is `1 * 2 * 0 * 4 = 0`.
If we try to calculate `P / 1`, we get `0 / 1 = 0`. Correct.
If we try to calculate `P / 2`, we get `0 / 2 = 0`. Correct.
If we try to calculate `P / 4`, we get `0 / 4 = 0`. Correct.
But if we try to calculate `P / 0`, we get `0 / 0`, which is an error (division by zero).

So, this approach requires special logic to handle zeros. The code you provided does exactly this: it counts the zeros first and then decides what to do.

---

### **Step 3b: The (Almost) Smart Solution (Using Division)**

This solution is based on the idea we just discussed: calculate a total product and then handle the special cases involving zeros.

#### **Write the C++ Code**

Here is the code you asked me to analyze. I've added comments to explain the `WHY` behind each part.

```cpp
#include <iostream>
#include <vector>

// Note: This function is NOT a valid solution because it uses division.
std::vector<int> productExceptSelf_withDivision(std::vector<int>& nums) {
    // WHY: This variable is used as a COUNTER for the number of zeros in the array.
    // Its name 'product' is a bit confusing; 'zero_count' would be clearer.
    int zero_count = 0;

    // WHY: This variable will store the product of ALL non-zero numbers in the array.
    // It's initialized to 1, the multiplicative identity.
    int product_without_zero = 1;

    // --- Pass 1: Count zeros and calculate product of non-zeros ---
    // WHY: This loop goes through every number `num` in the input `nums`.
    for (int num : nums) {
        // WHY: If the current number is 0, we simply increment our zero counter.
        if (num == 0) {
            zero_count++;
        } else {
            // WHY: If the number is not 0, we multiply it into our running product.
            product_without_zero *= num;
        }
    }

    // WHY: Create the final result vector, with the same size as the input.
    std::vector<int> result(nums.size());

    // --- Pass 2: Build the result array based on the counts ---
    // WHY: This loop goes through the input array again, this time by index `i`,
    // to place the correct value into our `result` array.
    for (int i = 0; i < nums.size(); i++) {
        // WHY: This condition checks if the number we are currently looking at, `nums[i]`, is NOT zero.
        if (nums[i] != 0) {
            // WHY: If there is at least one zero somewhere in the array (`zero_count > 0`),
            // the product of all *other* elements must be 0.
            if (zero_count > 0) {
                result[i] = 0;
            } else {
                // WHY: If there are NO zeros in the array, the result is the total product
                // divided by the current number. THIS IS THE FORBIDDEN STEP.
                result[i] = product_without_zero / nums[i];
            }
        }
        // WHY: This block handles the case where the number we are looking at, `nums[i]`, IS zero.
        else {
            // WHY: If there is MORE than one zero in the total array, then the product
            // of the "other" elements will also include a zero, so the result is 0.
            if (zero_count > 1) {
                result[i] = 0;
            } else {
                // WHY: If this is the ONLY zero in the array (`zero_count` must be 1), then the result
                // at this position is the product of all the other (non-zero) numbers.
                result[i] = product_without_zero;
            }
        }
    }
    return result;
}
```

#### **Show a Detailed "Dry Run"**

Let's trace `nums = [1, 2, 0, 4]`.

**Pass 1:**

  - Loop starts.
  - `num = 1`: `product_without_zero` becomes `1 * 1 = 1`.
  - `num = 2`: `product_without_zero` becomes `1 * 2 = 2`.
  - `num = 0`: `zero_count` becomes `1`.
  - `num = 4`: `product_without_zero` becomes `2 * 4 = 8`.
  - Pass 1 ends. We have `zero_count = 1` and `product_without_zero = 8`.

**Pass 2:**

  - `i = 0` (`nums[0]` is 1): `nums[0]` is not 0. `zero_count > 0` is true. `result[0] = 0`.
  - `i = 1` (`nums[1]` is 2): `nums[1]` is not 0. `zero_count > 0` is true. `result[1] = 0`.
  - `i = 2` (`nums[2]` is 0): `nums[2]` is 0. `zero_count > 1` is false. `result[2] = product_without_zero` which is `8`.
  - `i = 3` (`nums[3]` is 4): `nums[3]` is not 0. `zero_count > 0` is true. `result[3] = 0`.
  - Pass 2 ends. `result` is `[0, 0, 8, 0]`. This is the correct answer.

The logic works perfectly.

#### **Analyze Speed and Memory**

  - **Time (Speed):** The code has two separate loops that each run `N` times. The total work is $N + N = 2N$. We drop the constant, making the time complexity **$O(N)$**. This is very fast.
  - **Space (Memory):** We only created a few variables (`zero_count`, `product_without_zero`, `i`) to store single values. This is **$O(1)$** extra space, which is excellent.

#### **The Critical Flaw**

So, the solution is fast and memory-efficient. It passes our dry run. What's wrong with it?

**It breaks the \#1 rule: You must write an algorithm that runs in $O(N)$ time and does not use the division operation.**

The line `result[i] = product_without_zero / nums[i];` makes this entire solution invalid for the problem as stated. In a real interview, providing this solution would show that you missed a critical constraint. It's a great demonstration of an algorithm, but not a solution to *this specific problem*.

We must find a way to get this $O(N)$ speed *without* using division. This brings us back to our Prefix and Suffix product idea.

---

### **Step 4: Thinking Towards a Better Way (The Expert's Thought Process)**

#### **Find the Slow Part**

Let's look back at our simple solution. Where is the slowness coming from?

It's right here:

```cpp
for (int i = 0; i < n; ++i) {
    int product = 1;
    // THIS IS THE SLOW PART
    for (int j = 0; j < n; ++j) {
        if (i != j) {
            product = product * nums[j];
        }
    }
    answer[i] = product;
}
```

The problem is the **inner loop**. For every single number in `nums`, we are looping through the *entire array* all over again. This is a massive amount of repeated work.

For example, when we calculate the result for `nums[0]`, we compute `nums[1] * nums[2] * nums[3] * ...`.
When we calculate the result for `nums[1]`, we compute `nums[0] * nums[2] * nums[3] * ...`.

Notice how `nums[2] * nums[3] * ...` is calculated in both steps? We are doing the same multiplications again and again. An expert sees this repetition and immediately asks: *"How can I avoid recalculating things I've already figured out?"*

#### **Reveal the "Aha\!" Idea**

The key insight comes from breaking the problem down differently. For any given number `nums[i]`, the result we want is:

`(product of all numbers to the left of i)` **multiplied by** `(product of all numbers to the right of i)`

Let's take our example: `nums = [1, 2, 3, 4]`.

To find the answer for `nums[2]` (which is `3`):

  - Numbers to the left are `[1, 2]`. Their product is `1 * 2 = 2`.
  - Numbers to the right are `[4]`. Their product is `4`.
  - The final result is `2 * 4 = 8`.

This seems simple enough. But how do we get these "left" and "right" products for *every* element efficiently?

**The "Aha\!" moment is realizing we can pre-calculate all the left products and all the right products in two separate passes (loops).**

Instead of a loop inside a loop, we can do one loop from left-to-right to get all the "prefix" products. Then, we do a second loop from right-to-left to get all the "suffix" products.

This way, we never repeat our multiplication work.

#### **Draw the New Idea**

Let's visualize this with `nums = [1, 2, 3, 4]`.

**1. The "Left Products" Pass (Prefix Products)**

We'll create a new array, let's call it `left_products`. `left_products[i]` will store the product of all numbers to the *left* of `nums[i]`. For the very first element, there's nothing to its left, so we'll just use `1`.

```
nums =           [ 1,  2,  3,  4 ]

left_products =  [ 1,  ?,  ?,  ? ]  // Start with 1 for the first element.

// For index 1 (value 2): product of left is just nums[0] -> 1
left_products =  [ 1,  1,  ?,  ? ]

// For index 2 (value 3): product of left is nums[0]*nums[1] -> 1*2 = 2
left_products =  [ 1,  1,  2,  ? ]

// For index 3 (value 4): product of left is nums[0]*nums[1]*nums[2] -> 1*2*3 = 6
left_products =  [ 1,  1,  2,  6 ]
```

We just calculated all the prefix products in one simple pass\!

**2. The "Right Products" Pass (Suffix Products)**

Now we do the same thing, but from right to left. Let's call this `right_products`. `right_products[i]` will store the product of all numbers to the *right* of `nums[i]`. For the last element, there's nothing to its right, so we use `1`.

```
nums =           [ 1,  2,  3,  4 ]

right_products = [ ?,  ?,  ?,  1 ]  // Start with 1 for the last element.

// For index 2 (value 3): product of right is just nums[3] -> 4
right_products = [ ?,  ?,  4,  1 ]

// For index 1 (value 2): product of right is nums[3]*nums[2] -> 4*3 = 12
right_products = [ ?, 12,  4,  1 ]

// For index 0 (value 1): product of right is nums[3]*nums[2]*nums[1] -> 4*3*2 = 24
right_products = [24, 12,  4,  1 ]
```

We just calculated all the suffix products in another single pass\!

**3. Combining for the Final Answer**

Now we have everything we need. The final `answer[i]` is simply `left_products[i] * right_products[i]`.

```
left_products  = [ 1,  1,  2,   6 ]
                   * * * *
right_products = [24, 12,  4,   1 ]
                   =   =   =    =
answer         = [24, 12,  8,   6 ]
```

And there's our final answer, `[24, 12, 8, 6]`. We got it by using just two separate loops, not a loop inside another loop. This is much, much faster.

---

### **Step 4: The Smart Solution (The Optimized Code)**

We'll now write the code for the two-pass method. A clever trick here is that we don't even need to create two separate `left_products` and `right_products` arrays. We can do all the work using just the final `answer` array and one extra variable. This saves memory.

#### **Write the Optimized C++ Code**

Here is the much faster, smarter solution.

```cpp
#include <iostream>
#include <vector>

std::vector<int> productExceptSelf_optimized(std::vector<int>& nums) {
    // WHY: Get the total number of elements.
    int n = nums.size();

    // WHY: Create the answer vector. We will use this vector for multiple purposes.
    // First, to store the left products, and then to store the final answer.
    // The problem states the output array does not count as extra space.
    std::vector<int> answer(n);

    // --- Pass 1: Calculate Left Products ---
    // WHY: We initialize the first element of our answer array to 1.
    // There are no elements to the left of the first element.
    answer[0] = 1;

    // WHY: This loop calculates the product of all elements to the LEFT of index `i`.
    // It iterates from the second element to the end.
    // `answer[i]` will store the product of nums[0] * ... * nums[i-1].
    for (int i = 1; i < n; ++i) {
        // WHY: The product to the left of `i` is the product to the left of `i-1`
        // multiplied by the element at `i-1`. We are building upon our previous work.
        answer[i] = answer[i - 1] * nums[i - 1];
    }

    // --- Pass 2: Calculate Right Products and Final Answer ---
    // WHY: Introduce a variable to keep track of the product of elements to the right.
    // We initialize it to 1 because before we start the loop from the end,
    // the "right product" for the last element is 1.
    int right_product = 1;

    // WHY: This loop iterates backwards, from the last element to the first.
    // In this single pass, we will combine the right products with the
    // left products that are already stored in the `answer` array.
    for (int i = n - 1; i >= 0; --i) {
        // WHY: For the current index `i`, `answer[i]` already holds the product of its LEFT side.
        // We now multiply it by `right_product`, which holds the product of its RIGHT side.
        // This gives us the final answer for `answer[i]`.
        answer[i] = answer[i] * right_product;

        // WHY: After using the `right_product` for the current `i`, we must update it
        // for the *next* iteration (which will be `i-1`). We do this by multiplying
        // the current number `nums[i]` into it.
        right_product = right_product * nums[i];
    }

    // WHY: The answer array is now correctly filled.
    return answer;
}
```

#### **Give a Simple Analogy**

This technique is a classic example of using **Prefix and Suffix Calculations**.

Think of it like building a bridge from both sides at once.

1.  **The first pass (left-to-right)** is like one crew starting from the left bank and laying down foundation sections one after another.
2.  **The second pass (right-to-left)** is like a second crew starting from the right bank, doing the same.
3.  The final result for each point on the bridge is a combination of the work done by both crews.

By working from both ends, you get the job done much faster than if one person tried to build the whole thing from scratch for every single spot.

#### **Show the "Dry Run"**

Let's trace `nums = [1, 2, 3, 4]` with this new code.

**Initially:**

  - `n = 4`
  - `answer = [?, ?, ?, ?]`

**Pass 1: Left Products**

1.  `answer[0] = 1`. `answer` is now `[1, ?, ?, ?]`.
2.  `i = 1`: `answer[1] = answer[0] * nums[0]` -\> `1 * 1 = 1`. `answer` is `[1, 1, ?, ?]`.
3.  `i = 2`: `answer[2] = answer[1] * nums[1]` -\> `1 * 2 = 2`. `answer` is `[1, 1, 2, ?]`.
4.  `i = 3`: `answer[3] = answer[2] * nums[2]` -\> `2 * 3 = 6`. `answer` is `[1, 1, 2, 6]`.

<!-- end list -->

  - *First pass is done. `answer` now holds all the prefix (left) products.*

**Pass 2: Right Products & Final Result**

1.  Initialize `right_product = 1`.
2.  `i = 3` (the last element):
      - `answer[3] = answer[3] * right_product` -\> `6 * 1 = 6`.
      - Update `right_product`: `right_product = right_product * nums[3]` -\> `1 * 4 = 4`.
      - `answer` is `[1, 1, 2, 6]`.
3.  `i = 2`:
      - `answer[2] = answer[2] * right_product` -\> `2 * 4 = 8`.
      - Update `right_product`: `right_product = right_product * nums[2]` -\> `4 * 3 = 12`.
      - `answer` is `[1, 1, 8, 6]`.
4.  `i = 1`:
      - `answer[1] = answer[1] * right_product` -\> `1 * 12 = 12`.
      - Update `right_product`: `right_product = right_product * nums[1]` -\> `12 * 2 = 24`.
      - `answer` is `[1, 12, 8, 6]`.
5.  `i = 0`:
      - `answer[0] = answer[0] * right_product` -\> `1 * 24 = 24`.
      - Update `right_product`: `right_product = right_product * nums[0]` -\> `24 * 1 = 24`.
      - `answer` is `[24, 12, 8, 6]`.

<!-- end list -->

  - *Second pass is done. The final answer is correct\!*

#### **Compare Speed and Memory**

  - **Time (Speed):**

      - Brute-Force: $O(N^2)$.
      - **Optimized:** We have one loop that goes from beginning to end ($N$ steps) and a second loop that goes from end to beginning ($N$ steps). The total work is $N + N = 2N$. In complexity analysis, we drop the constant `2`, so the final time complexity is **$O(N)$**. This is a massive improvement. If the list has 1,000,000 items, we do about 2,000,000 operations, not a trillion ($10^6 \\times 10^6$)\!

  - **Space (Memory):**

      - Brute-Force: $O(1)$ extra space.
      - **Optimized:** We used the `answer` array (which doesn't count as extra space) and one extra variable, `right_product`. This is also **$O(1)$** extra space. We successfully made the solution much faster without using more memory.

---

### **Step 5: How to Explain Your Solution (The Interview Plan)**

Imagine an interviewer asks you to solve this problem. Here is a simple, clear template you can follow to explain the optimized `O(N)` solution without division.

Here is your step-by-step explanation guide:

1.  **The Goal:** "The goal of this problem is to create a new array where each element `answer[i]` is the product of all numbers in the original array except for `nums[i]`. A key constraint is that we cannot use division and the solution must be `O(N)` time complexity."

2.  **My Solution Idea:** "A brute-force approach would use nested loops, resulting in slow `O(N^2)` time. A much better approach is to use a **Prefix and Suffix Product** strategy. The core idea is that the answer for any element is simply **(the product of all elements to its left) * (the product of all elements to its right)**."

3.  **How it Works:** "I can calculate these products efficiently in two passes:"
    * "**First Pass (Left to Right):** I'll iterate through the array from left to right. I'll populate my `answer` array such that `answer[i]` holds the product of all numbers to the left of `i`. This is a prefix product."
    * "**Second Pass (Right to Left):** Then, I'll iterate backward from right to left. I'll use a single variable, say `right_product`, to keep track of the product of all numbers to the right. In this pass, I'll update each element `answer[i]` by multiplying it with the current `right_product`. This combines the left and right products to give the final result."

4.  **Performance:** "This solution is very efficient."
    * "**Time Complexity** is **$O(N)$** because I make two separate passes through the array, not nested ones. This is a huge improvement over the $O(N^2)$ brute-force method."
    * "**Space Complexity** is **$O(1)$** for extra space. Although I create an `answer` array, this is the required output. The only extra memory I use is a single variable to store the `right_product` during the second pass."

5.  **Tricky Cases:** "This approach elegantly handles edge cases like zeros and negative numbers without any special `if` statements. If there's a zero in the array, the prefix products after it and the suffix products before it will naturally become zero, correctly zeroing out the results for all other elements."

---

### **Step 6: Key Lessons and Future Problems**

#### **The Main Pattern**

The key pattern here is the **Prefix and Suffix Calculation** (sometimes called a "Two-Pass Algorithm").

This is a powerful technique where you process an array in two passes—one from left-to-right and another from right-to-left—to gather information. The first pass calculates "prefix" data (information about everything to the left), and the second pass calculates "suffix" data (information about everything to the right). You then combine this data to get your final answer.

#### **Clues for Next Time**

How do you know when to use this pattern? Look for these clues in a problem description:

-   The problem asks you to compute a value for each element `i` that depends on the elements **before `i`** and the elements **after `i`**.
-   A brute-force solution involves a nested loop where the inner loop scans to the left or right of the current element.
-   The problem mentions constraints that forbid a slow $O(N^2)$ solution.

If you see these clues, ask yourself: *"Can I calculate the 'left' part and the 'right' part in two separate, linear passes?"*

#### **One More Problem**

A classic problem that uses this exact same pattern is **[42. Trapping Rain Water]**.

In that problem, to find out how much water can be trapped above a specific bar in a histogram, you need to know two things: the height of the tallest bar to its left and the height of the tallest bar to its right. You can find these efficiently by creating a `left_max` array in one pass and a `right_max` array in a second pass. It's a perfect application of the pattern we just learned.
