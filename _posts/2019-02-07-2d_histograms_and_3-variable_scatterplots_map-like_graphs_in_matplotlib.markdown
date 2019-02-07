---
layout: post
title:      "2D histograms and 3-variable scatterplots: map-like graphs in matplotlib"
date:       2019-02-07 23:17:33 +0000
permalink:  2d_histograms_and_3-variable_scatterplots_map-like_graphs_in_matplotlib
---


#### *Why use matplotlib for spatial data?*

As part of a project to predict house prices, I was handed a large dataset of housing data from King County, Washington, USA. This dataset contained a wide range of house-related data, including the latitude and longitude of each house sold within a one-year period. As latitude and longitude are both continuous numerical data, these are potentially useful features to use in a multiple linear regression model. However, even before getting to that stage of the project, seeing latitude and longitude in a dataset is a sign that the exciting world of spatial data mapping is also available to you.

Several free or low-cost GUI-based spreadsheet programs with semi-functional mapping capabilities do exist - from the cumbersome Excel Power Map, to the limited Google My Maps, to the sadly soon-to-be discontinued Google Fusion Tables. However, with just a few lines of code you can create informative map-like visualisations using Python much more quickly, with far more options for customisation. And you don't even need knowledge of niche geospatial analysis libraries to do it - as long as you have latitude and longitude in your dataset, you can visualise it using everyone's favourite graphing buddy matplotlib.


#### *Two-dimensional histograms*

Histograms are a great way to visualise the distribution of a variable in a dataset, by showing the number of entries in your dataset that lie in particular value ranges, or bins. The code below produces simple (one-dimensional) histograms for latitude and longitude in the King County ('kc') dataset:

```
import matplotlib.pyplot as plt
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 5))
fig.suptitle('Distributions of latitude and longitude in the King County housing dataset', fontsize=16)
ax1.hist(kc.lat)
ax1.set_xlabel('Latitude', fontsize=13)
ax1.set_ylabel('Frequency', fontsize=13)
ax2.hist(kc.long)
ax2.set_xlabel('Longitude', fontsize=13)
ax2.set_ylabel('Frequency', fontsize=13);
```

![Latitude and longitude histograms](https://i.imgur.com/rRxyNlA.png)

These work fine as a way to show the distribution of each individual variable, but they don't really give you a sense of what this looks like geographically. What we want is a map. Luckily enough, we can achieve this with a two-dimensional histogram. This is essentially combining a histogram along the x axis (longitude) with a histogram along the y axis (latitude). Instead of the bins being the width of the bars (i.e. a single dimension) they are now essentially a grid (i.e. a square of two dimensions). We can add a colorbar with `plt.colorbar()` to help us visualise this as a kind of heatmap:

```
plt.figure(figsize = (10,8))
plt.hist2d(kc.long, kc.lat, bins=150, cmap='hot')
plt.colorbar().set_label('Number of properties')
plt.xlabel('Longitude', fontsize=14)
plt.ylabel('Latitude', fontsize=14)
plt.title('Number of properties in each area of King County, Washington', fontsize=17)
plt.show()
```

![Two-dimensional histogram](https://i.imgur.com/KLWsXy9.png)

And if you look closely and squint a bit, you can make out the shape of King County with Seattle towards the top left, and the shape of the Puget Sound.

![Map of King County, Washington, USA](https://i.imgur.com/Br5BMAi.png)

*(Source: Google Maps)*


#### *Scatterplots with three variables*

One other way of plotting spatial data with matplotlib is by using scatterplots. Scatterplots visualise the relationship between two variables, one on the x axis and one on the y axis, by plotting points for each datapoint at the values for its x and y variables. Matplotlib allows us to go one step further, and use a third variable to change the colour (or shape, or size) of each point in accordance with the datapoint's value for this third variable.

The King County database contains, among other things, the price at which each house was sold and the size of the living space of each house. From looking at the 2D histogram above, it's clear that a lot of properties are found in and around Seattle, but that the dataset also includes properties further away from the city. Lot size is likely to vary in a non-random way, with smaller lots in the city and larger lots in the country. Therefore, a better metric to compare like-for-like is to compare price per square foot.

In this scatterplot, longitude and latitude are plotted, and points are coloured by their price per square foot to produce a map of how expensive each area is:

```
plt.figure(figsize = (10,8))
plt.scatter(kc.long, kc.lat ,c=kc.price_per_sqft, cmap = 'hot', s=1)
plt.colorbar().set_label('Price per square foot ($)', fontsize=14)
plt.xlabel('Longitude', fontsize=14)
plt.ylabel('Latitude', fontsize=14)
plt.title('House prices in King County, Washington', fontsize=17)
plt.show()
```

![Scatterplot of prices per square foot](https://i.imgur.com/W1MaRI0.png)

Not surprisingly, the most expensive properties (in terms of price per square foot) are found in Bellevue and Medina - home of tech billionaires Bill Gates and Jeff Bezos, as well as some of the most expensive zipcodes in the country.

So there we are - two methods for creating map-like viualisations using matplotlib. The next time you find yourself in possession of a dataset including latitude and longitude, why not give them a try!
