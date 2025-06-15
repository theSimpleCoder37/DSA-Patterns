> # Implementations are provided below
![leetcode11](https://github.com/user-attachments/assets/e1a9225d-912e-456b-854b-076c6a777634)

# Brute-Force Code:
```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int maxArea = 0; // This will store our maximum area, intialized to 0, cause we have not find yet.
        int n = height.size (); // Number of lines (heights)

        // Outer loop: It will goes from first line to second-last line
        // Why? Because we need one line after it for making a pair
        for (int i = 0; i < n - 1; i++) {
            // Inner loop: It will start from second line till last line
            for (int j = i + 1; j < n; j++) {
                // Calculate the height of the water
                // The height of the water level is the shorter line of both the lines
                int waterLevel = min (height[i], height[j]);

                // Calculate the width
                // the width is the distance between the two lines
                int width = j - i;

                // Calculate the area between these two lines
                int area = waterLevel * width;

                // If current area is larger, update maxArea
                maxArea = max(maxArea, area);
            }
        } 
        
        return maxArea;
    }
};
```

# Optimized Code:
```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int n = height.size ();
        int left = 0; // it starts from the beginning 
        int right = n - 1; // it starts from at the end

        int maxArea = 0; // It will store our maximum area, intialized to 0, cause we didn't find yet

        // Move until both pointer not crosses each other
        while (left < right) {
            // Calculate the height of the water for the current container
            // the water level will upto shorter line always 
            int waterLevel = min (height[left], height[right]);

            // Calculate the width of the current container
            // it is the distance between the two lines
            int width = right - left;

            // Calculate the area of the current container
            int area = waterLevel * width;

            // If current area is larger, update the maxArea
            maxArea = max (maxArea, area);

            // Move the pointer at the shorter line to try and find a taller one, 
            // since this might increase the height and give a bigger area. 
            // Moving the taller line won't help and only reduces the width.
            if (height[left] < height[right])  {
                left++; // if left line is shorter, try to find taller line, by moving it forward
            } else {
                right--; // if right line is shorter, try to find taller line, by moving it backward
            }
        }
        return maxArea;
    }
};
```
