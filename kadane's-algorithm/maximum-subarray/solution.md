# Implementations are provided below:
![maximum-subarray](https://github.com/user-attachments/assets/28656d45-5279-4830-bf58-64cb2f5923ac)

# Brute-Force Code:
```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        // We need a variable to store the maximum sum found so far
        // We start it at the smallest possible integer value so that any real sum
        // from the array will be larger than it
        int maxSum = INT_MIN;

        // 'i' will go from the first element to the last element
        for (int i = 0; i < nums.size (); i++) {
            // Reset the sum for each new subarray
            int currentSum = 0;
            // 'i' will go from the first element to the last element.
            for (int j = i; j < nums.size (); j++) {
                currentSum += nums[j]; // Our running sum
                // If the current one is bigger, we update maxSum
                maxSum = max (maxSum, currentSum);
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
    int maxSubArray(vector<int>& nums) {
        // Initialize 'maxSum' to the smallest possible integer,
        // so any number in the array will be larger and update maxSum correctly
        int maxSum = INT_MIN;

        // 'currentSum' holds the sum of the current subarray we're exploring
        // It starts at zero because we haven't added any numbers yet
        int currentSum = 0;

        // Iterate over each number once
        for (int num : nums) {
            // Add the current number to 'currentSum' extend the current subarray
            currentSum += num;
            // Update 'maxSum' if the current sum is better than what we had before
            maxSum = max (maxSum, currentSum);

            // If the current sum dips below zero, reset it to zero.
            // This means the current subarray is hurting the total sum,
            // so start fresh from the next element.
            if (currentSum < 0) {
                currentSum = 0;
            }
        }
        return maxSum;
    }
};
```
