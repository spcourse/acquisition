# Transforming

We're on our way to answer the questions:

- *Which actors were the most influential?*
- *Which languages were the most influential?*

For most years the current data contains many more movies than just the top 5. So we should first do a transformation.

## Goal

Create a new program called `extract.py`. The idea of the program is that you can call it like this:

    $ python extract.py data/movies.csv data/top5.csv

This will create a new file, `top5.csv` in the directory `data` that contains a subset from `movies.csv`.

The program has the following requirements:

- It accepts two positional command line arguments the input file and the output file. You should use `argparse` for this. _Check out how we did that in `scraper.py`!_
- The program reads the data from the input file into a pandas DataFrame.
- It will filter the DataFrame such that it only contains the top N (default is 5) movies from every year.
- It should accept an optional argument `-n` that will allow us to select a lower `N`, so that we could also use the program to generate a top 4 or lower. So we could in principle use the following command to create a top 3: `$ python extract.py -n 3 data/movies.csv data/top3.csv`
- The result is written as a `.csv` to the output file.
- The resulting output `.csv` should be sorted by year (ascending), and then rating (descending)

Don't forget to keep an eye on code design. Use functions, choose useful names for your variables, prevent code repetition, place comments, etc.

## Checking your answer

You can apply some common sense testing to see if your answer is correct:

- How many lines do you expect the file to have?
- Does the top 5 in your file correspond to what you find on IMDB?
- Are the movies sorted properly? Both on year **and** then by rating?
- Does the optional command line argument `-n` work properly? Ask yourself the questions above for different values of N.

## Done

In the next step you'll gather some more data that we need to answer our research questions.
