# Scraping

*What was the best year for movies?*

This is often debated on the internet. For example [here](https://www.washingtonpost.com/news/style/wp/2018/12/28/feature/what-was-the-best-year-in-movie-history/), [here](https://www.independent.co.uk/arts-entertainment/films/features/film-history-best-year-1999-star-wars-matrix-fight-club-sixth-sense-a9036911.html), [here](https://www.reddit.com/r/movies/comments/5m6jrp/best_year_for_movies/) and [here](https://www.maxim.com/entertainment/10-movies-prove-1994-was-best-year-film-history).

Let's try to see if we can find some data to settle these discussions. For this we need to do some *web scraping*.

Web scraping is a way of extracting data from websites. While this process could be done manually (by reading information on a website, and then copying that information to a file) it is usually done through the use of software. Scraping can be a valuable tool for extracting data. A website might not give you an option to download the content, either through an API or a direct download link. One example of such a website is the IMDB page that we will be scraping in this exercise.

In order to answer the questions we'll have to make some assumptions. Most importantly we're going to assume that the top 5 movies of each year are indicative of how good for movies that year is. So we'll reformulate the question:

*What year had the best average IMDB rating for it's top 5 movies?*

Admittedly a much less catchy question.

## Provided code

Download the code you need for this assignment: [scraping.zip](../code/scraping.zip)

## Libraries

In this assignment you will learn to use three libraries:

- You'll use *BeautifulSoup* to parse the Document Object Model (DOM) if IMDB. We provide some scaffolding for the programming exercise. [BeautifulSoup documentation](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
- *Pandas* provide a convenient way to store and manipulate data in python. [Pandas documentation](https://pandas.pydata.org/docs/)
- *Argparse* is a built-in library with which you can manage command line arguments for your programs. [Argparse documentation](https://docs.python.org/3/library/argparse.html)

## Pipeline

For this exercise you will set up a data pipeline with the following steps:

| Pipeline step          	| This week                                                                                     |
|------------------------	|---------------------------------------------------------------------------------------------- |
| Asking a Question      	| What was the best year for movies?                                                            |
| Acquiring the data     	| Using pandas to scrape IMDB pages to a CSV file (`scraper.py`)                                |
| Transforming the data 	| Use the acquired CSV to generate another file containing a top 5 for each year (`extract.py`) |
| Visualizing the data  	| Plotting the average score for the top 5 movies for each year (`visualize_years.py`)          |

## Before starting

1. For this assignment you'll need to have the following Python 3 libraries installed: Requests, Pandas, and BeautifulSoup4.

2. To get you started we have provided you with some code (`scraper.py` and `helpers.py`).
    - The file `helpers.py` contains the function `simple_get` for loading a webpage.
    - The file `scraper.py` is the file you'll be editing. To get started, it already contains some code that imports `simple_get` and uses it to load the correct IMDB address and prints the first line.

3. When scraping a website there can be inconsistencies in the HTML. It could for instance be that there are missing data (for instance the realease year for a film or the runtime). Make sure to check this and insert an appropriate value when something is missing.

## BeautifulSoup

To scrape data from webpages, we will be using BeautifulSoup, a Python web mining module. Its description is as follows: _Beautiful Soup is a Python library for pulling data out of HTML and XML files. It works with your favorite parser to provide idiomatic ways of navigating, searching, and modifying the parse tree. It commonly saves programmers hours or days of work._

This is the introductory exercise to BeautifulSoup. We will try to guide you along as much as possible, but you should read up on documentation and get used to doing that. It's a really useful skill and a big part of programming is self-­learning! Watch the following videos on HTML and the DOM. Ignore any reference to JavaScript (you may replace it in your mind with BeautifulSoup), as it will not be needed in our exercises.

![embed](https://www.youtube.com/embed/ng2o98k983k)

### Building `scraper.py`

To get you started we have provided you with a script (`scraper.py`) that loads the correct IMDB address.

Open up and read the `scraper.py`-file. Note that this is just some scaffolding, so you actually don't have to use this at all. As long as your code runs at the end of the day and produces the right results in a CSV file, we're happy.

We can run the file using the command:

    python scraper.py data/movies.csv

This does a number of things:

- It reads the following URL: [https://www.imdb.com/search/title/?title_type=feature&release_date=1930-01-01,2020-01-01&num_votes=5000,&sort=user_rating,desc&start=1&view=advanced]
- The function `extract_movies()` reads the header from the webpage and prints it. This print statement is just an example to get you started, but should be removed. The function shouldn't print anything when it's finished.
- The function `extract_movies()` currently returns a pandas DataFrame with nonsense values. This is also just an example to get you started. This dataframe should be filled with the values you scrape from the website.
- The DataFrame is written to the csv file: data/movies.csv
