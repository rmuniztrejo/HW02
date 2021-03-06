What went wrong?
================
Robert Gruener

``` r
knitr::opts_chunk$set(echo = TRUE, error = TRUE)
```

## HW02 Part A

In this document, I will add some examples of some coding mistakes, it
is up to you to figure out why the graphs are messing up.

### First load packages

It is always best to load the packages you need at the top of a script.
It’s another common coding formatting standard (like using the
assignment operator instead of the equals sign). In this case, it helps
people realize what they need to install for the script and gives an
idea of what functions will be called.

It is also best coding practice to only call the packages you use, so if
you use a package but end up tossing the code you use for it, then make
sure to remove loading it in the first place. For example, I could use
`library("tidyverse")` but since this script will only be using ggplot2,
I only load ggplot2.

``` r
library("ggplot2")
```

    ## Warning: package 'ggplot2' was built under R version 4.0.2

``` r
library("magrittr") #so I can do some piping
```

    ## Warning: package 'magrittr' was built under R version 4.0.2

### Graph Fail 1

What error is being thrown? How do you correct it? (hint, the error
message tells you)

##### Answer 1

**The error being thrown out is:**

\#\# Error: `mapping` must be created by `aes()` \#\# Did you use %\>%
instead of +?

**The problem in this graph was that we were using** %\>% **instead of**
`+` **(very self-explanatory) inside the** `ggplot()` **function.**

``` r
data(mpg) #this is a dataset from the ggplot2 package

mpg %>%
ggplot(mapping = aes(x = cty, y = hwy, color = "blue")) +
  geom_point()
```

![](HW02_A_Graph-Fails_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

### Graph Fail 2

Why aren’t the points blue? It is making me blue that the points in the
graph aren’t blue :\`(

##### Answer 2

**Because blue here is being used inside the** `aes()` **function, R
interpreted it as a variable of the graph (which is why appears inside
the legend), but “blue” is not a vector within mpg, so it cannot be
shown in the plot. Instead, the** `color = "blue"` **argument should be
specified outside** `aes`**.**

``` r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy), color = "blue")
```

![](HW02_A_Graph-Fails_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

### Graph Fail 3

Two mistakes in this graph. First, I wanted to make the the points
slightly bolder, but changing the alpha to 2 does nothing. What does
alpha do and what does setting it to 2 do? What could be done instead if
I want the points slightly bigger?

Second, I wanted to move the legend on top of the graph since there
aren’t any points there, putting it at approximately the point/ordered
pair (5, 40). How do you actually do this? Also, how do you remove the
legend title (“class”)? Finally, how would you remove the plot legend
completely?

##### Answer 3

`alpha` **specifies the transparency of the color by a proportion, and
it can only take values between 0 and 1. By putting 2 you are not doing
anything really, because something can’t be 200% solid color. If you
want bigger points, you can increase their size. I enter** `size = 5`
**just to exaggerate my point (got it?** :wink:**).**

**It is important to remember that in a numeric vector to specify the
position of the legend** `c(x,y)`**, the values must be between 0-1,
being c(0,0) the bottom left corner, and c(1,1) the top right corner.
c(5,40) does not lead to any position within the graph.**

**To remove the legend title, one must indicate the argument**
`legend.title =` **and call** `element_blank()`**.**

**Lastly, to remove the legend completely, you should write**
`legend.position = "none"`**.**

``` r
mpg %>% 
ggplot() + 
  geom_point(mapping = aes(x = displ, y = hwy, color = class), size = 5) +
  theme(legend.direction = "horizontal",
        legend.position = c(0.7,0.9),
        legend.title = element_blank()) #+
```

![](HW02_A_Graph-Fails_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

``` r
  #If you want to remove the legend
# theme(legend.position = "none")
```

### Graph Fail 4

I wanted just one smoothing line. Just one line, to show the general
relationship here. But that’s not happening. Instead I’m getting 3
lines, why and fix it please?

##### Answer 4

**The problem here was that we were using drv as a color aesthetic, so
ggplot was interpreting every color as a different set of data, not just
one, and because of that several smooth lines were drawn. To correct it,
you must quit drv from the** `aes()` **function.**

**Now, if you want to keep the color scheme according to drv without
losing the unique smooth line, you can add the color as an exclusive
attribute of** `geom_point()` **as shown in the code.**

``` r
mpg %>% 
ggplot(mapping = aes(x = displ, y = hwy)) + 
  geom_point(aes(color = drv)) + 
  geom_smooth(se = F) #se = F makes it so it won't show the error in the line of fit +
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](HW02_A_Graph-Fails_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

### Graph Fail 5

I got tired of the points, so I went to boxplots instead. However, I
wanted the boxes to be all one color, but setting the color aesthetic
just changed the outline? How can I make the box one color, not just the
outline?

Also, the x-axis labels were overlapping, so I rotated them. But now
they overlap the bottom of the graph. How can I fix this so axis labels
aren’t on the graph?

##### Answer 5

`Color` **argument only affects the outline of the figure, you need
the** `fill` **argument to add color to the entire box.**

**To be honest, I’m not sure if there’s a better solution, but one way
to avoid overlapping is by increasing the length of the ticks to move
the labels below the graph lower limit.**

``` r
ggplot(data = mpg, mapping = aes(x = manufacturer, y = cty, fill = manufacturer)) + 
  geom_boxplot() + 
  theme(axis.text.x = element_text(angle = 45),
        axis.ticks.length.x = unit(0.8, "line")) 
```

![](HW02_A_Graph-Fails_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->
