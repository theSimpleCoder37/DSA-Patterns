> # Implementations are provided below:
![majority-element](https://github.com/user-attachments/assets/b4f368d1-85e4-4c53-bc81-dd297755085a)

# Brute-force Code:
```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int n = nums.size ();

        int majorityCount = n / 2; // Is any number greater than this, that's our answer

        // This picks one number at a time
        for (int i = 0; i < n; i++) {
            int count = 0; // Store the frequency of the number picked

            // This will count how many the times the picked number comes
            for (int j = i; j < n; j++) {
                // If we see the picked number in the array
                if (nums[i] == nums[j]) { 
                    count++; // Increament the count
                }

                // Check, it is greater or not, that `majorityCount`
                if (count > majorityCount) {
                    return nums[i]; // If it is, then return our picked number
                }
            }
        }
        return -1; // this is not required, but for compiler
    }
};
```
# Better Code:
```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int n = nums.size();

        // This map will store frequencies of the numbers we encounter
        // Key -> number, Value -> frequency
        unordered_map<int, int> countsMap;

        int majorityCount = n / 2; // If frequency of any number greater than this, is our answer

        // We pick each number at a time
        for (int num : nums) {
            
            // It will increase the count of the `num` in map, whenever we encounter it
            // Or if the number is not in the map, it will automatically store the `num` with 0, before increament
            countsMap[num]++;

            // If the `num` is greater than `majorityCount`
            if (countsMap[num] > majorityCount) {
                return num; // Then return the `num`
            }
        }
        return -1; // This is not required, but it is for compiler only
    }
};
```
# Optimized Code:
```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        // Our candidate is not fixed yet, so the count is 0
        int candidate = 0;
        int count = 0;

        // We will pick each number at a time
        for (int num : nums) {
            
            // If the `count` is 0, so we set our `candidate`
            // to current `num` as our new `candidate`
            if (count == 0) {
                candidate = num;
            }

            // If we see a num equals to candidate, increment the count
            // or if we any different num, decrement the count
            if (candidate == num) {
                count++;
            } else {
                count--;
            }
        }
        // The algorithm will give the answer easily, cause if the majority element exist
        // It will cancel out all the other numbers, and left candidate will be our answer
        return candidate;
    }
};
```
