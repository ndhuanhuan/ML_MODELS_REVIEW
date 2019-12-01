
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

## Computation on NumPy Arrays: Universal Functions
### Introducing UFuncs
Vectorized operations in NumPy are implemented via ufuncs, whose main purpose is to quickly execute repeated operations on values in NumPy arrays. Ufuncs are extremely flexible 
```
x = np.arange(9).reshape((3, 3))
2 ** x
array([[  1,   2,   4],
       [  8,  16,  32],
       [ 64, 128, 256]])
 ```
 
 ### Exploring NumPy's UFuncs
 Ufuncs exist in two flavors: unary ufuncs, which operate on a single input, and binary ufuncs, which operate on two inputs.
 
 #### Array arithmetic
 ```
 x = np.arange(4)
 x     = [0 1 2 3]
 x + 5 = [5 6 7 8]
 x - 5 = [-5 -4 -3 -2]
 x * 2 = [0 2 4 6]
 x / 2 = [ 0.   0.5  1.   1.5]
 x // 2 = [0 0 1 1]
 -x     =  [ 0 -1 -2 -3]
 x ** 2 =  [0 1 4 9]
 x % 2  =  [0 1 0 1]
 
 
Operator	Equivalent ufunc	Description
+	np.add	Addition (e.g., 1 + 1 = 2)
-	np.subtract	Subtraction (e.g., 3 - 2 = 1)
-	np.negative	Unary negation (e.g., -2)
*	np.multiply	Multiplication (e.g., 2 * 3 = 6)
/	np.divide	Division (e.g., 3 / 2 = 1.5)
//	np.floor_divide	Floor division (e.g., 3 // 2 = 1)
**	np.power	Exponentiation (e.g., 2 ** 3 = 8)
%	np.mod	Modulus/remainder (e.g., 9 % 4 = 1)

 ```

#### Absolute value
```
np.abs(x)
array([2, 1, 0, 1, 2])
```
This ufunc can also handle complex data, in which the absolute value returns the magnitude:
```
x = np.array([3 - 4j, 4 - 3j, 2 + 0j, 0 + 1j])
np.abs(x)
array([ 5.,  5.,  2.,  1.])
```

#### Trigonometric functions
```
theta = np.linspace(0, np.pi, 3)
print("theta      = ", theta)
print("sin(theta) = ", np.sin(theta))
print("cos(theta) = ", np.cos(theta))
print("tan(theta) = ", np.tan(theta))

x = [-1, 0, 1]
print("x         = ", x)
print("arcsin(x) = ", np.arcsin(x))
print("arccos(x) = ", np.arccos(x))
print("arctan(x) = ", np.arctan(x))

```

#### Exponents and logarithms
```
x = [1, 2, 3]
print("x     =", x)
print("e^x   =", np.exp(x))
print("2^x   =", np.exp2(x))
print("3^x   =", np.power(3, x))

x     = [1, 2, 3]
e^x   = [  2.71828183   7.3890561   20.08553692]
2^x   = [ 2.  4.  8.]
3^x   = [ 3  9 27]
```

```
x = [1, 2, 4, 10]
print("x        =", x)
print("ln(x)    =", np.log(x))
print("log2(x)  =", np.log2(x))
print("log10(x) =", np.log10(x))

x        = [1, 2, 4, 10]
ln(x)    = [ 0.          0.69314718  1.38629436  2.30258509]
log2(x)  = [ 0.          1.          2.          3.32192809]
log10(x) = [ 0.          0.30103     0.60205999  1.        ]

x = [0, 0.001, 0.01, 0.1]
print("exp(x) - 1 =", np.expm1(x))
print("log(1 + x) =", np.log1p(x))

exp(x) - 1 = [ 0.          0.0010005   0.01005017  0.10517092]
log(1 + x) = [ 0.          0.0009995   0.00995033  0.09531018]
```

#### Specifying output
For large calculations, it is sometimes useful to be able to specify the array where the result of the calculation will be stored. Rather than creating a temporary array, this can be used to write computation results directly to the memory location where you'd like them to be. For all ufuncs, this can be done using the out argument of the function:
```
x = np.arange(5)
y = np.empty(5)
np.multiply(x, 10, out=y)
print(y)

[  0.  10.  20.  30.  40.]
```

#### Aggregates
```
x = np.arange(1, 6)
np.add.reduce(x)
15
```

```
np.multiply.reduce(x)
120
```

```
np.add.accumulate(x)
array([ 1,  3,  6, 10, 15])
```

#### Outer products
```
x = np.arange(1, 6)
np.multiply.outer(x, x)
array([[ 1,  2,  3,  4,  5],
       [ 2,  4,  6,  8, 10],
       [ 3,  6,  9, 12, 15],
       [ 4,  8, 12, 16, 20],
       [ 5, 10, 15, 20, 25]])
 ```
