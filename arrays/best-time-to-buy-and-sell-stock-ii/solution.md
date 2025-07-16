# Implementations are provided below:
![alt](https://github.com/user-attachments/assets/7ddb6e1c-cd14-410d-bc3a-ff0b59dd5420)

# Simple And Optimized Code:
```cpp
class Solution {
public:
    int maxProfit(vector<int> &prices) {
        int n = prices.size();

        int totalProfit = 0; // this will store our maximum total profit

        // We will start looking from the second element. Why?
        // we need a element before, to compare with it
        for (int i = 1; i < n; i++) {
            // If this current price, is larger than the previous price
            // We gain a profit then
            if (prices[i] > prices[i - 1]) {
                // Add this profit to our `totalProfit`
                totalProfit += prices[i] - prices[i - 1]; // totalProfit = Selling Price - Buying Price
            }
            // else, Do nothing
        }

        return totalProfit;
    }
};
```
