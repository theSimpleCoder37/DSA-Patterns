# Implementations are provided below:

![increasing-triplet-subsequence](https://github.com/user-attachments/assets/9bd9af6d-932b-437b-a78e-712816676569)


# Optimized Code:
```cpp
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        int num1 = INT_MAX; // Setting it to very big number, to compare it with our array elements
        int num2 = INT_MAX; // Setting it to very big number, to compare it with our array elements

        // Pick each element from left to right
        for (int currentNumber : nums) {
            // Case 1: `currentNumber` <= `num1`
            if (currentNumber <= num1) {
                num1 = currentNumber; // Set `currentNumber` to `num1`
            }
            // Case 1: `currentNumber` >= `num1` and `currentNumber` <= `num2`
            else if (currentNumber <= num2) {
                num2 = currentNumber; // Set `currentNumber` to `num2`
            }
            // Case 3: `currentNumber` >= `num1` and `num2`
            else {
                return true; // If `currentNumber` is >= `num1` and `num2`. 
                // We found a bigger number of our triplet
            }
        }
        return false; // If, we do not reach till `Case 3` it means there is no triplet subsequence 
    }
};
```
