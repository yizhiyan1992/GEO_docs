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

### Q: how to find the nearest points?

```python
from shapely.ops import nearest_points
shapely.ops.nearest_points(geom1, geom2)
#Returns a tuple of the nearest points in the input geometries.

# nearest_points can also be applied to get the point between point and linestring or polygon.
```

The `nearest_points` method can be quite low efficient for large scale dataset. For large-scale dataset, consider using **BallTree** or **KDTree**.

### Q: how to find the nearest point for a column of points?

build a geodataframe and use .apply() function.

### Metrics for examing distance

- Euclidean distance

  Euclidean distance does not apply well for high-dimension data. The distance will be skewed if the units are different for different dimensions.

- Cosine similarity

  It applies well for high-dimension data. However, it cannot measure the gratitude of the difference.

- Hamming distance

  Measure the difference for strings with equal length.

- Manhattan Distance

  Integer computation is faster than float computation.

- Chebyshev Distance

- Minkowski distance

- Jaccard similarity

  Intersection/Union. Mostly used in text processing and image processing.

- Haversine distance

  Calculate the shortest distance for two points on sphere. If the shape is ellipsoid, we should use `vincenty distance`.

### Spatial index searching trees

- k-d (k-dimensional) tree
  ![k-d tree](./pictures/kdtree.jpg)

  Description:

  a k-d tree is a space-partitioning data structure for organizing points in a k-dimensional spaces. K-d trees are useful for several applications, such as searches involving a multidimensional search key (range searches and nearest neighbor searches) and creating point clouds.

  Time complexity:
  | algorithm | average | worst case |
  |-----------|---------|------------|
  | space | O(n) | O(n) |
  | search| O(logn)| O(n) |
  | insert| O(logn)| O(n) |
  | delete| O(logn)| O(n) |

  Construction process:

  - as one moves down the tree, one cycles through the axes used to select the splitting planes.
  - Points are inserted by selecting the median of the points being put into the subtree, with respect to their coordinates in the axis being used to create the splitting plane.

  Nearest neighbor searching process:

  1. start with root node, the algorithm moves down the tree recursively. It goes down left or right depending on whether the point is lesser than or greater than the current node in the split dimension.

  2. Once the algorithm reaches a leaf node, it checks that node point and if the distance is better, that node point is saved as the "current best".

  3. the algorithm unwinds the recursion of the tree, performing the following steps at each node:
     - if the current node is closer than the curren best, then it becomes the current best.
     - the algorithm checkes whether there could be any points on the other side of the splitting plane that are closer to the search point than the current best.
       - if the hypershpere crosses the plane, there could be nearer points on the other side of the planes, so the algorithm must move down the other branch of the tree from the current node looking for closer points, following the same recursive process as the entire search.
       - if the hyoersphere does not intersect the splitting plane, then the algorithm continues walking up the tree, and the entire branch on the other side of that node is eliminated.
  4. when the algorithm finishes this process for the root node, then the search is complete.
