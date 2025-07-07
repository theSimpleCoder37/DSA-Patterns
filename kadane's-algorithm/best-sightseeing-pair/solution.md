# Implementations are provided below:
![best-sightseeing-pair](https://github.com/user-attachments/assets/4f5d313b-80eb-4688-8b98-5971a38cced3)

# Brute-Force Code:
```cpp
class Solution {
public:
    int maxScoreSightseeingPair(vector<int>& values) {
        int n = values.size();
        int maxPair = 0; // It will store our maximum score so far

        // It will start from the first value till second last
        // Why? we need a value after `i` for making a pair
        for (int i = 0; i < n - 1; i++) {
            int currentPair = 0; // It will store our current score of each pair

            // It will start from after `i` till end of the array
            for (int j = i + 1; j < n; j++) {
                // Calculate the pair using the formula
                currentPair = values[i] + values[j] + i - j;

                // Whichever is the maximum, take it
                maxPair = max(maxPair, currentPair);
            }
        }

        return maxPair;
    }
};
```

# Optimized Code:
```cpp
class Solution {
public:
    int maxScoreSightseeingPair(vector<int>& values) {
        int n = values.size();
        
        // Ye har `j` ke pehle jo bhi maximum Part A (values[i] + i) ko store karenga
        // We intialize it to first value of our values array
        int prevMax = values[0] + 0;

        int maxPair = 0; // Ye sab se badi pair value ko store karenga

        // Ek loop chalayenge j = 1, se end tak
        for (int j = 1; j < n; j++) {
            // har `j` ka current pair store karenga
            // isme ham Part A + Part B ko store karenge
            int currentPair = prevMax + (values[j] - j);

            // Update maxPair, whichever is big
            maxPair = max(maxPair, currentPair);

            // Aage jo `j` aayenga uske best Part A ko find karenga
            prevMax = max(prevMax, values[j] + j);
        }

        return maxPair;
    }
};
```
