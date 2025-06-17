# Implementations are provided below:
![3sum](https://github.com/user-attachments/assets/02c36504-bea6-460b-9926-b91355ee5fe9)

# Brute-Force Code:
```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        // This will store our unique triplets
        // We use a set of vectors to automatically handle duplicates.
        set <vector <int>> uniqueTriplets;

        int n = nums.size (); // Get the total number of elements in our array

        // Edge case handle:
        // If there is less than 3 numbers, we can't form a triplet
        if (n < 3) {
            return {}; // return empty array
        }

        // Loop for the first number [i]
        // It will goes upto n - 2, because we need atleast two numbers afer i
        for (int i = 0; i < n - 2; i++) {
            // Loop for the second number [j]
            // j starts from 'i + 1' to ensure we pick different number than nums[i]
            // and to avoid duplicate combinations
            for (int j = i + 1; j < n - 1; j++) {
                // Loop for the third number [k]
                // k starts from 'j + 1' to ensure its different from nums[j]
                for (int k = j + 1; k < n; k++) {
                    // Check if the sum of the three numbers is zero
                    if (nums[i] + nums[j] + nums[k] == 0) {
                        // found a triplet
                        vector <int> currentTriplet = {nums[i], nums[j], nums[k]};
                        // sort the triplet to ensure uniqueness in the set
                        sort (currentTriplet.begin (), currentTriplet.end ());
                        // Insert the sorted triplet into our set.
                        // The set will automatically ignore duplicates.
                        uniqueTriplets.insert (currentTriplet);
                    }
                }
            }
        } 
        
        // Convert the unique triplets back into vector of vectors
        // because the problem asked for it
        vector <vector <int>> result (uniqueTriplets.begin (), uniqueTriplets.end ());
        return result;

    }
};
```

# Optimized Code:
```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector <vector <int>> result; // This will store all our unique triplets

        // Sort the entire array.
        // Sorting allows us to use the two-pointer technique efficiently
        // and makes it easy to skip duplicate numbers.
        sort (nums.begin (), nums.end ());

        int n = nums.size ();

        // Edge case handle: if there are less than 3 numbers, we can't form a triplet.
        if (n < 3) {
            return result; // return empty result vector
        }

        // Loop for the first number (our 'a' in a + b + c = 0)
        // We will go up to n - 2 because we need at least two more elements (for 'b' & 'c')
        for (int i = 0; i < n - 2; i++) {
            // Skip duplicate values for 'i'.
            // If nums[i] is the same as nums[i-1], we would find the exact same
            // triplets we already found in the previous iteration. Skipping prevents
            // duplicate triplets in our result without needing a set.

            if (i > 0 && nums[i] == nums[i - 1]) {
                continue; // Move to the next 'i'
            }


            int left = i + 1; // our 'b'
            int right = n - 1; // our 'c'
            int target = -nums[i]; // our '-a'

            while (left < right) {
                int currentSum = nums[left] + nums[right];

                if (currentSum == target) {
                    // Found a triplet!
                    result.push_back ({nums[i], nums[left], nums[right]});

                    // Skip duplicate values for 'left' and 'right'
                    // by moving past all identical values, we ensure uniqueness.
                    while (left < right && nums[left] == nums[left + 1]) {
                        left++;
                    }   

                    while (left < right && nums[right] == nums[right - 1]) {
                        right--;
                    }

                    // Move both pointers inwards to find other possible triplets.
                    left++;
                    right--;
                } else if (currentSum < target ) {
                    // CurrentSum is too small, We need a larger sum
                    left++;
                } else {
                    // CurrentSum is too big. We need a smaller sum.
                    right--;
                }
            }
        }
        return result;
    }
};
```
