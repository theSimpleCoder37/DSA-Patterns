# Implementations are provided below:

![continuous-subarray-sum](https://github.com/user-attachments/assets/260170a8-1381-44f7-b0c9-a564360d46e7)

# Brute-Force Code:
```cpp
class Solution {
public:
    bool checkSubarraySum(vector<int>& nums, int k) {
        int n = nums.size (); // Total elements

        // If the array has fewer than 2 elements, we can't form a subarray
        // of size at least two 
        if (n < 2) {
            return false;
        }

        // It goes from the first element (index 0) up to the second-to-last element
        // It stops at n-1 because the subarray needs at least two elements
        for (int i = 0; i < n - 1; i++) {

            // Initialize the sum for the subarray starting at index `i`
            int currentSum = nums[i];

            // It starts at the element right after our starting point `i`
            for (int j = i + 1; j < n; j++) {
                // Add the next element to our running sum to extend the subarray
                currentSum += nums[j]; 

                // Check the currentSum is multiple of `k`or not
                if (currentSum % k == 0) {
                    return true; // If it is, then return true
                }
            }
        }

        return false; // If not, return false
    }
};
```

# Optimized Code: 
```cpp
class Solution {
public:
    bool checkSubarraySum(vector<int>& nums, int k) {
        // We use an unordered_map (a hash map) to store the remainders we've seen
        // so far and the index where they first occurred. The key is the remainder,
        // and the value is the index. This gives us very fast O(1) lookups
        unordered_map <int, int> remainderMap;

        // We initialize the map with a remainder of 0 at index -1. This handles the edge case where the
        // subarray that sums to a multiple of k starts from the very beginning (index 0).
        // For example, if nums=[6, 5] and k=6, at index 0, sum=6, remainder=0.
        // We check length: i - map[0] => 0 - (-1) = 1. This is not >= 2. Correct.
        // If nums=[3, 3], at index 1, sum=6, remainder=0.
        // We check length: i - map[0] => 1 - (-1) = 2. This is >= 2. Correct.
        remainderMap[0] = -1;

        // This variable will keep track of the sum of elements from index 0 up to `i`
        int runningSum = 0;

        // We only need one loop that goes through the array from start to end
        for (int i = 0; i < nums.size (); i++) {
            // Update the running sum with the current number
            runningSum += nums[i];

            // Calculate the remainder of the running sum when divided by k
            int remainder = runningSum % k;

            // We check if we have seen this remainder before
            // map.count() is a fast way to see if a key exists in the map
            if (remainderMap.count (remainder)) {
                // If we've seen this remainder before, we get the previous index
                // We must check if this subarray has at least two elements
                // The length is `i - prevIndex`
                if (i - remainderMap[remainder] >= 2) {
                    // We found a subarray of at least size 2!
                    return true;
                }
            } else {
                // If this is the first time we are seeing this remainder,
                // we store it in our map along with the current index `i`
                remainderMap[remainder] = i;
            }
        }

        // If the loop finishes and we never returned true, it means
        // no such subarray was found
        return false;
    }
};
```
