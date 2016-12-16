


# Context

This section will introduce you to the software, data set and plotting functions that you will need to use for this chapter's challenge.

<br>

---
 
**Table of Contents**

<!-- toc -->

<br>

---

## Installing R and RStudio

To interact with R, we'll be using RStudio: a free, open source R integrated development environment. You will need to download and install both [R](https://cran.ms.unimelb.edu.au/) and [RStudio](https://www.rstudio.com/products/rstudio/download/). After you have done this, open RStudio to use R (you don't need to open R separately).

## The RStudio interface

When you first open RStudio, you will be greeted by three panels:

* The interactive R **console** (entire left)
 
 The console is used to write instructions or lines of code to the computer, such as read in data, or plot data. These instructions will be performed by the computer as soon as you press the ENTER key. Beware that the instructions you type into the console will not be saved for later use.

* Environment/History (tabbed in upper right)

 In this panel you will find a record of all the instructions you have performed in the console panel, as well as all of the things that you have created with your code (when you first open RStudio, both the environmnet and history should be empty).

* Files/**Plots**/Packages/Help/Viewer (tabbed in lower right)

 This panel is where your plots will appear and where you can find help files.

Once you open R files, an **editor** panel will also open in the top left. Here you can write instructions to the computer that will be saved. This is much like saving text in a Word document for later use. You can create a new R document by going to File > New File > R Script. In order to get the computer to perform instructions from a file that you have open in the editor panel, you need to highlight the code with your cursor and press the Run button (circled in blue below), or hold down CTRL + ENTER.

![RStudio interface](images/RStudio_interface.png)

<br>

## The titanic dataset

In this chapter we will be looking at survival rates of people on board the Titanic. Let’s see if we can work out who had the highest survival rate! Was it really women and children first? The dataset has 11 different columns, describing the characteristics of each of the people known to have been on board the Titanic. We are interested in 3 of these columns: 

1. Sex - were they female or male? 
2. Passenger class (PClass) - were they in 1st, 2nd or 3rd class? 
3. Survived - did they survive the titanic? 

![The Titanic](images\titanic.jpg)

Download the [code file for the challenge here](plottingChallenge.R). The code should appear in the Editor panel when you open this file in RStudio. 

Let’s take a look at the code (don't worry, you don't need to be able to reproduce this!). The first thing to note is that lines beginning with a hash are comments or notes to ourselves. This makes the code easier for both your future self and others to read but is otherwise ignored by the computer (always be kind to your future self!). 

The code is: importing a data set, manipulating the data set and then plotting the data. The first line of code imports the data from a csv file stored online. 


~~~sourcecode
titanic <- read.csv("https://goo.gl/4Gqsnz")
~~~

You don’t need to worry too much about the next two lines which are manipulating the data. In brief, the second line of code tabulates the data, counting the number of people in each combination of gender and passenger class that either survived or did not survive. How many females in first class survived? How many did not survive? How many males in first class survived? How many did not survive? Etc. The third line of code converts this into a percentage of people who survived for each group. What percentage of the females in first class survived? What percentage of the males in second class survived? 


~~~sourcecode
counts <- table(titanic$Sex, titanic$Pclass, titanic$Survived)
percentageSurvival <- counts[,,2] / (counts[,,1] + counts[,,2]) * 100
~~~

The final line of code is the one that we’re most interested in. This is where we plot survival rates as a barplot. 


~~~sourcecode
barplot(percentageSurvival, beside = TRUE)
~~~

<img src="images/unnamed-chunk-4-1.png" title="plot of chunk unnamed-chunk-4" alt="plot of chunk unnamed-chunk-4" style="display: block; margin: auto;" />

The y-axis represents the percentage that survived. The x-axis shows the different passenger classes. The coloured bars represent genders: the black bars represent females and the grey bars represent males. So it does look like women had a better chance of surviving than men. And not surprisingly, the first class passengers had higher survival rates than the other passengers. 

## Plotting

Let’s take a closer look at the line of code for plotting. 


~~~sourcecode
barplot(percentageSurvival, beside = TRUE)
~~~

We have told R that we would like a barplot. The brackets are important. Everything within the brackets describes the characteristics of the plot. These are called arguments. Each argument is separated from the next argument by a comma. The first argument tells R which data to include in our barplot, which is the survival rates for the various passenger classes and genders (`percentageSurvival`). The second argument tells R not to stack the bars but to place them side by side (`beside = TRUE`). So the genders are plotted with females next to males, rather than having one gender stacked on top of the other. We’re all about gender equality at Research Platforms!

Next we will look at 3 different ways of modifying the plot: changing the bar colour, changing the background colour and adding a plot title.

## Modifying the bar colours in a barplot

Let's make the bars red. We can do this using the `col` argument (it's important to include the quotation marks around the colour name).


~~~sourcecode
barplot(percentageSurvival, beside = TRUE, col = "red")
~~~

<img src="images/unnamed-chunk-6-1.png" title="plot of chunk unnamed-chunk-6" alt="plot of chunk unnamed-chunk-6" style="display: block; margin: auto;" />

Hmm, just having the one colour is not that informative, given that we have two different genders. So we should probably choose two different colours. Let's try red and green (at the time of writing this chapter it was getting close to Christmas, which may explain my colour choices!). We can add the extra colour by adding a comma and a c up the front, which stands for combine, so we are combining these two colours for the one (`col`) argument. 


~~~sourcecode
barplot(percentageSurvival, beside = TRUE, col = c("red", "green"))
~~~

<img src="images/unnamed-chunk-7-1.png" title="plot of chunk unnamed-chunk-7" alt="plot of chunk unnamed-chunk-7" style="display: block; margin: auto;" />

<!--sec data-title="Tip: Choosing a colour" data-id="tip1" data-show=true data-collapse=true ces-->

To check what colours are available, type in colors() (American spelling) in the console and press ENTER. This will return a list of all possible colours.

<!--endsec-->

## Modifying the background colour of plots

Now let's change the background colour to `lightblue`. This is a little more complicated, since the background colour is changed globally (for all plots) rather than in our specific barplot. The way we do this is to write a line of code - before the line of code to plot - which sets the global plotting parameters. Given the the colour argument is `col`, it shouldn't surprise you that for parameters we use `par`. Arguments within the round brackets describe the global plotting characteristics. The argument `bg` is used for the background colour. 


~~~sourcecode
par(bg = "lightblue")
barplot(percentageSurvival, beside = TRUE, col = c("red", "green"))
~~~

<img src="images/unnamed-chunk-8-1.png" title="plot of chunk unnamed-chunk-8" alt="plot of chunk unnamed-chunk-8" style="display: block; margin: auto;" />

## Adding a title to a plot

We can use the `main` argument to add a title to this plot (don't forget the comma beforehand). Let's call the plot 'Survival rates for Titanic passengers'. 


~~~sourcecode
par(bg = "lightblue")
barplot(percentageSurvival, beside = TRUE, col = c("red", "green"), main = "Survival rates for Titanic passengers")
~~~

<img src="images/unnamed-chunk-9-1.png" title="plot of chunk unnamed-chunk-9" alt="plot of chunk unnamed-chunk-9" style="display: block; margin: auto;" />

<!--sec data-title="Tip: Spreading your code over multiple lines" data-id="tip2" data-show=true data-collapse=true ces-->

The above code is becoming difficult to read with all 4 arguments on the same line. We can spread the code along multiple lines (as long as we don't close the brackets before we reach the end of the code). Writing the code across additional lines makes no difference to the computer, but can make code much easier to read for humans!


~~~sourcecode
par(bg = "lightblue")
barplot(percentageSurvival, 
        beside = TRUE,
        col = c("red", "green"), 
        main = "Survival rates for Titanic passengers")
~~~

<img src="images/unnamed-chunk-10-1.png" title="plot of chunk unnamed-chunk-10" alt="plot of chunk unnamed-chunk-10" style="display: block; margin: auto;" />

<!--endsec-->