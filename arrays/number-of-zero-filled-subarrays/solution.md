# Implementations are provided below:
![number-of-zero-filled-subarrays](https://github.com/user-attachments/assets/c066e5d1-a75c-4690-8562-00e6654dcc2c)

# Brute-Force Code:
```cpp
class Solution {
public:
    long long zeroFilledSubarray(vector<int>& nums) {
        int n = nums.size();

        long long count = 0; // This will count all the possible subarrays of zeroes

        // Pick a starting point of the subarray
        for (int i = 0; i < n; i++) {
            // Pick the ending point of the subarray
            for (int j = i; j < n; j++) {
                // If current element is equal -> 0
                // Increment the count
                if (nums[j] == 0) {
                    count++; // Found a subarray
                } else {
                    break; // If, not found, break the loop, move to next starting point
                }
            }
        }
        return count; // Done
    }
};
```

# Optimized Code:
```cpp
class Solution {
public:
    long long zeroFilledSubarray(vector<int>& nums) {
        int n = nums.size();

        int counter = 0; // If we see `0` we increment it, Else we see `non-zero` we set it to `0`
        long long total = 0; // This will store the total count of subarrays

        // We start from left -> right
        for (int i = 0; i < n; i++) {
            // If we see a `zero`
            if (nums[i] == 0) {
                counter++; // Increment the `counter`
            } else {
                counter = 0; // If we see a `non-zero` we set it to `0` 
            }

            // At last, add the counter value to out `total`
            total += counter;
        }

        return total; // Done!
    }
};
```
