
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
