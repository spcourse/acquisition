# Scraping

Web scraping is a way of extracting data from websites. While this process could be done manually (by reading information on a website, and then copying that information to a file) it is usually done through the use of software. Scraping can be a valuable tool for extracting data. A website might not give you an option to download the content, either through an API or a direct download link. One example of such a website is the IMDB page that we will be scraping in this exercise.

In this assignment you will learn to use the Document Object Model (DOM) using Python via the BeautifulSoup library. We provide some scaffolding for the programming exercise. We will be trying to answer the question: Between 2008 and now, were there any years in which movies (from the top 50) scored significantly higher than in other years? With years we mean the year the movie was released.

* The IMDB highest rated movies exercise: [moviescraper.py]

[moviescraper.py]: moviescraper.py

| Pipeline step          	| This week                                                                        	         |
|------------------------	|------------------------------------------------------------------------------------------- |
| Asking a Question      	| Were there any years in which movies (from the top 50) scored higher than in other years?  |
| Acquiring the data     	| Scraping IMDB to CSV                                                                       |
| Transforming the data 	| During scraping, and in calculating averages in visualizer.py                              |
| Visualizing the data  	| Plotting the average scores visualiser.py                                                  |
| Analyzing the data    	| Using your final product: a line chart                                         	           |

## Before starting

1. For this assignment you'll need to have the following Python 3 libraries installed: Requests, Pandas, Seaborn, and BeautifulSoup4.

2. We will be looking at IMDB movies and getting data off this website. To get started, you should look at [the BeautifulSoup documentation].

3. To get you started, we have provided you with a script ([moviescraper.py]) that loads the correct IMDB address, makes a local backup of it (`movies.html`) and outputs a CSV file (`movies.csv`) that will contain only a header until you complete the implementation of the functions `extract_movies(dom)` and `save_csv(outfile, movies)`. Note that if you want to check your `movies.csv` file in Excel on a Mac, add 'sep=,' as the first line of the file. This way the file is parsed correctly in columns.

4. To help you validate your script, we provide an example output CSV file [output.csv]. You should run the `moviescraper.py` script with the command `python moviescraper.py`. This will copy the IMDB webpage to the current directory and save a CSV file. The example output file only contains the first two movies.

5. When you hand in this exercise, be sure to submit your `moviescraper.py`, `visualizer.py`, `movies.html`, and `movies.csv`. This will allow us to verify that your output CSV file is correct and that the script actually works given the HTML from IMDB.

6. It could be that there are missing data (for instance the runtime). Insert an appropriate value when something is missing. Note that the director is not an actor or actress.

[output.csv]: output.csv
[the BeautifulSoup documentation]: https://www.crummy.com/software/BeautifulSoup/bs4/doc/

## DOM scraping and traversal

A webpage is a document. This document can be either displayed in the browser window or as the HTML source. The Document Object Model (DOM) represents that same document so it can be manipulated. The DOM is an object-oriented representation of the webpage and can be used as a programming interface for HTML and XML documents. It represents the page so that programs can change the document's structure, style, and content.

To scrape data from webpages, we will be using BeautifulSoup, a Python web mining module. Its description is as follows: _Beautiful Soup is a Python library for pulling data out of HTML and XML files. It works with your favorite parser to provide idiomatic ways of navigating, searching, and modifying the parse tree. It commonly saves programmers hours or days of work._

This is the introductory exercise to BeautifulSoup. We will try to guide you along as much as possible, but you should read up on documentation and get used to doing that. It's a really useful skill and a big part of programming is self-­learning! Watch the following videos on HTML and the DOM. Ignore any reference to JavaScript (you may replace it in your mind with BeautifulSoup), as it will not be needed in our exercises.

![embed](https://www.youtube.com/embed/YK78KhMf7bs?t=68)

![embed](https://www.youtube.com/embed/GBKwdFEyJks)

![embed](https://www.youtube.com/embed/ng2o98k983k)

### Building `moviescraper.py`

To get you started, we have provided you with a script ([moviescraper.py]) that loads the correct IMDB address, makes a local backup of it (`movies.html`) and outputs a CSV file (`movies.csv`) that will contain only a header until you complete the implementation of the functions `extract_movies(dom)` and `save_csv(outfile, movies)`. Note that if you want to check your `movies.csv` file in Excel on a Mac, add 'sep=,' as the first line of the file. This way the file is parsed correctly in columns.

<!--> De alinea hierboven staat ook al op regel 25 <!-->

Open up and read the `moviescraper.py`-file. Note that this is just some scaffolding, so you actually don't have to use this at all. As long as your code runs at the end of the day and produces the right results in a CSV file, we're happy.

First, implement the `extract_movies(dom)` function. It should extract a list of the highest rated movies from the passed DOM (which is the IMDB page). Each movie entry should be a dictionary that contains the following fields:
  - Title
  - Rating
  - Year of release (only a number!)
  - Actors/actresses (comma separated if more than one)
  - Runtime (only a number!)

You might need to filter out some characters from a string. _Especially retrieving multiple actors will become very difficult with BeautifulSoup only._ One method to do this is through the use of [Regular Expressions]. After importing `re`, `re.findall` can be used to find all occurances in a string, while `re.search` can be used to find the first occurence. Keep in mind that the resulting type after these Regular Expressions is still a string!

    >>> import re
    >>> re.findall(r'\d+', '123 dogs jumped the fence and ate over 4400 sheep!')
    ['123', '4400']
    >>> re.search(r'\d{4}', '123 dogs jumped the fence and ate over 4400 sheep!').group()
    '4400'

[Regular Expressions]: https://www.w3schools.com/python/python_regex.asp

Then, implement the `save_csv(outfile, movies)` function. It should write the list of the highest rated movies (`movies`) to `outfile`.

### Hints for scraping

`print()` is probably going to be your best friend for debugging, so print often, especially if something goes wrong.

Take a look at the following attributes, taken from [the BeautifulSoup documentation], that show some basic functionalities of BeautifulSoup.

        soup.title
        # <title>The Dormouse's story</title>

        soup.title.name
        # u'title'

        soup.title.string
        # u'The Dormouse's story'

        soup.title.parent.name
        # u'head'

        soup.p
        # <p class="title"><b>The Dormouse's story</b></p>

        soup.p['class']
        # u'title'

        soup.a
        # <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>

        soup.find_all('a')
        # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
        #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
        #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

        soup.find(id="link3")
        # <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>

An easy method to finding what class, tag, or id to target with BeautifulSoup is:
- Go to the webpage in your favorite browser
- Right click on the element you are interested in
- Click 'Inspect element'

This will open the browser's inspector functionality which shows you the source HTML of the element you have clicked. Hovering over this HTML source will show the corresponding webpage element.

Also have a look at the `find()` function in the documentation of BeautifulSoup4 and look for the CSS selectors, they will make this exercise much easier!

> Keep in mind that the use of BeautifulSoup and RegEx will not necessarily result in _beautiful_ code. In fact, data collection through scraping is known to be a very messy practice. You might need to heavily comment code to make sure that it is understandable. **Especially** retrieving actors and directors will be very messy.

## Visualizing the data

Now that we have the data in a `.csv`-format, it is time to try to get some insights into what we scraped. For this you will be using [visualizer.py].

[visualizer.py]: visualizer.py

For now, `visualizer.py` is mostly empty. Your aim in this part of the exercise is to visualize the data scraped from IMDB in an appropriate chart. Find the average rating a movie in the top 50 of IMDB has gotten for the years 2008-now. Our aim is to answer the question: "Were there any years in which movies (from the top 50) scored higher than in other years?". Potentially interesting things to look at for this part of the exercise are the [Pandas read csv method] and [Seaborn]. If you feel that there is perhaps a better way to answer our research question using your data, feel free to **also** create any other plots.

[Pandas read csv method]: https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html
[Seaborn]: https://seaborn.pydata.org/api.html

Once the data is in the correct format, you can plot it using Matplotlib or Seaborn. You can read [this tutorial] on some of the basics of plotting data using Matplotlib. Of course, there is much more documentation to be found online, which can either help you on your way with plotting the data, or make your already existing plot more aesthetically pleasing or more effective.

[this tutorial]: https://matplotlib.org/users/pyplot_tutorial.html

Reflect on the figure your script creates. How do you choose the range of the y-axis? Are all elements present in the figure? Think about the consequences of the design choices you make.

Don't forget to keep an eye on code design. Use functions, choose useful names for your variables, prevent code repetition, place comments, etc.

## Finished?

If you have finished the scraper and visualization, you can continue to the next exercise: [Crawler]

If you have finished far before the deadline and are itching to gather more data, you can have a look at the [Pandas] library to see what kind of things you can do with the data gathered using the scraper, that are beyond the scope of this exercise.

[Crawler]: /acquisition/crawling
[Pandas]: https://pandas.pydata.org/
