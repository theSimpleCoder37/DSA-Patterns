# Implementations are provided below:
![alt](https://github.com/user-attachments/assets/76104854-2c57-4504-b3a6-6708c4289c81)


# Brute-Force Code:
```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int rows = matrix.size();
        if (rows == 0) return;
        int cols = matrix[0].size();

        // Create two arrays to mark which rows and columns to be made zeroes
        vector<bool> zeroRows(rows, false);
        vector<bool> zeroCols(cols, false);

        // 1. Finding Zeroes: find zeroes and mark their rows and cols
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                // If we find the zero, we need to remember its row and col
                if (matrix[i][j] == 0) {
                    zeroRows[i] = true;
                    zeroCols[j] = true;
                }
            }
        }

        // 2. Setting Zeroes: set all marked rows and cols to zero
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                // If this cell is in a marked row OR col, set it to zero
                if (zeroRows[i] || zeroCols[j]) {
                    matrix[i][j] = 0;
                }
            }
        }
    }
};
```

# Optimized Code:
```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int rows = matrix.size();
        if (rows == 0) return;
        int cols = matrix[0].size();

        bool firstRowHasZero = false;
        bool firstColHasZero = false;

        // Step 1: Check if the first row or column has a zero
        for (int col = 0; col < cols; col++) {
            // If the first row has a zero, flag it to true
            if (matrix[0][col] == 0) {
                firstRowHasZero = true;
                break;
            } 
        }   

        for (int row = 0; row < rows; row++) {
            // If the first row has a zero, flag it to true
            if (matrix[row][0] == 0) {
                firstColHasZero = true;
                break;
            } 
        }

        // Step 2: Use rest of the matrix to mark zeroes
        for (int i = 1; i < rows; i++) {
            for (int j = 1; j < cols; j++) {
                // If we see a zero at any cell, mark its row's first cell and column's first cell
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }

        // Step 3: Make the cell zero based on the marks
        for (int i = 1; i < rows; i++) {
            for (int j = 1; j < cols; j++) {
                // the cell at (i, j) if it's row's first cell or column's first cell is zero
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0; // Make that cell zero
                }
            }
        }

        // Step 4: Make first row or first column zero if needed
        if (firstRowHasZero) {
            // If yes, make the first entire row zero
            for (int col = 0; col < cols; col++) {
                matrix[0][col] = 0;
            }
        }

        if (firstColHasZero) {
            // If yes, make the first entire column zero
            for (int row = 0; row < rows; row++) {
                matrix[row][0] = 0;
            }
        }
    }
};
```
