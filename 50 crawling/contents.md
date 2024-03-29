# Crawling

We have done what we set out to do. But we might ask ourselves some additional questions. Like; which actors were the most influential? Additionally, when we look at the language spoken in the top films, we might assume that English is very dominant. But how much so compared to other languages? And has this always been the same? Or does this depend on the decade we consider?

The problem, our current dataset doesn't have any actor or language information. We'll have to add that by doing some web crawling.

## Web crawling

The file `top5.csv` you have generated before doesn't contain any actor or language information. The main problem is that this information is not present on the IMDB page that we have used. So we'll have to get it some other way. We did store the URL's for the webpage of each movie in the `. csv` file. This links to a page with much more detailed information about each movie. This includes information on the movies' "stars" and language.

Your goal is to write a program called `crawler.py` that looks up this information and creates an augmented dataset.

Usage:

    $ python crawler.py data/top5.csv data/top5-actors-languages.csv

This will take the data from `top5.csv`, use the URL's to read the page for each movie and retrieve the languages spoken in that movie. It will write the results to `top5-actors-languages.csv`. So this will look something like this:

    title,rating,year,actors,languages,runtime,url
    emlya,7.3,1930,Stepan Shkurat;Semyon Svashenko;Yuliya Solntseva;Yelena Maksimova,None;Russian,75,https://www.imdb.com/title/tt0021571/
    Animal Crackers,7.5,1930,Groucho Marx;Harpo Marx;Chico Marx;The Marx Brothers,English,97,https://www.imdb.com/title/tt0020640/
    All Quiet on the Western Front,8.1,1930,Lew Ayres;Louis Wolheim;John Wray;Arnold Lucy,English;French;German;Latin,152,https://www.imdb.com/title/tt0020629/

    ...

    Kumbalangi Nights,8.6,2019,Shane Nigam;Soubin Shahir;Fahadh Faasil;Sreenath Bhasi,Malayalam,135,https://www.imdb.com/title/tt8413338/
    Jersey,8.6,2019,Nani;Shraddha Srinath;Sathyaraj;Harish Kalyan,Telugu,157,https://www.imdb.com/title/tt8948790/
    Kaithi,8.5,2019,Karthi;Narain;Ramana;George Maryan,Tamil,145,https://www.imdb.com/title/tt9900782/

Notice the additional columns `actors` and `languages`. Often a movie has more than one actor or language. They are separated by a semicolon `;`.

To aid you in this process (and speed it up significantly) we have provided you with a couple of functions in `helpers.py` which you might have spotted before:

- `get_multiple_page_contents(url_list)`: this function retrieves the page content for a list of URLs and stores them in a folder named "webpages". If an URL was already downloaded, it will skip this one. Retrieving all your movies might still take a couple of minutes (which you'll luckily only have to do once!). Note that `get_multiple_page_contents()` will sometimes produce empty files. It will always state why this happened, and sometimes simply re-running the function could solve the problem.
- `read_page_content_from_disk(url)`: this function will retrieve the page content for a single URL from disk.

> `get_multiple_page_contents()` retrieves multiple pages at the same time. If it is still too slow or you think your computer can take more, increase the value of `processes`. If your computer struggles a bit, you can decrease the value of `processes`.

### Requirements

- The program accepts two positional command line arguments: the input file and the output file. You should use `argparse` for this.
- The program reads the data from the input file into a pandas DataFrame.
- The program uses `get_multiple_page_contents()` from `helpers.py` to retrieve the content for a list of URLs and stores the htmls in the folder "/webpages".
- The program then uses `read_page_content_from_disk()` to get the html from disk one by one, finds the actors listed under "stars" (often only 3 or 4 actors), and the languages spoken in that movie. Store these values in the DataFrame. This html can be converted to a DOM using BeautifulSoup.
- The result is written as a `.csv` to the output file.
- If a movie has more than one actor or language, they should be separated by a semicolon.
- The resulting output `.csv` should be sorted by year (ascending)

### Hints

* Have a look at the `find()` function in the documentation of BeautifulSoup4 and look for the CSS selectors, they will make this exercise much easier!
* The script will be not be fast. Each page has a lot of text to get through, and there will be hundreds of pages. This is not a problem. But, for your own convenience, **create a small test input file with only two or three movies**, to see if it's working correctly before you run the script for the entire dataset.
* There is at least one movie that has no language listed at all ([this one](https://www.imdb.com/title/tt2185022/)). Think of a way that makes sure your program doesn't crash when your Beautifulsoup `find()` doesn't find any languages. You could test this movie explicitly in your small test input file!
