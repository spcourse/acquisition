# Visualizing the data

We're still trying to answer the question:

What year had the best average IMDB rating for it's top 5 movies?

In the previous part you wrote a script to produce a csv file `top5.csv`. It is time to try to get some insights into what we scraped.

### Goal

For this you will be writing a new file called `visualize_years.py`. Your aim in this part of the exercise is to visualize the data scraped from IMDB in a appropriate chart. You will have to generate a bar-plot `years.png` that show the average rating of the top 5 movies per year (from 1930 to 2020). Once the data is in the correct format, you can plot it using Matplotlib. If you've never used Matplotlib have a look at [this tutorial](https://matplotlib.org/users/pyplot_tutorial.html).

The program should run like this:

    $ python visualize_years.py data/top5.csv plots/years.png


The program has the following requirements:

- It should accept two command line arguments. The first argument is the location of the data file, the second argument is the location of the output file. So with the call above the program will generate a plot `years.png` in the directory `plots`, based on the data of `top5` in the directory `data`.
- It should read the input into a pandas DataFrame.
- It should output a `.png` file containing a bar plot. This can be done using matplotlibs [savefig](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.savefig.html) method.
- The horizontal axis of the plots should have the years (from 1930 to 2020), the vertical axis should have the average rating of the top 5 movies for that year.

Reflect on the figure your script creates. How do you choose the range of the y-axis? Are all elements present in the figure?

Don't forget to keep an eye on code design. Use functions, choose useful names for your variables, prevent code repetition, place comments, etc.

### Question 1

(write your answers in a document called `answers.txt`)

Looking at your plot, which year was the best year for movies?

## Done

In the next assignment we're going to look at the representation of languages and actors in the top movies. This is very similar to what we've done so far, but will require an additional step (web crawling).
