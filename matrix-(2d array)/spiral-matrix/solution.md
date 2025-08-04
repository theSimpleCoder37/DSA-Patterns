> # Implementations are provided below:
![alt](https://github.com/user-attachments/assets/a1e85c09-fece-40b6-b244-2f76b1b31efc)


# Approach 1 Code: 
```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> result; // This will store our spiral elements

        int rows = matrix.size();
        int cols = matrix[0].size();

        // Check if matrix is empty
        if (matrix.empty()) {
            return result;
        }

        // Set our boundaries
        int top     = 0; // top-most row boundary 
        int bottom  = rows - 1; // bottom-most row boundary
        int left    = 0; // left-most column boundary
        int right   = cols - 1; // right-most column boundary

        // Walk in spiral form, until our boundaries gets crossed
        while (top <= bottom && left <= right) {
            // 1. Go right (top row)
            for (int col = left; col <= right; col++) {
                result.push_back(matrix[top][col]);
            }
            top++; // We're done with this top row, move down

            // 2. Go dowm (right column)
            for (int row = top; row <= bottom; row++) {
                result.push_back(matrix[row][right]);
            }
            right--; // We're done with this right column, move left

            // 3. Go left (bottom row)
            // Check: Is there still a bottom row, to go left?
            if (top <= bottom) {
                for (int col = right; col >= left; col--) {
                    result.push_back(matrix[bottom][col]);
                }
                bottom--; // We're done with this bottom row, move up
            }

            // 4. Go up (left column)
            // Check: Is there still a left column, to go up?
            if (left <= right) {
                for (int row = bottom; row >= top; row--) {
                    result.push_back(matrix[row][left]);
                }
                left++; // We're done with this left column, move right
            }
        }
        return result;
    }
};
```
# Approach 2 Code:
```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector <int> result;

        int rows = matrix.size();
        int cols = matrix[0].size();

        if (matrix.empty()) {
            return result;
        }

        int top = 0;
        int bottom = rows - 1;
        int left = 0;
        int right = cols - 1;
        
        int dir = 0; // This will decide, to which direction we have to go
        // dir = 0 -> left to right (top)
        // dir = 1 -> top to bottom (right)
        // dir = 2 -> right to left (bottom)
        // dir = 3 -> bottom to top (left)
        // dir = 4 -> dir = 0

        while (top <= bottom && left <= right) {
            // If dir = 0 -> Go left to right (top)
            if (dir == 0) {
                for (int col = left; col <= right; col++) {
                    result.push_back(matrix[top][col]);
                }
                top++; // This top-row is done, move inward
            }

            // If dir = 1 -> Go top to bottom (right)
            if (dir == 1) {
                for (int row = top; row <= bottom; row++) {
                    result.push_back(matrix[row][right]);
                }
                right--; // This right-col is done, move inward
            }

            // If dir = 2 -> Go right to left (bottom)
            if (dir == 2) {
                for (int col = right; col >= left; col--) {
                    result.push_back(matrix[bottom][col]);
                }
                bottom--; // This bottom-row is done, move inward
            }

            // If dir = 3 -> Go bottom to top (left)
            if (dir == 3) {
                for (int row = bottom; row >= top; row--) {
                    result.push_back(matrix[row][left]);
                }
                left++; // This left col is done, move inward
            }
            dir++; // Change direction at each

            // If dir = 4, make dir = 0
            dir = dir % 4;
        }

        return result;
    }
};
```
