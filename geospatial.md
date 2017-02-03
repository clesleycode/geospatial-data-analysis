Intro to Geospatial Data Analysis in Python 
==================

Brought to you by [Lesley Cordero](http://www.columbia.edu/~lc2958) and [ADI](https://adicu.com)

## Table of Contents

- [0.0 Setup](#00-setup)
    + [0.1 Python and Pip](#01-python--pip)
    + [0.2 Libraries](#02-libraries)
    + [0.3 Other](#03-other)
- [1.0 Background](#10-background)
    + [1.1 What is Geospatial Data Analysis](#11-what-is-geospatial-data-analysis)
    + [1.2 Why is Geospatial Analysis Important?](#12-why-is-geospatial-analysis-important)
    + [1.3 Terminology](#13-terminology)
        * [1.3.1 Interior Set](#131-interior-set)
        * [1.3.2 Boundary Set](#132-boundary-set)
        * [1.3.3 Exterior Set](#133-exterior-set)
    + [1.4 Data Types](#14-data-types)
        * [1.4.1 Point](#141-point)
        * [1.4.2 Polygon](#142-polygon)
        * [1.4.3 Curve](#143-curve)
        * [1.4.4 Surface](#144-surface)
- [2.0 Geojsonio & Geopandas](#30-geojsonio-geopandas)
    + [2.1 Geojsonio](#31-geojsonio)
    + [2.2 Geopandas](#32-geopandas)
- [3.0 Plotly](#30-plotly)
- [4.0 Shapely & Descartes](#50-Shapely-Descartes)
- [5.0 Final Words](#50-final-words)
    + [5.1 Resources](#51-resources)
    + [5.2 Mini Courses](#52-mini-courses)


## 0.0 Setup

This guide was written in Python 3.5

### 0.1 Python and Pip

Download [Python](https://www.python.org/downloads/) and [Pip](https://pip.pypa.io/en/stable/installing/).

### 0.2 Libraries

```
pip install geojsonio
pip install geopandas
pip install shapely
pip install plotly
```

### 0.3 Plotly

Sign up and make an account [here](https://plot.ly).

## 1.0 Background

### 1.1 What is Geospatial Data Analysis? 

Geospatial analysis involves applying statistical analysis to data which has a geographical aspect. 

### 1.2 Why is Geospatial Analysis Important?

Think about how much data contains location as an aspect. Anything in which location makes a difference or can be represented by location is likely going to be a geospatial problem. With different computational tools, we can create beautiful and meaningful visualizations that tell us about how location affects a given trend. 

### 1.3 Terminology

Before we get into the specifics, first we'll review some terminology that you should keep in mind. 

#### 1.3.1 Interior Set

An Interior Set is the set of points contained within a geometrical object. 

#### 1.3.2 Boundary Set

A Boundary Set is the set of points which form the outline of a geometrical object. Boundary Sets and Interior Sets have no intersection. 

#### 1.3.3 Exterior Set

An Exterior Set is the set of all other points. 

### 1.4 Data Types

Spatial data consists of location observations. Spatial data identifies features and positions on the Earth’s surface and is ultimately how we put our observations on the map.

#### 1.4.1 Point

A Point is a zero-dimensional object representing a single location. Put more simply, they're XY coordinates.

Because points are zero-dimensional, they contain exactly one interior point, 0 boundary points, and infinite many exterior points. 

#### 1.4.2 Polygon

A Polygon is a two-dimensional surface stored as a sequence of points defining the exterior.

#### 1.4.3 Curve

 A Curve has an interior set consisting of the infinitely many points along its length, a boundary set consisting of its two end points, and an exterior set of all other points. 

 Curves contain an infinite number of interior points, exactly two boundary points, and infinite many exterior points. 
 
#### 1.4.4 Surface

 A Surface has an interior set consisting of the infinitely many points within, a boundary set consisting of one or more Curves, and an exterior set of all other points.

 Surfaces have infinite many interior, exterior, and boundary points.

## 2.0 Geojsonio & Geopandas 

### 2.1 Geojsonio

Geojsonio converts geographic data to geojson formats. Here we read in the json of a point and plot it on an interactive map. 

``` python
from geojsonio import display

with open('map.geojson') as f:
    contents = f.read()
    display(contents)
```

### 2.2 Geopandas

GeoPandas is an open source project to make working with geospatial data in python easier by extending the datatypes used by pandas to allow spatial operations on geometric types. So now that we know what polygons are, we can set up a map of the United States using data of the coordates that shape each state. 

``` python
import geopandas as gpd
import geojsonio

states = gpd.read_file('states.geojson')
geojsonio.display(states.to_json())
```


## 3.0 Shapely & Descartes

Shapely converts feature geometry into GeoJSON structure and contains tools for geometry manipulations. This module works with three of the types of geometric objects we discussed before: points, curves, and surfaces.

First, we import the needed modules. 

``` python
from shapely.geometry import shape, LineString, Point
from descartes import PolygonPatch
import fiona, pylab
```

These are some coordinates we'll need to plot the path of a flight from San Francisco to New York. 
``` python 
latlons = [(37.766, -122.43), (39.239, -114.89), (38.820, -104.82), (38.039, -97.96),
    (38.940, -92.32), (39.156, -86.53), (40.749, -84.08), (41.494, -81.66),
    (42.325, -80.06), (41.767, -78.01), (41.395, -75.68), (40.625, -73.780)]
```

This simply takes the points and reformats the x and y (since it's originally coordinates) and converts it to a LineString type. 

``` python
path = [(x, y) for y, x in latlons]
ls = LineString(path)
```

So now we want to display this on a map. Using some data I found online (which you can access via the github), I turn each of these into polygon with fiona.  

``` python
with fiona.collection("shapefiles/statesp020.shp") as features:
        states = [shape(f['geometry']) for f in features]

fig = pylab.figure(figsize=(24, 12), dpi=180)
```

Don't worry about this session too much, but it basically finds where the intersections occur to denote a dark outline. 

``` python
for state in states:
    if state.geom_type == 'Polygon':
        state = [state]

    for poly in state:
        if ls.intersects(poly):
            alpha = 0.4
        else:
            alpha = 0.4

        try:
            poly_patch = PolygonPatch(poly, fc="#6699cc", ec="#6699cc", alpha=alpha, zorder=2)
            fig.gca().add_patch(poly_patch)
        except:
            pass
```

Here, we format the specifics of our plot. 

``` python
fig.gca().plot(*ls.xy, color='#FFFFFF')
```

This is where we set the outlines to a separate color. 
``` python
for x, y in path:
    p = Point(x, y)
    spot = p.buffer(.1)
    x, y = spot.exterior.xy
    pylab.fill(x, y, color='#cc6666', aa=True)
    pylab.plot(x, y, color='#cc6666', aa=True, lw=1.0)
```

And finally, we save this image we have created to a file with a png extension. 
``` python
fig.gca().axis([-125, -65, 25, 50])
fig.gca().axis('off')
fig.savefig("states.png", facecolor='#F2F2F2', edgecolor='#F2F2F2')
```

## 4.0 Plotly

In order to use plotly, you have to own an account with your own API key. To find these, go into your plotly folder and type the following command into your terminal:

```
vim ~/.plotly/.credentials
```

It should be in the following format - the username and api_key fieds will be especially important. 

``` 
{
    "username": "username",
    "stream_ids": [],
    "api_key": "api_key",
    "proxy_username": "",
    "proxy_password": ""    
}
```
And so here we begin. First, as always, we import the needed modules. From there, we initialize our session by signing in on plotly.


``` python

import plotly.plotly as py
from plotly.graph_objs import *

py.sign_in('username', 'api_key')

```

Plotly supports three types of maps - chloropeth, atlas maps, and satelite maps. Using data from the electoral college, we'll plot a map of the United States with a color scale. The dark the color, the greater number of votes.

So first, we load in the electoral college data. 

``` python
data = Data([
    Choropleth(
        z=[3.0, 3.0, 3.0, 3.0, 3.0, 3.0, 3.0, 3.0, 4.0, 4.0, 4.0, 4.0, 4.0, 5.0, 5.0, 5.0, 6.0, 6.0, 6.0, 6.0, 6.0, 6.0, 7.0, 7.0, 7.0, 8.0, 8.0, 9.0, 9.0, 9.0, 10.0, 10.0, 10.0, 10.0, 11.0, 11.0, 11.0, 11.0, 12.0, 13.0, 14.0, 15.0, 16.0, 16.0, 18.0, 20.0, 20.0, 29.0, 29.0, 38.0, 55.0],
        autocolorscale=False,
        colorbar=ColorBar(
            title='Votes'
        ),
```
In the same 'Data' call, we feed in parameters that will be our sliding scale. 

``` python
        colorscale=[[0.0, 'rgb(242,240,247)'], [0.2, 'rgb(218,218,235)'], [0.4, 'rgb(188,189,220)'], [0.6, 'rgb(158,154,200)'], [0.8, 'rgb(117,107,177)'], [1.0, 'rgb(84,39,143)']],
        hoverinfo='location+z',
```

Again, continuing the parameters, we add labels for each state.  

``` python

        locationmode='USA-states',
        locations=['DE', 'VT', 'ND', 'SD', 'MT', 'WY', 'AK', 'DC', 'NH', 'RI', 'ME', 'ID', 'HI', 'WV', 'NE', 'NM', 'MS', 'AR', 'IA', 'KS', 'NV', 'UT', 'CT', 'OR', 'OK', 'KY', 'LA', 'SC', 'AL', 'CO', 'MD', 'MO', 'WI', 'MN', 'MA', 'TN', 'IN', 'AZ', 'WA', 'VA', 'NJ', 'NC', 'GA', 'MI', 'OH', 'PA', 'IL', 'NY', 'FL', 'TX', 'CA'],
        marker=Marker(
            line=Line(
                color='rgb(255,255,255)',
                width=2
            )
        ),
        text=['DE', 'VT', 'ND', 'SD', 'MT', 'WY', 'AK', 'DC', 'NH', 'RI', 'ME', 'ID', 'HI', 'WV', 'NE', 'NM', 'MS', 'AR', 'IA', 'KS', 'NV', 'UT', 'CT', 'OR', 'OK', 'KY', 'LA', 'SC', 'AL', 'CO', 'MD', 'MO', 'WI', 'MN', 'MA', 'TN', 'IN', 'AZ', 'WA', 'VA', 'NJ', 'NC', 'GA', 'MI', 'OH', 'PA', 'IL', 'NY', 'FL', 'TX', 'CA']
    )
])
```

As a final product, we'll have: 

``` python
data = Data([
    Choropleth(
        z=[3.0, 3.0, 3.0, 3.0, 3.0, 3.0, 3.0, 3.0, 4.0, 4.0, 4.0, 4.0, 4.0, 5.0, 5.0, 5.0, 6.0, 6.0, 6.0, 6.0, 6.0, 6.0, 7.0, 7.0, 7.0, 8.0, 8.0, 9.0, 9.0, 9.0, 10.0, 10.0, 10.0, 10.0, 11.0, 11.0, 11.0, 11.0, 12.0, 13.0, 14.0, 15.0, 16.0, 16.0, 18.0, 20.0, 20.0, 29.0, 29.0, 38.0, 55.0],
        autocolorscale=False,
        colorbar=ColorBar(
            title='Votes'
        ),
        colorscale=[[0.0, 'rgb(242,240,247)'], [0.2, 'rgb(218,218,235)'], [0.4, 'rgb(188,189,220)'], [0.6, 'rgb(158,154,200)'], [0.8, 'rgb(117,107,177)'], [1.0, 'rgb(84,39,143)']],
        hoverinfo='location+z',
        locationmode='USA-states',
        locations=['DE', 'VT', 'ND', 'SD', 'MT', 'WY', 'AK', 'DC', 'NH', 'RI', 'ME', 'ID', 'HI', 'WV', 'NE', 'NM', 'MS', 'AR', 'IA', 'KS', 'NV', 'UT', 'CT', 'OR', 'OK', 'KY', 'LA', 'SC', 'AL', 'CO', 'MD', 'MO', 'WI', 'MN', 'MA', 'TN', 'IN', 'AZ', 'WA', 'VA', 'NJ', 'NC', 'GA', 'MI', 'OH', 'PA', 'IL', 'NY', 'FL', 'TX', 'CA'],
        marker=Marker(
            line=Line(
                color='rgb(255,255,255)',
                width=2
            )
        ),
        text=['DE', 'VT', 'ND', 'SD', 'MT', 'WY', 'AK', 'DC', 'NH', 'RI', 'ME', 'ID', 'HI', 'WV', 'NE', 'NM', 'MS', 'AR', 'IA', 'KS', 'NV', 'UT', 'CT', 'OR', 'OK', 'KY', 'LA', 'SC', 'AL', 'CO', 'MD', 'MO', 'WI', 'MN', 'MA', 'TN', 'IN', 'AZ', 'WA', 'VA', 'NJ', 'NC', 'GA', 'MI', 'OH', 'PA', 'IL', 'NY', 'FL', 'TX', 'CA']
    )
])

```
And so now, we've formatted our data. But of course, there needs to be a layout for our code to follow. Luckily, we can just use the Layout method. As parameters, we feed in color schemes and labels. 

``` python
layout = Layout(
    geo=dict(
        lakecolor='rgb(255, 255, 255)',
        projection=dict(
            type='albers usa'
        ),
        scope='usa',
        showlakes=True
    ),
    title='2016 Electoral College Votes'
)
```

And we're almost done! The last step is to simply construct the math and use plotly to actually display it. 

``` python
fig = Figure(data=data, layout=layout)
plot_url = py.plot(fig)
```


## 5.0 Final Words

Most of these techniques are interchangeable in R, but Python is one of the best suitable languages for geospatial analysis. Its modules and tools are built with developers in mind, making the transition into geospatial analysis must easier.

### 5.1 Resources

[GeoJSON](http://geojson.org/) <br>
[OpenStreetMap](https://www.openstreetmap.org/#map=5/51.500/-0.100)


### 5.2 Mini Courses

Learn about courses [here](www.byteacademy.co/all-courses/data-science-mini-courses/).

[Python 101: Data Science Prep](https://www.eventbrite.com/e/python-101-data-science-prep-tickets-30980459388) <br>
[Intro to Data Science & Stats with R](https://www.eventbrite.com/e/data-sci-109-intro-to-data-science-statistics-using-r-tickets-30908877284) <br>
[Data Acquisition Using Python & R](https://www.eventbrite.com/e/data-sci-203-data-acquisition-using-python-r-tickets-30980705123) <br>
[Data Visualization with Python](https://www.eventbrite.com/e/data-sci-201-data-visualization-with-python-tickets-30980827489) <br>
[Fundamentals of Machine Learning and Regression Analysis](https://www.eventbrite.com/e/data-sci-209-fundamentals-of-machine-learning-and-regression-analysis-tickets-30980917759) <br>
[Natural Language Processing with Data Science](https://www.eventbrite.com/e/data-sci-210-natural-language-processing-with-data-science-tickets-30981006023) <br>
[Machine Learning with Data Science](https://www.eventbrite.com/e/data-sci-309-machine-learning-with-data-science-tickets-30981154467) <br>
[Databases & Big Data](https://www.eventbrite.com/e/data-sci-303-databases-big-data-tickets-30981182551) <br>
[Deep Learning with Data Science](https://www.eventbrite.com/e/data-sci-403-deep-learning-with-data-science-tickets-30981221668) <br>
[Data Sci 500: Projects](https://www.eventbrite.com/e/data-sci-500-projects-tickets-30981330995)
