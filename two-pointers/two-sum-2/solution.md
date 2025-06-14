> # Implementations are provided below
![Untitled-2025-02-14-1516](https://github.com/user-attachments/assets/afd64446-1754-4a69-8bcb-b7499afe54a3)


# Brute-Force Code:
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        // Outer loop: this pick the first number.
        // this 'i' goes from first to second-last number
        // Why? second last number -> we need atleast one number after it for sum
        for (int i = 0; i < numbers.size () - 1; i++) {
            // Inner loop: this pick the second number
            // this 'j' starts from 'i + 1' for not checking the same pair again
            for (int j = i + 1; j < numbers.size (); j++) {
                // Check if sum of the two numbers equals to target
                if (numbers[i] + numbers[j] == target) {
                    // if they do, return there positions with + 1
                    return {i + 1, j + 1};
                }
            }
        }

        return {}; // return empty array if no solution found (just for compiler)
    }
};
```
# Optimized Code:
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int n = numbers.size ();

        int left = 0; // left starts at the beginning of the array
        int right = n - 1; // right starts at the end

        // the loop continues as long as left is less than right
        while (left < right) {
            // Calculate the sum of numbers pointed by 'left' & 'right'
            int currentSum = numbers[left] + numbers[right];

            // Case 1: currentSum is equal to target
            if (currentSum == target) {
                // Found! return there 1-based indices
                return {left + 1, right + 1};
            } 
            // Case 2: currentSum is less than target
            else if (currentSum < target) {
                left++; // Move forward! to get larger sum
            }
            // Case 3: currentSum is greater than target
            else {
                right--; // Move backword! to get smaller sum
            }
        }
        return {}; // return empty array if no solution found (just for compiler)
    }
};
```
