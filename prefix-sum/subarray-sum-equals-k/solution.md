# Implementations are provided below:
![subarray-sum-equals-k](https://github.com/user-attachments/assets/4e206d9f-a719-4c56-8e24-69fdd40d7221)

# Brute-Force Code:
```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int count = 0; // This will store our final answer
        int n = nums.size ();

        // This loop selects a starting point 'i' for the subarray 
        for (int i = 0; i < n; i++) {
            int currentSum = 0; // Reset the sum for each new starting point

            // This loop extends the subarray from the starting point 'i' to the end of the array
            for (int j = i; j < n; j++) {
                // Add the current element to the sum of the subarray starting at 'i'
                currentSum += nums[j];

                // Check if the sum of the subarray from 'i' to 'j' equals our target
                if (currentSum == k) {
                    count++; // If it does, increament our counter
                }
            }
        }
        return count; // Return the total count found
    }
};
```

# Optimized Code:
```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        // Use an unordered_map (a hash map) to store the frequency of each prefix sum we encounter
        // Key: The prefix sum. Value: The number of times this prefix sum has occurred.
        unordered_map <int, int> prefixSumCount;

        // Initialize the map with a prefix sum of 0 having occurred 1 time
        // For example, if nums=[3, 4] and k=7, currentSum becomes 7. We need to find (7-7)=0 in the map
        prefixSumCount[0] = 1;

        int count = 0; // Our final result counter
        int currentSum = 0; // The running total (prefix sum) as we iterate through the array

        // We only need a single loop through the numbers
        for (int num : nums) {
            // Add the current number to our running sum
            currentSum += num;

            // This is the core logic. We are looking for a previous prefix sum `P` such that:
            // currentSum - P = k.  Rearranging gives: P = currentSum - k.
            // So, we calculate the `requiredSum` we need to have seen before
            int requiredSum = currentSum - k;

            // Check if this `requiredSum` exists as a key in our map
            // How? The .count() method is a safe way to check for existence in a C++ map
            if (prefixSumCount.count (requiredSum)) {
                // If it exists, it means we have found one or more subarrays that end at the current
                // position with a sum of k. The number of such subarrays is exactly the number of times
                // we have seen this `requiredSum` before. So we add that count to our result.
                count += prefixSumCount[requiredSum];
            }

            // Finally, we update the map with the `currentSum` we just calculated.
            // We increment its count, because we have now seen this sum one more time.
            prefixSumCount[currentSum]++;
        }
        return count;
    }
};
```
