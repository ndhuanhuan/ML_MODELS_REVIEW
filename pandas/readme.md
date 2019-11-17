# Basics

## Constructors
There are two core objects in pandas: the DataFrame and the Series.

### DataFrame
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


### Series
1. 
```
quantities = ['4 cups', '1 cup', '2 large', '1 can']
items = ['Flour', 'Milk', 'Eggs', 'Spam']
ingredients = pd.Series(quantities, index=items, name='Dinner')
```
Looks like
```
Flour     4 cups
Milk       1 cup
Eggs     2 large
Spam       1 can
Name: Dinner, dtype: object
```

## Reading data files
```
wine_reviews = pd.read_csv("../input/wine-reviews/winemag-data-130k-v2.csv", index_col=0)
wine_reviews.head()
```

## Save data file
```
animals.to_csv("cows_and_goats.csv")
```

## index based selection
```
first_description = reviews.description.iloc[0]
```

To get a row:
```
reviews.iloc[0]
```

To get a col:
```
reviews.iloc[:, 0]
```

 to select the country column from just the first, second, and third row, we would do:
 ```
 reviews.iloc[:3, 0]
 ```
 
 It's also possible to pass a list:
 ```
 reviews.iloc[[0, 1, 2], 0]
 ```
 
 ## label based selection
 ```
 reviews.loc[:, ['taster_name', 'taster_twitter_handle', 'points']]
 ```

## Manipulating the index
```
reviews.set_index("title")
```

## Conditional selection
```
reviews.country == 'Italy'
```

or

```
reviews.loc[reviews.country == 'Italy']
```

```
reviews.loc[(reviews.country == 'Italy') & (reviews.points >= 90)]
```

```
reviews.loc[(reviews.country == 'Italy') | (reviews.points >= 90)]
```

isin is lets you select data whose value "is in" a list of values.:
```
reviews.loc[reviews.country.isin(['Italy', 'France'])]
```

notnull or isnull:
```
reviews.loc[reviews.price.notnull()]
```
