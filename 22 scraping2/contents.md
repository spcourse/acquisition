## Step 2: Collecting more data

In order to decide which year was best for films, we will need a lot more movies than just the top50. IMDB does not provide an option to directly load a page with the top 4000 or so movies. On the website, by clicking *50 more*, we can load the next 50 movies, and the next, and the next, etc. This is very inconvenient.

However, if we pay attention to the URL we can see that we could do this in a more efficient fashion:

    `https://www.imdb.com/search/title/?title_type=feature&release_date=1930-01-01,2020-01-01&num_votes=5000,&sort=user_rating,desc&view=advanced`

We can adjust the dates in the link such that the results we get are the top 50 movies for a single year, and then repeat that request for every year that we need!

### Goal

Adapt your code to collect 50 top rated movies for every year between `s` and `e`:

- Write a loop that calls `extract_movies()` for each successive URL and collects a DataFrame with 50 movies for every year. (If you wrote it well, you should not have to adapt the function `extract_movies()` itself.)
- In the end all the DataFrames should be concatenated into one big DataFrame, that is then saved as a `.csv`.
- The resulting `.csv` should be sorted by year (ascending).
- Note that the code should be adaptive. If we decide to provide another period (than between 1930 and 2020) with the optional parameters `-s` and `-e`, it should generate a top N for only that period.

### Example

When we run your program:

    $ python scraper.py -s 1930 -e 2020 data/movies.csv

You should wind up with a large (thousands of lines) `.csv` file, like this:

    title,rating,year,runtime,url
    Animal Crackers,7.5,1930,97,https://www.imdb.com//title/tt0020640/
    Hell's Angels,7.3,1930,127,https://www.imdb.com//title/tt0020960/
    Der blaue Engel,7.7,1930,104,https://www.imdb.com//title/tt0020697/

    ...

    Sound of Metal,7.8,2019,120,https://www.imdb.com//title/tt5363618/
    The Boy Who Harnessed the Wind,7.6,2019,113,https://www.imdb.com//title/tt7533152/
    Batla House,7.2,2019,146,https://www.imdb.com//title/tt8869978/


## Done

Check that your code meets **all** the goals from above! In the next part of the module we will focus on transforming the data.
