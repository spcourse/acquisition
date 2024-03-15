## Visualizing Actors

Now that we have the data crawled and scraped, we might ask ourselves some additional questions.

Which actor was most influential between 1930 and 2020?

Again, we'll reformulate the question so we can give some sort of answer given our data:

Which actor has appeared the most in the top 5 movies of all the years between 1930 and 2020?

### Goal

Write a program called `visualize_actors.py` that generates a bar plot of the top 50 actors with the most appearances in our dataset.

Usage:

    $ python visualize_actors.py data/top5-actors-languages.csv plots/actors.png

The program has the following requirements:

- It should accept two command line arguments. The first argument is the location of the data file, the second argument is the location of the output file. So with the call above the program will generate a plot `actors.png` in the directory `plots`, based on the data of `top5` in the directory `data`.
- It should read the input into a pandas DataFrame.
- It should output a `.png` file containing a bar plot. This can be done using matplotlib's [savefig](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.savefig.html) method.
- The horizontal axis of the plots should have the actors (only the 50 actors with the most appearances), the vertical axis should have the number of appearances of that actor. Printing 50 names vertically might make your plot illegible, try to change this so that its clear what bar belongs to what actor.
- The bar plot should be sorted (highest first).

### Question 1

(write your answers in a document called `answers.txt`)

Looking at your plot, which actor was the most influential?
