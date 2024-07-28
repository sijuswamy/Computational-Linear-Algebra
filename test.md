## Mathematical expressions

$$\begin{cases}
x + 2y + 3z &= 9 \\
4x + 5y + 6z& = 24 \\
7x + 8y + 9z& = 39
\end{cases}$$

```python
FUNCTION solve_system_of_equations(A, b):
    # Step 1: Get the dimensions of the matrix
    rows = number_of_rows(A)
    cols = number_of_columns(A)
    
    # Step 2: Create the augmented matrix
    augmented_matrix = create_augmented_matrix(A, b)
    
    # Step 3: Transform the augmented matrix to row echelon form
    row_echelon_form(augmented_matrix, rows, cols)
    
    # Step 4: Perform back substitution
    solution = back_substitution(augmented_matrix, rows, cols)
    
    RETURN solution
END FUNCTION

FUNCTION create_augmented_matrix(A, b):
    # Combine A and b into an augmented matrix
    augmented_matrix = []
    FOR i FROM 0 TO number_of_rows(A)-1:
        augmented_matrix.append(A[i] + [b[i]])
    RETURN augmented_matrix
END FUNCTION

FUNCTION row_echelon_form(augmented_matrix, rows, cols):
    # Perform Gaussian elimination
    lead = 0
    FOR r FROM 0 TO rows-1:
        IF lead >= cols:
            RETURN
        i = r
        WHILE augmented_matrix[i][lead] == 0:
            i = i + 1
            IF i == rows:
                i = r
                lead = lead + 1
                IF lead == cols:
                    RETURN
        # Swap rows i and r
        swap_rows(augmented_matrix, i, r)
        # Make augmented_matrix[r][lead] = 1
        lv = augmented_matrix[r][lead]
        augmented_matrix[r] = [m / float(lv) for m in augmented_matrix[r]]
        # Make all rows below r have 0 in column lead
        FOR i FROM r + 1 TO rows-1:
            lv = augmented_matrix[i][lead]
            augmented_matrix[i] = [iv - lv * rv for rv, iv in zip(augmented_matrix[r], augmented_matrix[i])]
        lead = lead + 1
END FUNCTION

FUNCTION back_substitution(augmented_matrix, rows, cols):
    # Initialize the solution vector
    solution = [0 for _ in range(rows)]
    # Perform back substitution
    FOR i FROM rows-1 DOWNTO 0:
        solution[i] = augmented_matrix[i][cols-1]
        FOR j FROM i+1 TO cols-2:
            solution[i] = solution[i] - augmented_matrix[i][j] * solution[j]
    RETURN solution
END FUNCTION

FUNCTION swap_rows(matrix, row1, row2):
    temp = matrix[row1]
    matrix[row1] = matrix[row2]
    matrix[row2] = temp
END FUNCTION
```
