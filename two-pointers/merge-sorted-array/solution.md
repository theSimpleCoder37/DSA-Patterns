> # Implementation are provided below:
![Untitled-2025-02-14-1516 (1)](https://github.com/user-attachments/assets/51c78e3f-c316-402c-b624-b1a278b3742d)

# Brute-force Code:
```cpp []
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        // Copy elements from nums2 into the empty spots of nums1
        for (int i = 0; i < n; i++) {
            nums1[m + i] = nums2[i];
        }

        // Sort the nums1
        sort (nums1.begin (), nums1.end ());
    }
};
```

# Optimized Code:
```cpp []
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int p1 = m - 1; // Pointing to last real element in nums1
        int p2 = n - 1; // Pointing to last element in nums2
        int p = (m + n) - 1; // Pointing to last empty spot in nums1

        // We stop when p2 becomes less than '0'
        while (p2 >= 0) {
            // Check first, p1 is valid or not (there are still real numbers in nums1 to compare)
            // And, if - nums1 p1'th element is greater than nums2 p2'th element
            // Then, put nums1 p1'th element at nums1 p'th empty spot
            if (p1 >= 0 && nums1[p1] >= nums2[p2]) {
                nums1[p] = nums1[p1];
                p1--; // Go one step back, cause we used this number
            } else {
                // if - nums1 p1'th element is smaller than nums2 p2'th element
                // Then, put nums2 p2'th element at nums1 p'th empty spot
                nums1[p] = nums2[p2];
                p2--; // Go one step back, cause we used this number
            }
            p--; // In either case, we have filled one spot in nums1, so move one step back
        }

        // No extra steps needed after here, cause nums1 elements are already at corrected spots
    }
};
```
