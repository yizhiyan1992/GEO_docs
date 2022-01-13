website: http://automating-gis-processes.github.io/site/index.html

## Lesson 1- shapely

- The most fundamental geometric objects are Points, Lines, and Polygons which are the basic ingredients when working with spatial data in vector format.

create a point object:

```python
from shapely.geometry import Point
p1=Point(3,5)
p2=Point(2,2)
p2_3d=Point(3,5,1)
print(p1.geom_type) # get the geomtry type
p1.coords.xy # get the coordinate of point1
d=p1.distance(p2) # get the distance between p1 and p2
p1.name = "point 1" # define the name of p1
```

create a LineString object:

```python
from shapely.geometry import LineString
# the LineString object is created by a list of Point objects or a list of coordinate tuples
l1 = LineString([p1,p2])
l2 = LineString([(2,2), (5,7)])
l1.xy # get the coordinates of l1
l1.length # get the length
l1.centroid # get the centroid
p1.distance(l1) # the distance between point to line segment
```

create polygon object:

```python
from shapely.geometry import Polygon
# using similar methods of creating LineString to create Polygon (note that Polygon must take at least 3 points)
poly1 = Polygon([(1,1), (4,2), (5,7)])
#  create polygon with holes
holes=[[(0,0),(1,1),(2,0)]] # list of list
exterior=[(-1,-1),(-1,2),(2,2),(2,0)]
poly2=Polygon(shell=exterior,holes=holes)
# attributes
poly1.area # get the area
poly1.bounds # get the bounding box of the polygon
poly1.exterior # get the exterior
poly.length # get the length of object
p1.buffer(radius) # create a buffer ring
```

other less common geometry objects:

- multipoints
- multilines
- multipolygon

**shapely** can also load string data, which functions similar to pickle:

```python
from shapely import wkt
wkt.loads('POINT (3 5)')
```

`GeometryCollection()` can view all geometry objects.

How to get the coordinates of objects?

```python
np.array(obj.coords) # return a np.ndarray
list(obj.coords) # return a list of tuples
```

other useful methods:

`obj.project()`

`obj.interpolate()`

`obj.intersection()`

`polygon.contains(point)`

reads:

- [Geospatial adventures. Step 1: shapely](https://towardsdatascience.com/geospatial-adventures-step-1-shapely-e911e4f86361)

## Lesson 2- Vector data I/O and Coordinate systems

main goals:

- read and write from/to shapefile
- create geometrics into GeoDataFrame
- change the coordinate reference system of the data

basic concepts:

- shapefile: a **vector** data format for storing location information and related attributes. (contains several files .shp, .shx, .dbf, .prj etc.) The shapefile is developed by ESRI.
- geopackage: an open source format for storing and transferring geospatial information. GeoPackage is a container for an SQLite database with a .gpkg extension.
- CRS: coordinate reference system defines how coordinates relate to real locations on the Earth.
- datum: defines the center point, orientation, and scale of the reference surface related to a coordinate reference system. Same coordinates can relate to different locations depending on Datum.
- EPSG: Europe Petroleum Survey Group, a special reference systems.

## GeoPandas

- The main difference between geodataframe and dataframe is that a geodataframe should contain one column for geometries.

- data types:
  - GeoDataFrame
  - GeoSeries

how to read/write shapefile?

```python
import geopandas as gpd
gpd.read_file("xxx.shp")
gpd.to_file(file_name, path)
```

:heavy_exclamation_mark: To read .shp file, the .shp file must in a folder that contains .shx, .prj, .dbf files.

`gpd.io.file.fiona.drvsupport.supported_drivers` will tell all supported file types.

how to construct a geodataframe:

1. pass the dataframe that contains 'geometry' column
2. pass the shapely object with arg geometry = [Point/LineString/Polygon...]

visualize the geometries:

```python
%matplotlib inline
gpd_data.plot()

gpd_data['centroid'] = gpd_data.centroid # get the centroid of each object
gpd_data = gpd_data.set_geometry('centroid) # reset the geometry columns
```

some useful methods from pandas (also apply to geopandas):

`df.iterrows()` : return the index and series object of each row

`df.unique()` : return the unique objects

`df.groupby` : return the keys and df corresponding to each key.

## Lesson 3- GeoCoding/ Query spatial index/ spatial analysis

Concepts:

- Geocoding: Geocoding is the process of transforming place names or addresses into coordinates. (e.g. find coordinates using address)

- Reverse geocoding: A process that uses coordinates to identify the street addresses or other features.

### Q: how to check if a point is in a polygon?

Shapely

`Point.within(Polygon)`

`Polygon.contains(Point)`

It works the same for GeoDataFrame

`gdf.within(polygon)`

### Q: how to boost spatial queries?-Spatial Index

[document](https://automating-gis-processes.github.io/site/notebooks/L3/spatial_index.html)
