# Implementations are provided below:
![subarray-sums-divisible-by-k (1)](https://github.com/user-attachments/assets/98495790-bd24-44dd-b29b-7ada301cd783)

# Brute-Force Code:
```cpp
class Solution {
public:
    int subarraysDivByK(vector<int>& nums, int k) {
        int n = nums.size ();
        int count = 0; // This variable will store our final answer. We start the count at 0

        // It goes from the beginning (index 0) to the end
        for (int i = 0; i < n; i++) {

            // We reset it for every new starting position
            int currentSum = 0;
            // It starts from our chosen starting index 'i' and goes to the end
            for (int j = i; j < n; j++) {

                // We expand our subarray by one element (at index 'j') and add its value to the sum
                currentSum += nums[j];

                // If the remainder of the sum divided by k is 0, the sum is divisible
                if (currentSum % k == 0) {
                    // If it's divisible, increament the count
                    count++;
                }
            }
        }
        return count;
    }
};
```
# Optimized Code:
```cpp
class Solution {
public:
    int subarraysDivByK(vector<int>& nums, int k) {
        // An array to store the frequency of each remainder. The index of the array
        // (0 to k-1) represents the remainder itself, This is faster than a hash map
        vector <int> remainderCounts (k, 0);

        // We've seen a remainder of 0 one time
        // before we even start the loop (from a conceptual prefix sum of 0)
        // This correctly handles subarrays that start at index 0
        remainderCounts[0] = 1;

        int prefixSum = 0; // Holds the running sum from the beginning of the array
        int count = 0; // This will be our final answer

        // We only loop through the array ONCE
        for (int num : nums) {
            // Add the current number to our running sum
            prefixSum += num;

            // Calculate the remainder
            int remainder = prefixSum % k;

            // This is a crucial fix. In C++, taking the modulo of a negative number
            // can result in a negative remainder (e.g., -2 % 5 = -2). We need a positive
            // remainder (0 to k-1) to use as an index in our array
            // Adding k makes any negative remainder positive (e.g., -2 + 5 = 3)
            if (remainder < 0) {
                remainder += k;
            }

            // The value at remainderCounts[remainder] tells us how many
            // times we have seen this remainder before
            count += remainderCounts[remainder];

            // We've now seen this remainder one more time or first time (for future use) at the current position
            remainderCounts[remainder]++;
        }    
        return count;
    }
};
```
