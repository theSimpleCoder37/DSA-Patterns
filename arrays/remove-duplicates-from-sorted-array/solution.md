# Implementations are provided below:
![png](https://github.com/user-attachments/assets/1f18042d-d835-4e65-b2b2-c9b71394e82d)

# Brute-Force Code:
```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        // We create a `set`, cause it will store only the unique elements
        set<int> uniqueElements;

        // We loop from start to end in our `nums` array
        // And insert each element, (the `set` will ignore the duplicates)
        for (int num : nums) {
            uniqueElements.insert(num);
        }

        // After that, we get the size of the `uniqueElements`
        // It is the number of unique elements
        int k = uniqueElements.size();

        // We copy back the unique elements from beginning in our original array
        int index = 0;
        for (int uniqueNum : uniqueElements) {
            nums[index] = uniqueNum;
            index++;
        }

        return k; // Just return the number of unique elements, as the problem requires
    }
};
```
# Optimized Code:
```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int n = nums.size();

        // If the array is empty, no unique elements
        if (nums.empty()) {
            return 0;
        }

        // This is out `writerPointer` it will show the place, where we have to put the next unique number
        // It will also count the number of unique elements (the answer)
        int k = 1;

        // it will go from (index 1) till end of the array
        for (int i = 1; i < n; i++) {
            // If current number `i` is different from last, unique number we have seen `k - 1`
            if (nums[i] != nums[k - 1]) {
                nums[k] = nums[i]; // If it is different, we will overwrite the last, duplicate element

                k++; // And move the `k` pointer forward, cause that position has a unique number now
            }
            // else if it is a duplicate, we ignore it
        }

        return k;
    }
};
```
