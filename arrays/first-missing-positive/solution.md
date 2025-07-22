# Implementations are provided below:
![first-missing-positive](https://github.com/user-attachments/assets/ac1e62e1-1db6-43b9-898c-55d933b39316)

# Approach 1 [O(n) space] Code:
```cpp
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        // We are creating a set to store only the positive numbers,
        // This will help us to find first missing positive
        unordered_set<int> seen;

        // Step 1: Add only the positive numbers in the set
        for (int num : nums) {
            // If it is positive, then insert it
            if (num > 0) {
                seen.insert(num); // Store it
            }
        }

        // Step 2: Check for the first missing positive
        int i = 1; // Start checking from 1, 2, 3, ... until we not found one which is not present
        while (true) {
            // If `i` is not present in the `set`
            if (seen.find(i) == seen.end()) {
                return i; // return it
            }
            i++; // Move to the next number to check
        }
    }
};
```

# Approach 2 [O(1) space] Code:
```cpp
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int n = nums.size();

        // Step 1: Replace irrelevant numbers (zeros, negatives, > n)
        for (int i = 0; i < n; i++) {
            // If the nums[i] is a zero, negative, or > n
            // Not in our range: [1, n]
            if (nums[i] <= 0 || nums[i] > n) {
                nums[i] = n + 1; // Replace it with `n + 1`
            }
        }

        // Step 2: Mark the numbers which are present
        for (int i = 0; i < n; i++) {
            int num = abs(nums[i]); // Why abs(), if the number is already negative marked previously
            int index = num - 1; // Corresponding index of `num`

            // Only take those numbers which is in our range: [1, n]
            if (num <= n) {
                // mark the number at `index` negative, to indicate it is present 
                nums[index] = -abs(nums[index]);
            }
        }

        // Step 3: Find the first postive number
        for (int i = 0; i < n; i++) {
            // If the nums[i] is positive, it is missing
            if (nums[i] > 0) {
                return i + 1; // return the missing number
            }
        }

        // if all the numbers between the range: [1, n] are present
        // Return the number after n
        return n + 1;
    }
};
```
