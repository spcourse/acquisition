## Visualizing Languages

We're going to visualize the language representation in top rated movies, to answer the question:

How did language influence in the top rated movies change over the last 9 decades?

Write a program called `visualize_languages.py` that generates a line plot of **the top 10 languages** (that occur the most in our dataset) over time.

Usage:

    $ python visualize_languages.py data/top5-actors-languages.csv plots/languages.png

The program has the following requirements:

- It should accept two command line arguments. The first argument is the location of the data file, the second argument is the location of the output file. So with the call above the program will generate a plot `languages.png` in the directory `plots`, based on the data of `top5-actors-languages` in the directory `data`.
- It should read the input into a pandas DataFrame.
- It should output a `.png` file containing a line plot with one line per language. This can be done using matplotlibs [savefig](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.savefig.html) method.
- The plot should have a legend making clear which line corresponds to which language.
- It should show the number of occurrences of each language *per decade*. (Each occurrence counts equally, whether it's the first language or the last language for a movie).
- The horizontal axis of the plot should have the decades (1930s to 2010s), the vertical axis should have the number of occurrences.

### Hints

- Keep in mind that there is at least one movie for which there is no language available, and that a couple of movies also have one of their languages listed as `'None'`.

### Question 2

(write your answers in a document called `answers.txt`)

Looking at your plot, which was the second most influential language in the 1970s?


### Question 3

(write your answers in a document called `answers.txt`)

And which language was the second most influential in the 2010s?

## Done
