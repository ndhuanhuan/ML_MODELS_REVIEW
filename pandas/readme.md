# Basics

## Constructors
1. 
```
pd.DataFrame({'Bob': ['I liked it.', 'It was awful.'], 
              'Sue': ['Pretty good.', 'Bland.']},
             index=['Product A', 'Product B'])
```

Looks like:

|             | Bob  | Sue |
|-------------| ------------- | ------------- |
|Product A| I liked it.  | Pretty good  |
|Product B| It was awful.| Bland.  |


2. 
```
fruit_sales = pd.DataFrame([[35, 21], [41, 34]], columns=['Apples', 'Bananas'],
                index=['2017 Sales', '2018 Sales'])
```

Looks like:


|             | Apples  | Bananas |
|-------------| ------------- | ------------- |
|2017 Sales| 35 | 21 |
|2018 Sales| 41 | 34 |


