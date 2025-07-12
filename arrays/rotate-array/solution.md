> # Implementations are provided below:
<img width="4053" height="8768" alt="rotate-array excalidraw" src="https://github.com/user-attachments/assets/ec210790-005f-4b18-af7d-7f6bfc983bbe" />

# Brute-Force Code:
```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n = nums.size();

        // Edge case handle: If the `k` is bigger than `n`.
        // E.g: `nums` has 7 elements and `k` is 10, the 10th rotation for this is same as 3rd rotation
        // Therefore, we can use modulo: 7 % 10 = 3
        if (n > 0) { // Avoiding dividing by 0
            k = k % n;
        }

        // We do our process for `k` steps
        for (int i = 0; i < k; i++) {
            // Step 1: Take the last element
            int lastElement = nums.back();

            // Step 2: Remove the last element
            nums.pop_back();

            // Step 3: Insert the last element to the first
            nums.insert (nums.begin(), lastElement);
        }
    }
};
```

# Optimized Code:
```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n = nums.size();

        // Egde case: If the contains elements 1 or 0
        // There is nothing to rotate
        if (n <= 1) {
            return ;
        }

        // Edge case: If `k` is larger than `n`
        k = k % n; // we avoid unnecessary work

        // Step 1: reverse the whole array
        reverse(nums.begin(), nums.end());

        // Step 2: revere the first `k` elements
        reverse(nums.begin(), nums.begin() + k);

        // Step 3: reverse the rest part now
        reverse(nums.begin() + k, nums.end());
    }
};
```
