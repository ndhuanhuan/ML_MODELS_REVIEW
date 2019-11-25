
## Array
### Array Attributes
```
x3 = np.random.randint(10, size=(3, 4, 5))  # Three-dimensional array
print("x3 ndim: ", x3.ndim)
print("x3 shape:", x3.shape)
print("x3 size: ", x3.size)
print("dtype:", x3.dtype)
```

Other attributes include itemsize, which lists the size (in bytes) of each array element, and nbytes, which lists the total size (in bytes) of the array:
```
print("itemsize:", x3.itemsize, "bytes")
print("nbytes:", x3.nbytes, "bytes")
```

### Array Indexing: Accessing Single Elements
In a multi-dimensional array, items can be accessed using a comma-separated tuple of indices:
```
array([[3, 5, 2, 4],
       [7, 6, 8, 8],
       [1, 6, 7, 7]])
 x2[2, -1]
 ```
Keep in mind that, unlike Python lists, NumPy arrays have a fixed type. This means, for example, that if you attempt to insert a floating-point value to an integer array, the value will be silently truncated.

### Array Slicing: Accessing Subarrays
 The NumPy slicing syntax follows that of the standard Python list; to access a slice of an array x, use this:
 ```
 x[start:stop:step]
 
 x[1::2]  # every other element, starting at index 1
 x[5::-2]  # reversed every other from index 5
 ```
 
 ### Multi-dimensional subarrays
 ```
 array([[12,  5,  2,  4],
       [ 7,  6,  8,  8],
       [ 1,  6,  7,  7]])
  x2[:2, :3]  # two rows, three columns
  x2[:3, ::2]  # all rows, every other column
  ```
 
### Accessing array rows and columns
One commonly needed routine is accessing of single rows or columns of an array. This can be done by combining indexing and slicing, using an empty slice marked by a single colon (:):
```
print(x2[:, 0])  # first column of x2
print(x2[0, :])  # first row of x2
```

### Subarrays as no-copy views
One important–and extremely useful–thing to know about array slices is that they return views rather than copies of the array data.
This default behavior is actually quite useful: it means that when we work with large datasets, we can access and process pieces of these datasets without the need to copy the underlying data buffer.

### Creating copies of arrays
Despite the nice features of array views, it is sometimes useful to instead explicitly copy the data within an array or a subarray. This can be most easily done with the copy() method:
```
x2_sub_copy = x2[:2, :2].copy()
```

### Reshaping of Arrays
```
grid = np.arange(1, 10).reshape((3, 3))
print(grid)

[[1 2 3]
 [4 5 6]
 [7 8 9]]
 ```

Another common reshaping pattern is the conversion of a one-dimensional array into a two-dimensional row or column matrix.
```
x = np.array([1, 2, 3])

# row vector via reshape
x.reshape((1, 3))

array([[1, 2, 3]])
```

```
# row vector via newaxis
x[np.newaxis, :]

array([[1, 2, 3]])
```

```
# column vector via reshape
x.reshape((3, 1))

# column vector via newaxis
x[:, np.newaxis]

array([[1],
       [2],
       [3]])
```

### Array Concatenation and Splitting
#### Concatenation of arrays
```
x = np.array([1, 2, 3])
y = np.array([3, 2, 1])
np.concatenate([x, y])
array([1, 2, 3, 3, 2, 1])

grid = np.array([[1, 2, 3],
                 [4, 5, 6]])
# concatenate along the first axis
np.concatenate([grid, grid])
array([[1, 2, 3],
       [4, 5, 6],
       [1, 2, 3],
       [4, 5, 6]])

# concatenate along the second axis (zero-indexed)
np.concatenate([grid, grid], axis=1)
array([[1, 2, 3, 1, 2, 3],
       [4, 5, 6, 4, 5, 6]])
```

For working with arrays of mixed dimensions, it can be clearer to use the np.vstack (vertical stack) and np.hstack (horizontal stack) functions:

```
x = np.array([1, 2, 3])
grid = np.array([[9, 8, 7],
                 [6, 5, 4]])

# vertically stack the arrays
np.vstack([x, grid])

array([[1, 2, 3],
       [9, 8, 7],
       [6, 5, 4]])
       
y = np.array([[99],
              [99]])
np.hstack([grid, y])

array([[ 9,  8,  7, 99],
       [ 6,  5,  4, 99]])
```

### Splitting of arrays
Notice that N split-points, leads to N + 1 subarrays.
```
x = [1, 2, 3, 99, 99, 3, 2, 1]
x1, x2, x3 = np.split(x, [3, 5])
print(x1, x2, x3)
[1 2 3] [99 99] [3 2 1]
```

```
grid = np.arange(16).reshape((4, 4))
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11],
       [12, 13, 14, 15]])
 upper, lower = np.vsplit(grid, [2])
 [[0 1 2 3]
 [4 5 6 7]]
[[ 8  9 10 11]
 [12 13 14 15]]
 left, right = np.hsplit(grid, [2])
 [[ 0  1]
 [ 4  5]
 [ 8  9]
 [12 13]]
[[ 2  3]
 [ 6  7]
 [10 11]
 [14 15]]
 ```


