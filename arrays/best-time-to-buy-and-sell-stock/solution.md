# Implementations are provided below:
![best-time-to-buy-and-sell-stock](https://github.com/user-attachments/assets/bd63cc15-9afd-4a68-aa55-cf384416f9ee)

# Brute-Force Code:
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();

        int maxProfit = 0; // It will store our maximum profit so far

        // `i`: is our buying day, it will start from go from left to right
        for (int i = 0; i < n - 1; i++) {
            // `j`: is our selling day, it will use to calcuate the profit, it will start right after the `i`
            int profit = 0; // It will calculate the profit for each pair
            for (int j = i + 1; j < n; j++) {
                // profit = selling price - buying price
                profit = prices[j] - prices[i];

                maxProfit = max(maxProfit, profit); // Take, which is big 
            }
        }
        return maxProfit;
    }
};
```

# Optimized Code:
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int maxProfit = 0; // It will store our maximum profit so far
        int minPrice = INT_MAX; // It will store the minimum price till `currentPrice`

        // Pick each price only once
        for (int currentPrice : prices) {
            // Is `currentPrice` is less than the `minPrice`
            if (currentPrice < minPrice) {
                minPrice = currentPrice; // Update `minPrice`
            } 
            // Is `currentPrice - minPrice` > `maxProfit`
            else if (currentPrice - minPrice > maxProfit) {
                maxProfit = currentPrice - minPrice; // Update `maxProfit`
            }
        }

        return maxProfit;
    }
    
};
```
