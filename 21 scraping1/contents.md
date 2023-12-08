## Step 1: Extracting data from a single page

### Goal

First, implement the `extract_movies()` function. It should extract a `DataFrame` containing the highest rated movies from the passed DOM (which is of an [IMDB page](https://www.imdb.com/search/title/?title_type=feature&release_date=1930-01-01,2020-01-01&num_votes=5000,&sort=user_rating,desc&start=1&view=advanced)). Each row in the `DataFrame`  should be of a single movie. The columns should contain the following data:

  - Title
  - Rating
  - Year of release (only a number!)
  - Runtime (only a number!)
  - URL (a URL to the landing page for that film)

### Example

When we run your program:

    $ python scraper.py data/top50.csv

It should produce a file called `top50.csv` in the directory `data` containing the following data:

    title,rating,year,runtime,url
    Hababam Sinifi,9.3,1975,87,https://www.imdb.com//title/tt0252487/
    The Shawshank Redemption,9.3,1994,142,https://www.imdb.com//title/tt0111161/
    Aynabaji,9.2,2016,147,https://www.imdb.com//title/tt5354160/

    ...

    The Matrix,8.7,1999,136,https://www.imdb.com//title/tt0133093/
    Lepa sela lepo gore,8.7,1996,115,https://www.imdb.com//title/tt0116860/

(in total it should contain 51 lines)

You can cross check the output with the information on the [IMDB page](https://www.imdb.com/search/title/?title_type=feature&release_date=1930-01-01,2020-01-01&num_votes=5000,&sort=user_rating,desc&start=1&view=advanced).

### Hints

#### HTML

`print()` is probably going to be your best friend for debugging, so print often, especially if something goes wrong.

Take a look at the following attributes, taken from the BeautifulSoup documentation, that show some basic functionalities of BeautifulSoup.

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

- Go to the webpage in your favorite browser.
- Right click on the element in which you are interested.
- Click 'Inspect element'.

This will open the browser's inspector functionality which shows you the source HTML of the element you have clicked. Hovering over this HTML source will show the corresponding webpage element.

Also have a look at the `find()` function in the documentation of BeautifulSoup4 and look for the CSS selectors, they will make this exercise much easier!

#### Regex

You might need to filter out some characters from a string. One method to do this is through the use of [Regular Expressions]. After importing `re`, `re.findall` can be used to find all occurrences in a string, while `re.search` can be used to find the first occurrence. Keep in mind that the resulting type after these Regular Expressions is still a string!

    >>> import re
    >>> re.findall(r'\d+', '123 dogs jumped the fence and ate over 4400 sheep!')
    ['123', '4400']
    >>> re.search(r'\d{4}', '123 dogs jumped the fence and ate over 4400 sheep!').group()
    '4400'

[Regular Expressions]: https://www.w3schools.com/python/python_regex.asp


#### Use comments!

Keep in mind that the use of BeautifulSoup and RegEx will not necessarily result in _beautiful_ code. In fact, data collection through scraping (as well as taking ingredients out of a soup) is known to be a very messy practice. You might need to heavily comment code to make sure that it is understandable.
