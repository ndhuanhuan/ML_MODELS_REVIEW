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

## Assigning data
```
reviews['critic'] = 'everyone'
```

```
reviews['index_backwards'] = range(len(reviews), 0, -1)
```

Create a variable `df` containing the `country`, `province`, `region_1`, and `region_2` columns of the records with the index labels `0`, `1`, `10`, and `100`.
```
cols = ['country', 'province', 'region_1', 'region_2']
indices = [0, 1, 10, 100]
df = reviews.loc[indices, cols]
```

`iloc` uses the Python stdlib indexing scheme, where the first element of the range is included and the last one excluded. 
`loc`, meanwhile, indexes inclusively. 

## Summary functions
```reviews.points.describe()```
```reviews.points.mean()```
```reviews.taster_name.unique()```
```reviews.taster_name.value_counts()```

## map() apply()
```
review_points_mean = reviews.points.mean()
reviews.points.map(lambda p: p - review_points_mean)
```

```
def remean_points(row):
    row.points = row.points - review_points_mean
    return row

reviews.apply(remean_points, axis='columns')
```

```
bargain_idx = (reviews.points / reviews.price).idxmax()
bargain_wine = reviews.loc[bargain_idx, 'title']
```

## Groupwise analysis
```
>>> df = pd.DataFrame({'Animal': ['Falcon', 'Falcon',
...                               'Parrot', 'Parrot'],
...                    'Max Speed': [380., 370., 24., 26.]})
>>> df
   Animal  Max Speed
0  Falcon      380.0
1  Falcon      370.0
2  Parrot       24.0
3  Parrot       26.0
>>> df.groupby(['Animal']).mean()
        Max Speed
Animal
Falcon      375.0
Parrot       25.0
```

```
reviews.groupby('winery').apply(lambda df: df.title.iloc[0])
```

For even more fine-grained control, you can also group by more than one column.:
```
reviews.groupby(['country', 'province']).apply(lambda df: df.loc[df.points.idxmax()])
```

Another groupby() method worth mentioning is agg(), which lets you run a bunch of different functions on your DataFrame simultaneously:
```
reviews.groupby(['country']).price.agg([len, min, max])
```

## Multi-indexes
```
countries_reviewed = reviews.groupby(['country', 'province']).description.agg([len])
```

select from multiindexes: https://pandas.pydata.org/pandas-docs/stable/user_guide/advanced.html

However, in general the multi-index method you will use most often is the one for converting back to a regular index, the reset_index() method:
```
countries_reviewed.reset_index()
```

## Sorting
```
countries_reviewed = countries_reviewed.reset_index()
countries_reviewed.sort_values(by='len')
```

```
countries_reviewed.sort_values(by='len', ascending=False)
```

```
countries_reviewed.sort_index()
```

```
countries_reviewed.sort_values(by=['country', 'len'])
```

## Dtypes
```
reviews.price.dtype
```

```
reviews.dtypes
```

 transform the points column from its existing int64 data type into a float64 data type:
 ```
 reviews.points.astype('float64')
 ```

## Missing value
```
reviews[pd.isnull(reviews.country)]
```

```
reviews.region_2.fillna("Unknown")
```

```
reviews.taster_twitter_handle.replace("@kerinokeefe", "@kerino")
```

## Renaming

```
reviews.rename(columns={'points': 'score'})
```

```
reviews.rename(index={0: 'firstEntry', 1: 'secondEntry'})
```

Both the row index and the column index can have their own name attribute. The complimentary rename_axis() method may be used to change these names.
```
reviews.rename_axis("wines", axis='rows').rename_axis("fields", axis='columns')
```

## Combining
```
canadian_youtube = pd.read_csv("../input/youtube-new/CAvideos.csv")
british_youtube = pd.read_csv("../input/youtube-new/GBvideos.csv")

pd.concat([canadian_youtube, british_youtube])
```

join() lets you combine different DataFrame objects which have an index in common.
```
left = canadian_youtube.set_index(['title', 'trending_date'])
right = british_youtube.set_index(['title', 'trending_date'])

left.join(right, lsuffix='_CAN', rsuffix='_UK')
```
