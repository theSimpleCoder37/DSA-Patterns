> # Implementations are provided below:

![trapping-rain-water (1)](https://github.com/user-attachments/assets/815a2bf0-6b73-4dc4-aadc-10fba4ebfc83)


# Brute-Force Code:
```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size ();

        // We want atleast two walls for holding the water between 
        if (n <= 2) {
            return 0;
        }

        int totalTrappedWater = 0; // This will store our final answer

        // We will iterate from the second building upto the second last building
        // Why? cause the buildings at the very ends can't trap the water
        for (int i = 1; i < n - 1; i++) {
            int maxLeft = 0; // Intialize the maximum height to left of current building
            int maxRight = 0; // Intialize the maximum height to right of current building

            // find maximum height to the left of current building
            // We will go up from start to the building just before our current building
            for (int j = 0; j < i; j++) {
                maxLeft = max (maxLeft, height[j]);
            }

            // find maximum height to the right of current building
            // We will go up from just after our current building till the last building
            for (int j = i + 1; j < n; j++) {
                maxRight = max (maxRight, height[j]);
            }

            // Calculate trapped water at the current position
            // The water level at 'i' is limited by the shorter of the two surrounding walls - maxLeft, maxRight
            int waterLevel = min (maxLeft, maxRight);

            // If the water level is higher than the current building, then water is trapped.
            if (waterLevel > height[i]) {
                totalTrappedWater += (waterLevel - height[i]);
            }
        }
        return totalTrappedWater;
    }
};
```
# Better Code:
```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size ();

        if (n <= 2) {
            return 0; // 0, 1, or 2 buildings cannot trap water
        }
        
        // Create two new vectors (arrays) to store our pre-calculated max heights
        vector <int> leftMax (n);
        vector <int> rightMax (n);

        // We start from the beginning.
        // leftMax[i] will store the maximum height encountered from index 0 up to index i
        leftMax[0] = height[0];
        for (int i = 1; i < n; i++) {
            // For each 'i', the max on its left is either the building at 'i'
            // or the max we've seen so far (leftMax[i-1]).
            leftMax[i] = max (leftMax[i - 1], height[i]);
        }

        // We start from the end and move backwards.
        // rightMax[i] will store the maximum height encountered from index n-1 down to index i.
        rightMax[n - 1] = height[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            // For each 'i', the max on its right is either the building at 'i' 
            // or the max we've seen so far (rightMax[i+1]).
            rightMax[i] = max (rightMax[i + 1], height[i]);
        }


        // Calculate total trapped water using pre-calculated arrays
        int totalTrappedWater = 0;

        // We still iterate from index 1 to n-2, as ends don't trap water.
        for (int i = 1; i < n - 1; i++) {
            // The actual water level at 'i' is determined by the shorter of this two walls
            int waterLevel = min (leftMax[i], rightMax[i]);

            // If this current water level is higher than the current building's height,
            // then water is trapped above this building.
            if (waterLevel > height[i]) {
                totalTrappedWater += (waterLevel - height[i]);
            }
        }
        return totalTrappedWater;
    }
};
```

# Optimized Code:
```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size ();

        if (n <= 2) {
            return 0; // 0, 1, or 2 buildings cannot trap water
        }
        
        int left = 0; // Pointer starting from the left end
        int right = n - 1; // Pointer starting from the right end

        int leftMax = 0; // Maximum height encountered from the left so far
        int rightMax = 0; // Maximum height encountered from the right so far

        int totalTrappedWater = 0; // This will store our total trapped water

        // Keep moving pointers towards each other until they meet or cross
        while (left < right) {
            // If the left wall is shorter, 
            // it limits the water level for the current 'left' position.
            if (height[left] < height[right]) {
                // If current building height is greater than or equal to leftMax, 
                // it becomes the new leftMax wall.
                if (height[left] >= leftMax) {
                    leftMax = height[left];
                }
                else {
                    // Otherwise, water can be trapped. The water level is leftMax,
                    // and we subtract current building height.
                    totalTrappedWater += (leftMax - height[left]);
                }
                left++; // Move the left pointer to the right
            }
            /*-------------------------------------------------------------------*/
             // The right wall is shorter or equal, 
             // so it limits the water level for the current 'right' position.
            else {
                
                // If current building height is greater than or equal to rightMax,
                // it becomes the new rightMax wall.
                if (height[right] >= rightMax) {
                    rightMax = height[right];
                } else {
                    // Otherwise, water can be trapped. The water level is rightMax,
                    // and we subtract current building height.
                    totalTrappedWater += (rightMax - height[right]);
                }
                right--; // Move the right pointer to the left
            }
        }
        return totalTrappedWater;
    }
};
```
