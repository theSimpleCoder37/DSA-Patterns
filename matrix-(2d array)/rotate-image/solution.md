> # Implementations are provided below:
![rotate-image excalidraw](https://github.com/user-attachments/assets/3f270553-dd00-473f-857c-b3e2b3064806)

# My Approach Code:
```cpp
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();

        // Transpose the matrix
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                swap(matrix[i][j], matrix[j][i]);
            }
        }

        // Reverse the rows
        for (int row = 0; row < n; row++) {
            int start = 0, end = n - 1;
            while (start < end) {
                swap(matrix[row][start], matrix[row][end]);
                start++;
                end--;
            }
        }
    }
};
```
