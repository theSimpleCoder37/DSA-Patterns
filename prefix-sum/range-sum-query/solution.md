> # Implementations are provided below:
![range-sum-query](https://github.com/user-attachments/assets/c95f8d3e-5241-4b61-a0a7-ca17ef29be56)

# Brute-Force Code:
```cpp
class NumArray {
private: 
    vector <int> data; // This will store our original array numbers
public:
    // This is the constructor. It gets called when we create a NumArray object.
    // it takes the 'nums' array and stores it in our 'data' member.
    NumArray(vector<int>& nums) {
        data = nums; // simply copy the input 'nums' into our 'data' vector.  
    }
    
    // This is the function that calculates the sum of given range
    int sumRange(int left, int right) {
        int currentSum = 0; // Initialize a variable to keep track of the sum

        // Loop from 'left' to 'right' (inclusize)
        // For each number between this, add it to our sum
        for (int i = left; i <= right; i++) {
            currentSum += data[i];
        }
        return currentSum;
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * int param_1 = obj->sumRange(left,right);
 */
```

# Optimized Code: 
```cpp
class NumArray {
private:
    vector <int> prefixSum; // This will store our prefix sum
public:
    // Constructor: This is where we calculate the prefix sums.
    NumArray(vector<int>& nums) {
        // if the input 'nums' is empty, there is nothing to do
        if (nums.empty ()) {
            return ;
        }

        // Resize our prefixSum vector to be same size as nums
        prefixSum.resize (nums.size ());

        // First element of prefixSum is just the first element of nums
        prefixSum[0] = nums[0];

        // Now, loop through the rest of the nums array to build prefixSum
        // For each element, add the current number to the previous prefix sum
        for (int i = 1; i < nums.size (); i++) {
            prefixSum[i] = prefixSum[i - 1] + nums[i];
        }
    }

    // Function to get the sum for a given range
    int sumRange(int left, int right) {
        // Handle the edge case where 'left' is 0
        // If left is 0, the sum from 0 to 'right' is simply prefixSum[right]
        if (left == 0) {
            return prefixSum[right];
        } else {
            // Otherwise, the sum from 'left' to 'right' is:
            // (sum from 0 to 'right') - (sum from 0 to 'left-1')
            return prefixSum[right] - prefixSum[left - 1];
        }
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * int param_1 = obj->sumRange(left,right);
 */
```
