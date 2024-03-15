# Crawling

We have done what we set out to do. But we might ask ourselves some additional questions. Like; which actors were the most influential? Additionally, when we look at the language spoken in the top films, we might assume that English is very dominant. But how much so compared to other languages? And has this always been the same? Or does this depend on the decade we consider?

The problem is that our current dataset doesn't have any actor or language information. We'll have to add that by doing some web crawling.

## Web crawling

The file `top5.csv` you have generated before doesn't contain any actor or language information. The main problem is that this information is not present on the IMDB pages that we have currently scraped. We'll have to get it some other way. We did store the URL's for the webpage of each movie in the `.csv` file. This links to a page with much more detailed information about each movie. This includes information on the movies' "stars" and language.

Your goal is to write a program called `crawler.py` that looks up this information and creates an augmented dataset.

Usage:

    $ python crawler.py data/top5.csv data/top5-actors-languages.csv

Your program will take the data from `top5.csv`, use the URL's to read the page for each movie and retrieve both the lead actors the languages spoken in that movie. It will write the results to `top5-actors-languages.csv`. Which will look something like this:

    title,rating,year,actors,languages,runtime,url
    All Quiet on the Western Front,8.1,1930,Lew Ayres;Louis Wolheim;John Wray,English;French;German;Latin,152,https://www.imdb.com/title/tt0020629/
    The Blue Angel,7.7,1930,Emil Jannings;Marlene Dietrich;Kurt Gerron,German;English;French,104,https://www.imdb.com/title/tt0020697/
    Animal Crackers,7.4,1930,Groucho Marx;Harpo Marx;Chico Marx,English,97,https://www.imdb.com/title/tt0020640/

    ...

    Soorarai Pottru,8.7,2020,Suriya;Paresh Rawal;Urvashi,Tamil,153,https://www.imdb.com/title/tt10189514/
    Attack on Titan: Chronicle,8.4,2020,Yûki Kaji;Yui Ishikawa;Marina Inoue,Japanese,122,https://www.imdb.com/title/tt12415546/
    Hamilton,8.3,2020,Lin-Manuel Miranda;Phillipa Soo;Leslie Odom Jr.,English,160,https://www.imdb.com/title/tt8503618/

Notice the additional columns `actors` and `languages`. Often a movie has more than one actor or language. They are separated by a semicolon `;`.

To aid you in this process (and speed it up significantly) we have provided you with a couple of functions in `helpers.py` which you might have spotted before:

- `get_multiple_page_contents(url_list)`: this function retrieves the page content for a list of URLs and stores them in a folder named "webpages". If an URL was already downloaded, it will skip this one. Retrieving all your movies might still take a couple of minutes (which you'll luckily only have to do once!). Note that `get_multiple_page_contents()` will sometimes produce empty files. It will always state why this happened, and sometimes simply re-running the function could solve the problem.
- `read_page_content_from_disk(url)`: this function will retrieve the page content for a single URL from disk. Returns the content of a file. _Note: this is not yet a DOM!_
- `get_movie_actors(dom)`: this function will extract the lead actors in a movie from a given DOM. Returns a string with the lead actors in the movie separated by semicolons.
- `get_movie_languages(dom)`: this function will extract the languages spoken in a movie from a given DOM. Returns a string with all languages spoken in the movie separated by semicolons.

> `get_multiple_page_contents()` retrieves multiple pages at the same time. If it is still too slow or you think your computer can take more, increase the value of `processes`. If your computer struggles a bit, you can decrease the value of `processes`.

### Requirements

- The program accepts two positional command line arguments: the input file and the output file. You should use `argparse` for this.
- The program reads the data from the input file into a pandas DataFrame.
- From this DataFrame, a list of URLs can be extracted.
- The program then uses `get_multiple_page_contents()` from `helpers.py` to retrieve the content for this list of URLs and stores the htmls in the folder "/webpages".
- The program then uses `read_page_content_from_disk()` to get the html from disk one by one. This html can be converted to a DOM using BeautifulSoup. For each DOM find the actors listed under "stars" using `get_movie_actors()` (often only 3 or 4 actors), and the languages spoken in that movie using `get_movie_languages()`. Store these values in the DataFrame.
- The result is written as a `.csv` to the output file.
- The resulting output `.csv` should still be sorted by year (ascending) and rating (descending)

> The script will be not be fast. Each page has a lot of text to get through, and there will be hundreds of pages. This is not a problem. But, for your own convenience, **create a small test input file with only two or three movies**, to see if it's working correctly before you run the script for the entire dataset.

## Done

In the next steps we will visualize the actors and languages of all our top movies.
