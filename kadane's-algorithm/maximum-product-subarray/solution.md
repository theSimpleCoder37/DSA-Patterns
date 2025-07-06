# Implementations are provided below:
![maximum-product-subarray](https://github.com/user-attachments/assets/91b9b44c-d475-4f1e-bfe1-5de9fdee24ae)

# Brute-Force Code:
```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int n = nums.size ();

        // We need a variable to store the maximum product found so far
        // We initialize it with the first number, as a single number is a valid subarray
        int maxProductFound = nums[0];

        // This first loop selects the starting point of our subarray
        for (int i = 0; i < n; i++) {

            // This variable will store the product of the current subarray that starts at 'i'
            // We must reset it to 1 for each new starting point
            int currentProduct = 1;
            
            // It moves the 'end' of the subarray from 'i' to the end of the array
            for (int j = i; j < n; j++) {
                // We multiply the numbers in the current subarray [i...j]
                currentProduct *= nums[j];

                // If the product of the current subarray is the biggest we've seen,
                // we update our answer
                maxProductFound = max (maxProductFound, currentProduct);
            }
        }
        return maxProductFound;
    }
};
```

# Optimized Code:
```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int n = nums.size();

        int result     = nums[0]; // It will store our maximum product so far
        int productMax = nums[0]; // This will be the maximum product till `i`
        int productMin = nums[0]; // This will be the minimum product till `i`

        // We iterate through complete array
        for (int i = 1; i < n; i++) {
            int tempMax = productMax; // Cause, actual productMax value will be needed to find productMin

            // 1. Start with this new subarray
            // 2. Extend this subarray by, using max product
            // 3. Extend this subarray by, using min product (-ve * -ve = big +ve)
            productMax = max({nums[i], nums[i] * tempMax, nums[i] * productMin});
            // same for `productMin`
            productMin = min({nums[i], nums[i] * tempMax, nums[i] * productMin});

            result = max(result, productMax); // Whoever is bigger, take it
        }

        return result;
    }
};
```
