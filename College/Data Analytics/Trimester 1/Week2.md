# VLOOKUP

VLOOKUP in Excel is a powerful function used to search for a value in a table and return a corresponding value from a specified column. It is commonly used to quickly extract data based on a matching value.

### Syntax:

excel

Copy code

`=VLOOKUP(lookup_value, table_array, col_index_num, [range_lookup])`

### Parameters:

- **`lookup_value`**: The value you want to search for.
- **`table_array`**: The range of cells where you want to search for the value.
- **`col_index_num`**: The column number (starting from 1) in the table from which to return the value.
- **`range_lookup`**: (Optional) A boolean indicating if you want an exact match (`FALSE`) or an approximate match (`TRUE`). If omitted, the default is `TRUE`.

### Example:

Let's say you have the following table:

| A       | B     |
| ------- | ----- |
| Product | Price |
| Apple   | 1.2   |
| Banana  | 0.5   |
| Orange  | 0.8   |

If you want to find the price of an "Orange", you could use this formula:

excel

Copy code

`=VLOOKUP("Orange", A2:B4, 2, FALSE)`

This will return `0.8`, the price of Orange, because:

- **`lookup_value`** is "Orange"
- **`table_array`** is the range `A2:B4`
- **`col_index_num`** is `2`, meaning it will return the value from the 2nd column (Price)
- **`range_lookup`** is `FALSE`, ensuring an exact match.

Would you like to dive deeper into any specific aspect?





# HLOOKUP
 

HLOOKUP in Excel is similar to VLOOKUP, but instead of searching for a value in a column, it searches for a value in a row and returns a corresponding value from a specified row.

### Syntax:

excel

Copy code

`=HLOOKUP(lookup_value, table_array, row_index_num, [range_lookup])`

### Parameters:

- **`lookup_value`**: The value you want to search for in the first row of the table.
- **`table_array`**: The range of cells where you want to search for the value.
- **`row_index_num`**: The row number (starting from 1) in the table from which to return the value.
- **`range_lookup`**: (Optional) A boolean indicating if you want an exact match (`FALSE`) or an approximate match (`TRUE`). If omitted, the default is `TRUE`.

### Example:

Consider the following table:
![[Pasted image 20240921164633.png]]

 

If you want to find the price of a "Banana", you could use this formula:


`=HLOOKUP("Banana", A1:C2, 2, FALSE)`

This will return `0.5`, the price of Banana, because:

- **`lookup_value`** is "Banana"
- **`table_array`** is the range `A1:C2`
- **`row_index_num`** is `2`, meaning it will return the value from the 2nd row (Price)
- **`range_lookup`** is `FALSE`, ensuring an exact match.

HLOOKUP is useful when your data is organized horizontally. Would you like any further examples or clarifications?

# X LOOK UP

`XLOOKUP` is a more flexible and powerful function in Excel compared to `VLOOKUP` and `HLOOKUP`. It allows you to search for a value in a column or row and return a corresponding value from another column or row, with more advanced features such as support for exact, approximate, and wildcard matches.

### Syntax:

excel

Copy code

`=XLOOKUP(lookup_value, lookup_array, return_array, [if_not_found], [match_mode], [search_mode])`

### Parameters:

- **`lookup_value`**: The value you want to search for.
- **`lookup_array`**: The range (row or column) where you want to search for the value.
- **`return_array`**: The range (row or column) from which you want to return the result.
- **`if_not_found`**: (Optional) The value to return if no match is found. If omitted, an error will be returned.
- **`match_mode`**: (Optional) Defines the type of match:
    - `0`: Exact match (default).
    - `-1`: Exact match or next smaller item.
    - `1`: Exact match or next larger item.
    - `2`: Wildcard match.
- **`search_mode`**: (Optional) Controls the search order:
    - `1`: Search from first to last (default).
    - `-1`: Search from last to first.

### Example:

Consider the following table:

| A       | B     |
| ------- | ----- |
| Product | Price |
| Apple   | 1.2   |
| Banana  | 0.5   |
| Orange  | 0.8   |

If you want to find the price of "Banana", you could use this formula:

excel

Copy code

`=XLOOKUP("Banana", A2:A4, B2:B4, "Not found")`

This will return `0.5` because:

- **`lookup_value`** is "Banana"
- **`lookup_array`** is `A2:A4` (the Product column)
- **`return_array`** is `B2:B4` (the Price column)
- **`if_not_found`** will return "Not found" if there's no match (instead of an error).

### Key Advantages of `XLOOKUP`:

- Can search both vertically and horizontally (unlike `VLOOKUP` and `HLOOKUP`).
- No need to specify a column index; the return array is directly specified.
- Supports searches from bottom to top or left to right.
- Handles missing values with a custom response (using the `if_not_found` parameter).
- More robust matching modes, including wildcards for partial matches.

Would you like a more complex example or further details?

![[Pasted image 20240921162921.png]]
# Match Function 
The `MATCH` function in Excel is used to find the position of a value in a specified range. Instead of returning the actual value, it returns the relative position of the value within the range. It is often used together with the `INDEX` function for flexible lookups.

### Syntax:

excel

Copy code

`=MATCH(lookup_value, lookup_array, [match_type])`

### Parameters:

- **`lookup_value`**: The value you want to search for.
- **`lookup_array`**: The range of cells where you want to search for the `lookup_value`.
- **`match_type`**: (Optional) Specifies the type of match:
    - **`1`**: (default) Finds the largest value that is less than or equal to the `lookup_value`. The array must be sorted in ascending order.
    - **`0`**: Finds the first exact match. The array can be in any order.
    - **`-1`**: Finds the smallest value that is greater than or equal to the `lookup_value`. The array must be sorted in descending order.

### Example 1: Finding the Position of a Value

Let's say you have the following data:

| A       | B     |
| ------- | ----- |
| Product | Price |
| Apple   | 1.2   |
| Banana  | 0.5   |
| Orange  | 0.8   |

If you want to find the position of "Banana" in the product list (`A2:A4`), you can use the `MATCH` function:

excel

Copy code

`=MATCH("Banana", A2:A4, 0)`

This will return `2`, because "Banana" is in the 2nd row of the range `A2:A4`.

### Example 2: Using `MATCH` for Exact Match

If you are searching for the exact price of `0.8` in the `B2:B4` range, you can use:

excel

Copy code

`=MATCH(0.8, B2:B4, 0)`

This will return `3`, because `0.8` is in the 3rd row of the range `B2:B4`.

### Example 3: Combining `MATCH` with `INDEX`

`MATCH` is often used in conjunction with `INDEX` for more dynamic lookups.

For instance, to find the price of "Orange" using `INDEX` and `MATCH`, you can use:

excel

Copy code

`=INDEX(B2:B4, MATCH("Orange", A2:A4, 0))`

Here:

- **`MATCH("Orange", A2:A4, 0)`**: This finds the row where "Orange" is located.
- **`INDEX(B2:B4, ...)`**: This retrieves the price from the same row in the price column.

This formula will return `0.8`, the price of "Orange."

### Example 4: Using `MATCH` for Approximate Matches

If you have a sorted list and you want to use an approximate match (the default behavior), you can use `MATCH` with `1` or `-1` for different behaviors:

#### Ascending Order (for `1`):

Consider the following marks and grade boundaries:

|A|B|
|---|---|
|Marks|Grade|
|30|D|
|50|C|
|70|B|
|90|A|

To find the position for a student with 65 marks (which falls between 50 and 70), use:

excel

Copy code

`=MATCH(65, A2:A5, 1)`

This will return `2`, because `65` is closest to the largest value less than or equal to it (50).

#### Descending Order (for `-1`):

If the list is sorted in descending order and you want to find a match for 65:

|A|B|
|---|---|
|90|A|
|70|B|
|50|C|
|30|D|

You can use:

excel

Copy code

`=MATCH(65, A2:A5, -1)`

This will return `2`, because `65` is closest to the smallest value greater than or equal to it (70).

### Conclusion:

- The `MATCH` function is used to find the relative position of a value within a range.
- It can be used alone or combined with `INDEX` to create powerful lookup formulas.
- `MATCH` is more flexible than `VLOOKUP` as it can search both horizontally and vertically, and doesn't require the lookup value to be in the first column of the range.