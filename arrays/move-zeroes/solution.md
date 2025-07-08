> # Implementations are provided below:
![move-zeroes](https://github.com/user-attachments/assets/a9f9400d-e2c2-4ea6-814e-fba30700706a)

# Brute-Force Code:
```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int n = nums.size();

        // Iterate to left to right
        for (int i = 0; i < n; i++) {

            // If the number at `i` is zero, so we need a non-zero
            // number to swap this zero with non-zero
            if (nums[i] == 0) {
                // We find the non-zero element from after the `i` till the end
                // And swap it
                for (int j = i + 1; j < n; j++) {
                    // If this `j` is not zero, then we can swap it
                    if (nums[j] != 0) {
                        swap (nums[i], nums[j]);
                        break; // Break the loop for next position
                    }
                }
            }
        }
    }
};
```
# Optimized Code:
```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int n = nums.size ();

        // This is correct spots of the non-zero elements
        // It will start from index 0
        int writePointer = 0;

        // We will iterate in the array using `readPointer`
        for (int readPointer = 0; readPointer < n; readPointer++) {
            // If the `readPointer` element is non-zero
            if (nums[readPointer] != 0) {
                // We swap this element with our `writePointer` spot
                swap (nums[writePointer], nums[readPointer]);
                // And move `writePointer` one step ahead
                writePointer++;
            }
        }
    }
};
```
