![[Drawing 2024-10-07 17.34.41.excalidraw]][73. Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)

 

Given an `m x n` integer matrix `matrix`, if an element is `0`, set its entire row and column to `0`'s.

 ![[Pasted image 20240923133615.jpg]]
 
**Input:** matrix = [[1,1,1],[1,0,1],[1,1,1]]
**Output:** [[1,0,1],[0,0,0],[1,0,1]]![[Pasted image 20240923133654.jpg]]
**Input:** matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
**Output:** [[[0,0,0,0],[0,4,5,0],[0,3,1,0]]



answer:- 
```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int rows = matrix.size();
        int cols = matrix[0].size();
        bool colSet = false;

        // First pass: use the first row and column to mark zero rows and columns
        for (int i = 0; i < rows; i++) {
            if (matrix[i][0] == 0) colSet = true;
            for (int j = 1; j < cols; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0; // mark the row and column as 0
                }
            }
        }

        // Second pass: use the marks to set elements to zero
        for (int i = 1; i < rows; i++) {
            for (int j = 1; j < cols; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }

        // If the first row needs to be set to zero
        if (matrix[0][0] == 0) {
            for (int j = 0; j < cols; j++) {
                matrix[0][j] = 0;
            }
        }

        // If the first column needs to be set to zero
        if (colSet) {
            for (int i = 0; i < rows; i++) {
                matrix[i][0] = 0;
            }
        }
    }
};

```

