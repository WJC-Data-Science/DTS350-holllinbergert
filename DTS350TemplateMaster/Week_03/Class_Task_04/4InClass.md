---
title: "4InClass"
author: "TomHollinberger"
date: "9/8/2020"
output: 
  html_document: 
    keep_md: yes
    toc: TRUE
---


#Day4Examples: ggplot2
#DTS 350
#Open an .R script and follow along with the examples. Make sure you add comments in your file so that when you return to the file you know what your code is doing.

#For all of the work we do today, you will need the tidyverse package.

```r
library(tidyverse)
```

```
## -- Attaching packages ---------------------------------------------------------------------------------------------------------------------- tidyverse 1.3.0 --
```

```
## v ggplot2 3.3.2     v purrr   0.3.4
## v tibble  3.0.3     v dplyr   1.0.0
## v tidyr   1.1.0     v stringr 1.4.0
## v readr   1.3.1     v forcats 0.5.0
```

```
## -- Conflicts ------------------------------------------------------------------------------------------------------------------------- tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

#You might want to refer to the ggplot2 cheatsheet.

#We will use the iris data in R. To see the descriptions of this data, run ?iris.



#Example 1.

#Below is the code we use to create a plot to investigate relationship between Sepal.Width and Sepal.Length



ggplot(data = iris, mapping = aes(x=Sepal.Width, y = Sepal.Length)) +
  geom_point()


#Exercise 1:
#  [] Try putting the mapping argument and its inputs in the geom_point() function instead. Did it change the plot? NO
#What are the advantages (or disadvantages) of putting the mapping argument in the ggplot() instead of geom_point()?
#  Advantage -- it creates a 'template aes mapping' for all geoms that are then layered onto the coord system.  
# It's efficient, as long as all goems have the same 'look'.
#   Disadvantage -- Pg3.3  You might want multiple layers (geoms like lines, 
# and points graphed on the coord system.  Each geom would need to have it's own aes.  
# Having mapping in each geom layer gives you that flexibility.   


ggplot(data = iris) +
  geom_point(mapping = aes(x=Sepal.Width, y = Sepal.Length))

  
#  Example 2.

#We want to encode the Species data with color, and make all the points diamond shape


ggplot(data = iris, mapping = aes(x = Sepal.Width, 
                                  y = Sepal.Length, 
                                  color = Species)) +
  geom_point(shape = 18)


#Exercise 2:
  
#  [] Choose any shape from the cheatsheet or figure 3.1 (pg3.9) of our textbook and change the points to that shape.


ggplot(data = iris, mapping = aes(x = Sepal.Width, 
                                  y = Sepal.Length, 
                                  color = Species)) +
  geom_point(shape = 11)



#Exercise 3:
  
#  Instead of having all of the points the same shape, we want the shape determined by the Species.

#[] Encode the Species of the data into the shape of the point.


ggplot(data = iris, mapping = aes(x = Sepal.Width, 
                                  y = Sepal.Length, 
                                  color = Species,
                                  shape = Species)) +
  geom_point()

#or


ggplot(data = iris) +
  geom_point(mapping = aes(x = Sepal.Width, 
                           y = Sepal.Length, 
                           color = Species,
                           shape = Species))

#[] What is the impact of putting something within the aes(), vs. outside the aes?
# Pg 3.8  You can lock-in the aestheic properties for all points, assuming color isn't a data-distinguisher, 
# but that you just want all points on the chart to be something other than standard black.
# for example (pg3.8) geom_point(mapping = aes(x = displ, y = hwy), color = "blue")  all points will be blue.

  
###  Adjusting the scale of various asthetics   
#?scale_manual

#Example 4:
  
#  We want to define the shape of the point for each species. To do that, 
# add the line 
### scale_shape_manual(values = c(1, 5, 7))   ???How to do this???
#to your code from Example 3.


ggplot(data = iris) +
  geom_point(mapping = aes(x = Sepal.Width, 
                           y = Sepal.Length, 
                           color = Species,
                           shape = Species)) +
  scale_shape_manual(values = c(1, 5, 7))  #this doesn't change the scale, it defines the shapes for the 3 variables.
#numbers in the C(list) are the assigned numbers from the shape list on pg3.9 (fig3.1)


#Example 5:
  
#  Notice that you can play with the scale of each aesthetic in a similar way.
#?scale_manual

#Exercise 5:
  
#  [] Try changing the x and y axis to a log scale by adding 
### scale_x_log10() + scale_y_log10   pg
#to the end of your code from Example 4.


ggplot(data = iris) +
  geom_point(mapping = aes(x = Sepal.Width, 
                           y = Sepal.Length, 
                           color = Species,
                           shape = Species)) +
  scale_shape_manual(values = c(1, 5, 7)) +
  scale_x_log10() + scale_y_log10()

#Exercise 6:
  
#  Manually change the colors.

#[] Make setosa purple, versicolor orange and virginica blue by adding the line 
### scale_color_manual(values = c("purple", "darkorange", "blue")) 
# to your code from Example 5.


ggplot(data = iris) +
  geom_point(mapping = aes(x = Sepal.Width, 
                           y = Sepal.Length, 
                           color = Species,
                           shape = Species)) +
  scale_shape_manual(values = c(1, 5, 7)) +
  scale_x_log10() + scale_y_log10() +
  scale_color_manual(values = c("purple", "darkorange", "blue"))  #colors in the c(list) become associated with 3 variables

#Example 7:
  
#  You can get carried away with color. Use the work many others have done for attractive, 
#useful combinations. Use brewer color scale. Pg28.26 For continuous variables, you can customize 
#the coloring with 
### scale_color_gradient().  pg28.28
# Also, notice that we are assigning this plot to the variable p so we can use it later.


p <- ggplot(data = iris, mapping = aes(x=Sepal.Width, 
                                       y = Sepal.Length, 
                                       color = Species,
                                       shape = Species),
            size = 5) +
  geom_point() +
  scale_color_brewer(palette = "Set1") 
#See the book for all the color palettes.


####
###Properly labeling the chart
#Example 8:
  
#  Use labs() to alter axis labels, title, captions, legends, etc.  pg28.2


ggplot(data = iris, mapping = aes(x = Sepal.Width, 
                                  y = Sepal.Length, 
                                  color = Species,
                                  shape = Species)) +
  geom_point() +
  scale_shape_manual(values =  c(1, 5, 7)) +
  scale_x_log10() +
  scale_y_log10() +
  scale_color_manual(values = c("purple", "darkorange", "blue")) + 
  labs(x = "Sepal Width (cm)",
       y = "Sepal Length (cm)",
       title = "This is where the title goes",
       subtitle = "        Subtitle goes here",  #with a little manual indentation
       caption = "Caption goes here")


#Exercise 8:
  
#[] Change the legend title of the Shape legend to read âSpecies of Irisâ instead of just âSpeciesâ.  pg28.5
# You can do this by adding in the labs() function the argument 
### shape = "Species of Iris"




ggplot(data = iris, mapping = aes(x = Sepal.Width, 
                                  y = Sepal.Length, 
                                  color = Species,
                                  shape = Species)) +
  geom_point() +
  scale_shape_manual(values =  c(1, 5, 7)) +
  scale_x_log10() +
  scale_y_log10() +
  scale_color_manual(values = c("purple", "darkorange", "blue")) + 
  labs(x = "Sepal Width (cm)",
       y = "Sepal Length (cm)",
       title = "This is where the title goes",
       subtitle = "        Subtitle goes here",  #with a little manual indentation
       caption = "Caption goes here",
       shape = "Species of Iris")
  #there are two legends on right side: Species, and Species of Iris

#[] What happens if you also change the title of the color legend to âSpecies of Irisâ?
#  It combines the two legends on right side into one: Species of Iris , with icons showing both shape and color.


ggplot(data = iris, mapping = aes(x = Sepal.Width, 
                                  y = Sepal.Length, 
                                  color = Species,
                                  shape = Species)) +
  geom_point() +
  scale_shape_manual(values =  c(1, 5, 7)) +
  scale_x_log10() +
  scale_y_log10() +
  scale_color_manual(values = c("purple", "darkorange", "blue")) + 
  labs(x = "Sepal Width (cm)",
       y = "Sepal Length (cm)",
       title = "This is where the title goes",
       subtitle = "        Subtitle goes here",  #with a little manual indentation
       caption = "Caption goes here",
       shape = "Species of Iris",
       color = "Species of Iris")
  
  
###  Themes
#We can use theme() to alter non-data related aspects of the chart.   pg28.35

#Example 9:
  
#  Use theme(plot.title) to center the title.


ggplot(data = iris, mapping = aes(x=Sepal.Width, 
                                  y = Sepal.Length, 
                                  color = Species,
                                  shape = Species)) +
  geom_point() +
  scale_shape_manual(values =  c(1, 5, 7)) +
  scale_x_log10() +
  scale_y_log10() +
  scale_color_manual(values = c(setosa = "purple", versicolor = "darkorange", virginica = "blue")) +
  labs(x= "Sepal Width (cm)",
       y = "Sepal Length (cm)",
       title = "This is where I would put a title") +
  theme(plot.title = element_text(hjust = .5))    #this centers the title horizontally


#There are some preprogrammed themes pg 28.37 which alter many formatting aspects of the chart at once instead of 
#having to alter each one. The theme attribute goes at the bottom of your plot layers.

#Example 10:
  
#  Use theme_bw() to change the formatting of the graph.


ggplot(data = iris, mapping = aes(x = Sepal.Width, 
                                  y = Sepal.Length, color = Species, shape = Species)) +
  geom_point() +
  scale_shape_manual(values =  c(1, 5, 7)) +
  scale_x_log10() +
  scale_y_log10() +
  scale_color_manual(values = c("purple", "orange", "blue")) +
  labs(x = "Sepal Width (cm)",
       y = "Sepal Length (cm)",
       title = "This is where I would put a title",
       shape = "Species of Iris",
       color = "Species of Iris") +
  theme_bw()           #white field, gray grid, black axis and box.


#Exercise 10:
  
#  [] Try playing with the chart by typing theme_ and then instead of selecting theme_bw(), 
# select from another of the autofill options such as classic, dark, minimal, pg 28.37 etc..


ggplot(data = iris, mapping = aes(x = Sepal.Width, 
                                  y = Sepal.Length, color = Species, shape = Species)) +
  geom_point() +
  scale_shape_manual(values =  c(1, 5, 7)) +
  scale_x_log10() +
  scale_y_log10() +
  scale_color_manual(values = c("purple", "orange", "blue")) +
  labs(x = "Sepal Width (cm)",
       y = "Sepal Length (cm)",
       title = "This is where I would put a title",
       shape = "Species of Iris",
       color = "Species of Iris") +
  theme_classic()           #white field, no grid, black axis, no box.

##


ggplot(data = iris, mapping = aes(x = Sepal.Width, 
                                  y = Sepal.Length, color = Species, shape = Species)) +
  geom_point() +
  scale_shape_manual(values =  c(1, 5, 7)) +
  scale_x_log10() +
  scale_y_log10() +
  scale_color_manual(values = c("purple", "orange", "blue")) +
  labs(x = "Sepal Width (cm)",
       y = "Sepal Length (cm)",
       title = "This is where I would put a title",
       shape = "Species of Iris",
       color = "Species of Iris") +
  theme_gray()           #gray field, white grid, no axis, no box.

###


ggplot(data = iris, mapping = aes(x = Sepal.Width, 
                                  y = Sepal.Length, color = Species, shape = Species)) +
  geom_point() +
  scale_shape_manual(values =  c(1, 5, 7)) +
  scale_x_log10() +
  scale_y_log10() +
  scale_color_manual(values = c("purple", "orange", "blue")) +
  labs(x = "Sepal Width (cm)",
       y = "Sepal Length (cm)",
       title = "This is where I would put a title",
       shape = "Species of Iris",
       color = "Species of Iris") +
  theme_linedraw()           #white field, black grid, black axis, black box.

###


ggplot(data = iris, mapping = aes(x = Sepal.Width, 
                                  y = Sepal.Length, color = Species, shape = Species)) +
  geom_point() +
  scale_shape_manual(values =  c(1, 5, 7)) +
  scale_x_log10() +
  scale_y_log10() +
  scale_color_manual(values = c("purple", "orange", "blue")) +
  labs(x = "Sepal Width (cm)",
       y = "Sepal Length (cm)",
       title = "This is where I would put a title",
       shape = "Species of Iris",
       color = "Species of Iris") +
  theme_light()           #white field, light gray grid, dark gray axis, dark gray box.


###
###Faceting  pg3.11
#Example 11:
  
# Create a separate panel (or facet) for each species, instead of plotting them all on the same plot. 
#This layer is usually after theme.


p <- ggplot(data = iris, mapping = aes(x = Sepal.Width, 
                                       y = Sepal.Length, 
                                       color = Species,
                                       shape = Species)) +
  geom_point() +
  scale_shape_manual(values =  c(1, 5, 7)) +
  scale_x_log10() +
  scale_y_log10() +
  scale_color_manual(values = c("purple", "orange", "blue")) +
  labs(x = "Sepal Length (cm)",
       y = "Sepal Width (cm)",
       title = "This is where I would put a title",
       color = "Species of Iris",
       shape = "Species of Iris") + 
  theme(plot.title = element_text(hjust = .5)) +
  theme_bw() +
  facet_wrap(vars(Species)) 

p
#Note: vars() means we supply faceting with variables.

#Exercise 11:
  
#  [] Fill in the blanks in the code below to add a horizontal line to each plot 
#that represents the mean Sepal length.

#First, do you have the right packages loaded to computed averages? Then calculate the averages:


library(dplyr)

# pipe on page 5.13,  group_by and summarise on pg 5.11


averages <- iris %>%    #pg 5.13
  group_by(Species) %>% 
  summarise(avglength = mean(Sepal.Length))
averages


#Add that value to the graph of p.  
# Note: !!! " p + " on the next line IS THE GRAPH CALLED P.
?geom_hline


p + geom_hline(data = averages, mapping = aes( yintercept = avglength))

#[] How could you change the color of all 3 horizontal lines to be red instead of black?


p + geom_hline(data = averages, mapping = aes( yintercept = avglength), color = "red")   #pg3.8


##Find the row number of the car with the best hwy miles per gallon in each class


library(tidyverse) #to get %>%
best_in_class <- mpg %>%
  group_by(class) %>%
  filter(row_number(desc(hwy)) == 1)
best_in_class
