## Step 2: Collecting more data

In order to decide which year was best for films, we will need a lot more movies than just the top50. IMDB does not provide a single wat to directly load a page with the top 4000 or so movies. But on the website, by clicking *next*, we can load the next 50 movies, and the next, and the next, etc.

If we pay attention to the URL we see that one thing changes, every time we load the next 50:

1. `https://www.imdb.com/search/title/?title_type=feature&release_date=1930-01-01,2020-01-01&num_votes=5000,&sort=user_rating,desc&start=1&view=advanced`
2. `https://www.imdb.com/search/title/?title_type=feature&release_date=1930-01-01,2020-01-01&num_votes=5000,&sort=user_rating,desc&start=51&view=advanced`
3. `https://www.imdb.com/search/title/?title_type=feature&release_date=1930-01-01,2020-01-01&num_votes=5000,&sort=user_rating,desc&start=101&view=advanced`
4. ...

The value for `start` increases by 50 every time. So first it's `start=1`, then `start=51`, then `start=101`, etc. We can use this to collect as many top rated movies as we'd like.

### Goal

Adapt your code to collect at least `m` top rated movies for every year between `s` and `e`:

- Write a loop that calls `extract_movies()` for each successive URL and collects the DataFrame's. (If you wrote it well, you should not have to adapt the function `extract_movies()` itself.)
- The loop should continue until we know that we have **at least `m` movies for every year** between 1930 and 2020.
- In the end all the DataFrames should be concatenated into one big DataFrame, that is then saved as a `.csv`.
- The resulting `.csv` should be sorted by year (ascending).
- Your program should have an optional parameter `-m` that can change the number of movies you at least want to have for every year.
- Note that the code should be adaptive. If we decide to provide another period (than between 1930 and 2020) with the optional parameters `-s` and `-e`, it should generate a top N for only that period.


### Example

When we run your program:

    $ python scraper.py -s 1930 -e 2020 -m 5 data/movies.csv

You should wind up with a large (thousands of lines) `.csv` file, like this:

    title,rating,year,actors,runtime,url
    Animal Crackers,7.5,1930,Groucho Marx;Harpo Marx;Chico Marx;The Marx Brothers,97,https://www.imdb.com//title/tt0020640/
    Hell's Angels,7.3,1930,Edmund Goulding;James Whale;Ben Lyon;James Hall;Jean Harlow;John Darrow,127,https://www.imdb.com//title/tt0020960/
    Der blaue Engel,7.7,1930,Emil Jannings;Marlene Dietrich;Kurt Gerron;Rosa Valetti,104,https://www.imdb.com//title/tt0020697/

    ...

    Sound of Metal,7.8,2019,Riz Ahmed;Olivia Cooke;Paul Raci;Lauren Ridloff,120,https://www.imdb.com//title/tt5363618/
    The Boy Who Harnessed the Wind,7.6,2019,Chiwetel Ejiofor;Maxwell Simba;Felix Lemburo;Robert Agengo,113,https://www.imdb.com//title/tt7533152/
    Batla House,7.2,2019,Nora Fatehi;Mrunal Thakur;John Abraham;Rajesh Sharma,146,https://www.imdb.com//title/tt8869978/


## Done

Check that your code meets **all** the goals from above! In the next part of the module we will focus on transforming the data.
