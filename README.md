# Pandas-and-funtions
## Write a function that classifies movies from the lesson materials according to the following rules:

-a rating of 2 or lower is a low rating;

-a rating of 4 or lower is a medium rating;

-a rating of 4.5 or 5 is a high rating.

-Write the classification result in the class column.
```python
import pandas as pd
import numpy as np
df = pd.read_csv("ratings.csv")
def rating_class(rating):
    if rating <= 2:
        return 'низкий рейтинг'
    elif rating <= 4:
        return 'средний рейтинг'
    else:
        return 'высокий рейтинг'
df['class'] = df['rating'].apply(rating_class)
```
## You need to write a geo-classifier that can assign a geographical region to each row. 

If the search query contains the name of a city in the region, the ‘region’ column will contain the name of that region. 

If the search query does not contain the name of a city, the ‘undefined’ value will be used.
```python
reg = pd.read_csv("keywords.csv")
geo_data = {
    'Центр': ['москва', 'тула', 'ярославль'],
    'Северо-Запад': ['петербург', 'псков', 'мурманск'],
    'Дальний Восток': ['владивосток', 'сахалин', 'хабаровск']
}
def geo_classify(query):
    query = query.lower()
    
    for region, cities in geo_data.items():
        for city in cities:
            if city in query:
                return region
    return 'undefined'
reg['region'] = reg['keyword'].apply(geo_classify)
```
## We need to check if it's true that as the release year of a movie increases, its average rating decreases.
#### In the variable years, write a list of all years from 1950 to 2010.

#### Write a production_year function that assigns a release year to each line of a movie title. Not all movie titles contain the release year in the same format, so use the following algorithm:

- for each line, iterate through all the years in the years list;
- if the year number is present in the movie title, the function returns that year as the release year;
- if none of the year numbers in the years list appear in the movie title, then the year 1900 is returned.
- Write the release year of the movie according to the algorithm of point 2 in a new column ‘year’.

#### Calculate the average rating of all movies for each value of the ‘year’ column and sort the result by descending rating.
```python
import pandas as pd

movies = pd.read_csv('movies.csv')
ratings = pd.read_csv('ratings.csv')
years = list(range(1950, 2011))
def production_year(title):
    for year in years:
        if str(year) in title:
            return year
    return 1900
movies['year'] = movies['title'].apply(production_year)
movies_ratings = ratings.merge(
    movies[['movieId', 'year']],
    on='movieId',
    how='left'
)
avg_rating_by_year = (
    movies_ratings
    .groupby('year')['rating']
    .mean()
    .sort_values(ascending=False)
)
```
