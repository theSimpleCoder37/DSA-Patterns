# Implementations are provided below:
![alt](https://github.com/user-attachments/assets/a60dc243-0b45-4881-8dea-41232bdd2fd4)

# Brute-Force Code:
```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();

        vector<int> answer(n); // This will store our, products of each `i` except the element at `i`

        // We will pick each number from left -> right
        for (int i = 0; i < n; i++) {
            // for each `i` to calculate new product
            int product = 1;

            // This will go from start to end again
            for (int j = 0; j < n; j++) {
                // if `i` and `j` index are not same
                // We can calculate the product of other elements
                if (i != j) {
                    product *= nums[j];
                }
            }
            // put the `product` in the `answer` array
            answer[i] = product;
        }
        
        return answer; // Done!
    }
};
```

# Better Code:
```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();

        int zeroCount = 0; // It will count how many zeros are there in array
        int productWithoutZero = 1; // It will store, product of all elements except zero(s)

        // Pass 1:
        // Count the zeros
        // Calculate the non-zero elements product
        for (int num : nums) {
            // if `num` is zero
            if (num == 0) {
                zeroCount++; // Increment the count of `zeroCount`
            } else {
                productWithoutZero *= num; // Multiply it with with non-zero elements
            }
        }

        // Create a result array to store the answers
        vector<int> result(n);

        // Pass 2:
        for (int i = 0; i < n; i++) {
            // If this current element is not zero
            if (nums[i] != 0) {
                // Check, is there any zero present somewhere?
                if (zeroCount > 0) {
                    result[i] = 0; // result will be directly zero, cause anything multiply by zero is zero
                } else {
                    result[i] = productWithoutZero / nums[i]; // Cancle out the `nums[i]` from `productWithoutZero`
                }
            } else { // If this current element is zero
                // Check, is there also another zero present somewhere?
                if (zeroCount > 1) {
                    result[i] = 0; // result will be directly zero
                } else {
                    result[i] = productWithoutZero; // product of all other elements
                }
            }
        }
        return result; // Done
    }
};
```

# Optimized Code:
```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> answer(n); // This will store answers

        answer[0] = 1; // because the first element do not have anyone before it

        // Pass 1: Left products
        // We will calculate the prefix product at each `i`
        for (int i = 1; i < n; i++) {
            answer[i] = answer[i - 1] * nums[i - 1]; 
        }

        // Pass 2: Right products & Final answer
        int rightProduct = 1; // because the last element do not have anyone after it
        // We will calculate the suffix product at each `i`
        for (int i = n - 1; i >= 0; i--) {
            answer[i] = answer[i] * rightProduct; // Multiply `rightProduct` with stored `answer[i]`

            // Update the `rightProduct` for next itration
            rightProduct = rightProduct * nums[i];
        }

        return answer; // Done
    }
};
```
