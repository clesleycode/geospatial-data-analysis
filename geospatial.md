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
- [2.0 Understanding the Data](#20-understanding-the-data)
    + [2.1 Data Types](#21-data-types)
        * [2.1.1 Point](#211-point)
        * [2.1.2 Polygon](#212-polygon)
- [3.0 Analysis](#30-analysis)
    + [3.1 Geojsonio](#31-geojsonio)
    + [3.2 Geopandas](#32-geopandas)
    + [3.3 Shapely](33-shapely)
- [4.0 Plotting](#40-plotting)
- [5.0 ](#50-)
- [6.0 Final Words](#60-final-words)
    + [6.1 Resources](#61-resources)


## 0.0 Setup

This guide was written in Python 2.7.

### 0.1 Python and Pip

Download [Python](https://www.python.org/downloads/) and [Pip](https://pip.pypa.io/en/stable/installing/).

### 0.2 Libraries

```
pip install geojsonio
pip install geopandas
pip install shapely
```

## 1.0 Background

### 1.1 What is Geospatial Data Analysis? 

Geospatial analysis involves applying statistical analysis to data which has a geographical aspect. 


## 2.0 Understanding the Data

### 2.1 Data Types

Spatial data consists of location observations. Spatial data identifies features and positions on the Earth’s surface and is ultimately how we put our observations on the map.

### 2.1.1 Point

A Point is a 0-dimensional object representing a single location. Put more simply, they're XY coordinates.

### 2.1.2 Polygon

A Polygon is a two-dimensional surface stored as a sequence of points defining the exterior.

## 3.0 Analysis

#### 3.1 Geojsonio

``` python
from geojsonio import display

with open('map.geojson') as f:
    contents = f.read()
    display(contents)

```

#### 3.2 Geopandas

``` python
import geopandas as gpd
import geojsonio

states = gpd.read_file('states.geojson')
geojsonio.display(states.to_json())
```

### 3.3 Shapely

## 4.0 Plotting

## 6.0 Final Words

### 6.1 Resources

[GeoJSON](http://geojson.org/) <br>
[OpenStreetMap](https://www.openstreetmap.org/#map=5/51.500/-0.100)


