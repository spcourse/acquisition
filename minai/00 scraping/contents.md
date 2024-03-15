# Scraping

Web scraping is a way of extracting data from websites. While this process could be done manually (by reading information on a website, and then copying that information to a file) it is usually done through the use of software. Scraping can be a valuable tool for extracting data. A website might not give you a (free) option to download the content, either through an API or a direct download link. One example of such a website is the IMDB page that we will be scraping in this exercise.

### Libraries

In this assignment you will learn about three libraries:

- We'll use *BeautifulSoup* to parse the Document Object Model (DOM) of IMDB. We provide some scaffolding for the programming exercise, so you'll not have to use BeautifulSoup yourself. [BeautifulSoup documentation](https://www.crummy.com/software/BeautifulSoup/bs4/doc/). Take a look at [this video](https://www.youtube.com/embed/ng2o98k983k) if you would like to learn more about BeautifulSoup.
- *Pandas* provide a convenient way to store and manipulate data in python. [Pandas documentation](https://pandas.pydata.org/docs/)
- *Argparse* is a built-in library with which you can manage command line arguments for your programs. [Argparse documentation](https://docs.python.org/3/library/argparse.html) We provide you with an example, after which you'll use this library yourself to make more programs.

## Research questions

*What was the best year for movies?*

This is often debated on the internet. For example [here](https://www.washingtonpost.com/news/style/wp/2018/12/28/feature/what-was-the-best-year-in-movie-history/), [here](https://www.independent.co.uk/arts-entertainment/films/features/film-history-best-year-1999-star-wars-matrix-fight-club-sixth-sense-a9036911.html), [here](https://www.reddit.com/r/movies/comments/5m6jrp/best_year_for_movies/) and [here](https://www.maxim.com/entertainment/10-movies-prove-1994-was-best-year-film-history).

Let's try to see if we can find some data to settle these discussions. For this we need to do some *web scraping*.

In order to answer the questions we'll have to make some assumptions. Most importantly we're going to assume that the top 5 movies of each year are indicative of how good that year was for movies. So we'll reformulate the question:

*What year had the best average IMDB rating for it's top 5 movies?*

Admittedly a much less catchy question.

### Pipeline

For this exercise you will set up a data pipeline with the following steps:

| Pipeline step           |                                                                                                 |
|-------------------------|-------------------------------------------------------------------------------------------------|
| Asking a Question       | What was the best year for movies?                                                              |
| Acquiring the data      | Using BeautifulSoup and pandas to scrape IMDB pages to a CSV file (`scraper.py`)                |
| Transforming the data   | Use the acquired CSV to filter out relevant data (`extract.py`)                                 |
<!-- | Visualizing the data    | Plotting the average score for the top 5 movies for each year (`visualize_years.py`)            | -->
| Collecting more data    | Use links we've collected earlier to scrape more data about our movies from IMDB (`crawler.py`) |
| Visualizing the data    | Answering our research questions by plotting data (`visualize_years.py`)                        |


## Reading and understanding the provided code in `scraper.py`

Download the code you need for this assignment: [scraping.zip](../code/scraping.zip).

To get you started we have provided you with a script (`scraper.py`) that loads the correct IMDB address. It is currently set up in a way that it scrapes the 50 best rated movies between 1930 and 2020, that have been rated at least 5000 times, by scraping the link:

  <https://www.imdb.com/search/title/?title_type=feature&release_date=1930-01-01,2020-12-31&num_votes=5000,&sort=user_rating,desc>

- `title_type=feature` indicates we only want so called "feature films".
- `release_date=1930-01-01,2020-12-31` indicates we want movies released between 1930 up to and including 2020.
- `num_votes=5000` indicates we want movies that have been rated at least 5000 times
- `sort=user_rating,desc` indicates we want to sort by user rating, in descending order

Open up and carefully read the `scraper.py`-file. It contains a couple of functions:

- `main()`: Set up such that the link <https://www.imdb.com/search/title/?title_type=feature&release_date=1930-01-01,2020-12-31&sort=user_rating,desc&num_votes=5000> is visited, then transformed into a Document Object Model (DOM), which is in turn analysed by the `extract_movies()` function using BeautifulSoup, which returns a DataFrame we save to a csv. You will adjust this function later.
- `extract_movies()`: Extracts all movie data from a IMDB DOM. Returns a DataFrame containing the title, rating, release year, runtime (in minutes), and a URL for each movie found in the DOM.
- `extract_movie_data()`: Helper function that extracts data for a single movie.
- `convert_runtime()`: Helper function that parses a runtime string and returns the runtime of a movie in minutes. Runtime strings have the format "[<hours>h] [<minutes>m]".

We can run the file using the command:

    python scraper.py data/top50.csv

This will produce a file called `top50.csv` in the directory `data` containing approximately the following data:

    title,rating,year,runtime,url
    The Shawshank Redemption,9.3,1994,142,https://www.imdb.com/title/tt0111161/
    The Godfather,9.2,1972,175,https://www.imdb.com/title/tt0068646/
    Hababam Sinifi,9.2,1975,87,https://www.imdb.com/title/tt0252487/

    ...

    Shoah,8.7,1985,566,https://www.imdb.com/title/tt0090015/
    Soorarai Pottru,8.7,2020,153,https://www.imdb.com/title/tt10189514/
    Ko to tamo peva,8.7,1980,86,https://www.imdb.com/title/tt0076276/

In total the `top50.csv` file should contain 51 lines that can be cross-checked with the information you'd get if you visit the IMDB link yourself.

## Modifying `scraper.py`

In order to decide which year was best for films, we will need a lot more movies than just the top50. IMDB does not provide an option to directly load a page with the top 4000 or so movies. On the website, by clicking the button with "50 more", we can load the next 50 movies, and the next, and the next, etc. This is very inconvenient as our program can not click this button.

However, if we pay attention to the URL we can see that we could do this in a more efficient fashion:

    https://www.imdb.com/search/title/?title_type=feature&release_date=1930-01-01,2020-12-31&sort=user_rating,desc&num_votes=5000

We can adjust the dates in the link such that the results we get are the top 50 movies for a single year, and then repeat that request for every year that we need!

### Argparse

You might have noticed that there is some more code all the way at the bottom of the file, that is run before we run our `main()` function. It uses the Argparse library to parse command line arguments, such that we can utilize these as variables in our code. These arguments can be accessed as follows

  python scraper.py movies.csv -s 1930 -e 2020

This would set the variable `args.output` to "movies.csv", the variable `args.start_year` to 1930, and the variable `args.end_year` to 2020. Currently, these values are then passed to the `main()` function, but only `output` is used.

Make sure you understand what is happening here. _You'll not be changing this code in this part of the assignment, but we expect you to create something similar in a future assignment!_

### Goal

Adapt the code in the `main()` function to collect 50 top rated movies _for every year_ between `start_year` and `end_year`:

- Write a loop that calls `extract_movies()` for each successive URL and collects a DataFrame with up to 50 movies for every year. You should not have to adapt the function `extract_movies()` itself.
- In the end all the DataFrames should be concatenated into one big DataFrame, that is then saved as a `.csv`.
- The resulting `.csv` should be sorted by year (ascending).
- Note that the code should be adaptive. If we decide to provide another period (than between 1930 and 2020) with the optional parameters `-s` and `-e`, it should scrape for only that period!

> Of course there are some years where there are fewer than 50 movies that have more than 5000 ratings (for example, 1931 only has 8). Don't worry, as `extract_movies()` will then just return a smaller dataframe.

### Example

When we run your program:

    $ python scraper.py -s 1930 -e 2020 data/movies.csv

You should wind up with a large (thousands of lines) `.csv` file, like this:

    title,rating,year,runtime,url
    All Quiet on the Western Front,8.1,1930,152,https://www.imdb.com/title/tt0020629/
    Der blaue Engel,7.7,1930,104,https://www.imdb.com/title/tt0020697/
    Animal Crackers,7.4,1930,97,https://www.imdb.com/title/tt0020640/

    ...

    Feels Good Man,7.5,2020,92,https://www.imdb.com/title/tt11394182/
    I Am Greta,7.5,2020,97,https://www.imdb.com/title/tt10394738/
    Lootcase,7.5,2020,132,https://www.imdb.com/title/tt10515526/

While with the following command:

    $ python scraper.py -s 1935 -e 1940 data/small_movies.csv

We'd expect a much smaller file!

    title,rating,year,runtime,url
    A Tale of Two Cities,7.8,1935,128,https://www.imdb.com/title/tt0027075/
    Bride of Frankenstein,7.8,1935,75,https://www.imdb.com/title/tt0026138/
    A Night at the Opera,7.8,1935,96,https://www.imdb.com/title/tt0026778/

    ...

    The Bank Dick,7.1,1940,72,https://www.imdb.com/title/tt0032234/
    Go West,6.8,1940,80,https://www.imdb.com/title/tt0032536/
    The Invisible Man Returns,6.5,1940,81,https://www.imdb.com/title/tt0032635/

> Keep in mind that doing this with large ranges of years might take a while, so it might be wise to somehow indicate the year for which your program is currently retrieving data. This way you have an indication that your program is still running, and how long it will take. Starting with a smaller range to test everything is probably wise!

## Done?

Check that your code meets **all** the goals from above! In the next part of the module we will focus on transforming the data.
