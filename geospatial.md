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
    + [1.2 Understanding the Data](#12-understanding-the-data)
- [2.0 Data Handling](#20-data-handling)
- [3.0 Analysis](#30-analysis)
    + [3.1 Geopandas](#31-geopandas)
- [4.0 Plotting](#40-plotting)
- [5.0 ](#40-)
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

### 1.2 Understanding the Data


## 2.0 Data Handling



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
import geopandas as gp

geo = [] 
segs = gp.GeoDataFrame.from_features(geo)
segs.head()
```

## 4.0 Plotting

## 6.0 Final Words

### 6.1 Resources

[GeoJSON](http://geojson.org/) <br>
[OpenStreetMap](https://www.openstreetmap.org/#map=5/51.500/-0.100)


