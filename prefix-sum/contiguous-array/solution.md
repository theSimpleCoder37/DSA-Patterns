> # Implementations are provided below:
![contiguous-array (1)](https://github.com/user-attachments/assets/97b8ea3e-92a4-4c9b-b5d6-d01dfc0d85f7)

# Brute-Force Code:
```cpp
class Solution {
public:
    int findMaxLength(vector<int>& nums) {
        // We need a variable to keep track of the longest balanced length we find
        int maxLen = 0;

        // It goes from the beginning of the list to the end
        for (int i = 0; i < nums.size (); i++) {
        
            // For each new starting point 'i', we reset our counts of zeros and ones
            int zeroes = 0;
            int ones = 0;

            // It starts where 'i' is and goes to the end of the list
            // This creates every possible subarray that starts at 'i'
            for (int j = i; j < nums.size (); j++) {
                
                // we update the counts based on the new number we're including
                if (nums[j] == 0) {
                    zeroes++;
                } else {
                    ones++;
                }

                // If the counts are equal,
                // we've found a "balanced" subarray
                if (zeroes == ones) {
                    
                    // The length of a subarray from i to j is (j - i + 1)
                    // We then update maxLen to be the larger of what it was before, or our new length
                    maxLen = max (maxLen, j - i + 1);
                }
            }
        }
        return maxLen;
    }
};
```
# Optimized Code:
```cpp
class Solution {
public:
    int findMaxLength(vector<int>& nums) {
        
        // The map stores the first time we see a particular count
        // The key is the count value, and the value is the index where we first saw it
        unordered_map <int, int> countMap;

        // We "pretend" we saw a count of 0 at index -1
        // For example, in [0, 1], the count becomes -1, then 0. The length is 1 - (-1) = 2
        countMap[0] = -1;

        int maxLen = 0; // To store the longest length found so far
        int count = 0; // This is our running sum

        for (int i = 0; i < nums.size (); i++) {

            // If the number is 0, we subtract 1 from our running count
            // If it's 1, we add 1
            count += (nums[i] == 1 ? 1 : -1);

            // We check if our current 'count' has ever been seen before by looking in the map
            if (countMap.count (count)) {
                //YES, we have seen this count before! This means the subarray
                // between the previous index and the current index 'i' has a sum of 0
                // We calculate its length and check if it's the new maximum
                maxLen = max (maxLen, i - countMap[count]);
            } else {
                // NO, this is the first time we've seen this 'count'
                // We must record it in our map along with the current index 'i'
                countMap[count] = i;
            }
        }

        return maxLen;
    }
};
```
