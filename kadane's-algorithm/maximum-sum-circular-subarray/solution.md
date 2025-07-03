# Implentations are provided below:
![maximum-sum-circular-subarray](https://github.com/user-attachments/assets/31c6441b-025b-4d6d-978f-6c382fed32f8)

# Brute-Force Code:
```cpp
class Solution {
public:
    int maxSubarraySumCircular(vector<int>& nums) {
        int n      = nums.size ();
        int maxSum = INT_MIN; // We initialize it with INT_MIN so that any subarray sum will be larger and can replace it

        // This outer loop iterates through each possible starting index of a subarray
        for (int i = 0; i < n; i++) {
            // We reuse it in the inner loop to avoid recalculating from scratch
            int currSum = 0;
            
            // This inner loop grows the subarray one element at a time, wrapping around the end using modulo
            for (int j = 0; j < n; j++) {
                // We use modulo to wrap around the circular array
                int index = (i + j) % n; 
                // Add the current element to the running sum
                currSum += nums[index];

                // Update maxSum if the current running sum is greater
                maxSum = max (maxSum, currSum);
            }
        }
        return maxSum;
    }
};
```

# Optimized Code:
```cpp
class Solution {
public:
    int maxSubarraySumCircular(vector<int>& nums) {
        int n = nums.size ();

        // (Case 1) To find the maximum sum of a NORMAL subarray (Kadane's Algorithm)
        // maxSum stores the answer for the non-circular case
        // currentMax tracks the max sum of a subarray ending at the current position
        int maxSum     = nums[0];
        int currentMax = nums[0];
        
        // (Case 2) To find the minimum sum of a NORMAL subarray. We'll use this to
        // calculate the maximum circular sum
        // minSum stores the minimum sum found so far
        // currentMin tracks the min sum of a subarray ending at the current position
        int minSum     = nums[0];
        int currentMin = nums[0];

        // (Case 2) We also need the total sum of all numbers in the array
        int totalSum = nums[0];

        // We loop from the second element (index 1) because we've already used the first
        // element to initialize our variables
        for (int i = 1; i < n; ++i) {
           int num = nums[i];

            // --- Part 1: Kadane's algorithm for the maximum sum ---
            // At each step, we either start a new subarray with the current number,
            // or we add the current number to our existing subarray. We do whichever is larger
            currentMax = max (num, currentMax + num);
            maxSum     = max (maxSum, currentMax);

            // --- Part 2: Kadane's algorithm for the minimum sum ---
            // This is the same logic, but for the minimum. We either start a new
            // subarray or continue the old one, doing whichever is smaller.
            currentMin = min (num, currentMin + num);
            minSum     = min (minSum, currentMin);

            // --- Part 3: Calculate the total sum ---
            totalSum += num;
        }

        // This is our critical edge case check. If maxSum is negative, it means all numbers
        // in the array are negative. In this case, the circular sum (total - min) would be incorrect
        // So, the answer must be the non-circular max sum
        if (maxSum < 0) {
            return maxSum;
        }
        
        // If we pass the check, the answer is the larger of the two cases:
        // 1. The normal max subarray sum (maxSum)
        // 2. The wrapping max subarray sum (totalSum - minSum)
        return max (maxSum, totalSum - minSum);
    }
};
```
