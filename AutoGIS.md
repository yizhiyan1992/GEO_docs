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

### EPSG: European petroleum servey group

- It is a public registry of geodetic datums, spatial reference system, Earth ellipsoids, coordinate transformations and related unit of measurement. Each entity is assigned an EPSG code between 1024-32767, along with a standard machine readable well-known text (WTK) representationm.
- common EPSG codes:

  - EPSG:4326 -WGS84 (gps coordinate system)
  - EPSG:3857 -Web mercator projection (used by Google Map and OpenstreetMap)
  - EPSG: 7789 - International terrestrial reference frame 2014.

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

### Spatial join

- definition: getting attributes from one layer and transferring them into another layer based on their spatial relationship.

- geopandas.sjoin() allows to perform spatial join. There are three possible types of join:

  - intersects
  - within
  - contains

- "how" parameter: left, right, inner (default)

- Note that before performing spatial join, we need to check the coordinate projection for both GDF.

### Other combining methods (merge, join, concatenate, and compare) in GeoPandas

- append: merge rows from one table to the other. Note that the columns names should be matched for two tables.

- attribute joins: attribute joins are accomplished using the merge().

- spatial joins: in a spatial join, two geometry objects are merged based on their spatial relationship to one another.

  - .sjoin(): join based on binary predicates.
  - .sjoin_nearest(): join based on proximity, with the ability to set a maximum search radius.
  - predicate argument:
    - intersects
    - contains
    - within
    - touches
    - crosses
    - overlaps
  - how argument:
    - left
    - right
    - inner (default)

- clip: clip points, lines, or polygon geometries to the mask extent. Both layers must be in same crs. The gdf will be clipped to the full extext of the clip object.

- set-operation with overlay: when working with multiple spatial datasets- users oftern wish to create new shapes based on places where those datasets overlap.
  `df1.overlay(df2,how='union')`

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

  <img src="./pictures/kdtree.jpg" width="500" />

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

- R-tree
  R-tree are tree data structures used for spatial access methods. For indexing multi-dimensional information such as points, linestrings, or polygons. A common real-world usage for R-tree might be to store spatial objects such as restaurant locations or the polygons that typical maps are made of street, buildings, etc. and then find answers quickly to queries such as: "find all museums within 2km of my current location", "retrieve all road segments within 2km of my location", or "find the nearest gas station".

### Spatial index in GeoPandas

- Geopandas offers built-in support for spatial indexing using an R-Tree algorithm. Depending on the ability to import `pygeos`, GeoPandas will either use pygeos.STRtree or rtree.index.Index. `GeoSeries.sindex` creates a spatial index, wich can use the methods and properties documented below:

| method                                  | descrption                                                                                                                                                            |
| --------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `intersection`                          | compability wrapper for rtree.index.Index.intersection, use `query` instead.                                                                                          |
| `nearest(*args,**kwargs)`               | return the nearest geometry in the tree for each input geometry in `geometry`                                                                                         |
| `query(geometry[, predicate, sort])`    | return the index of all geometries in the tree with extents that intersect the envelope of the input geometry                                                         |
| `query_bulk(geometry[,predicate,sort])` | Returns all combinations of each input geometry and geometries in the tree where the envelope of each input geometry intersects with the envelope of a tree geometry. |
| `valid_query_predicates`                | Returns valid predicates for this spatial index.                                                                                                                      |

## Lesson 4- Geometric operations and data reclassification

### Data reclassification

`PySAL/mapclassify` module is an extensive Python library for spatial analysis. It includes all of the most common data classifier that are used commonly.

Q: How to classify the DataFrame based on a certain feature?

```python
import mapclassify
classifier=mapclassify.Quantile.make(k=5) # return a function
df[['feature']].apply(classifier)

df.count() # count the non NAN value in each row or column
df.count_values() # count the unique values in a row or column
```

### Aggregating data

Combine the data into groups. For example, aggregate data in same district. We can use `dissolve(by=)` to perform aggregation.

### Map tile

- what is maptile?

  Map tile is a map displayed in a web browser by seamlessly joining dozens of individually requested image or vector data files. The first map tile used raster files, before the emergence of vector tiles.

- why use map tiles?

  map tile can be cached, and map tile can increase the transmission efficiency and reduce the server's pressure.

- Google Map conventions:

  - tiles are 256\*256 pixels
  - at the outer most zoom level (0), the entire world can be rendered in a single map.
  - each zoom level doubles in both dimensions
  - the web mercator projection is used with latitude limits of aorund 85 degrees.

## Lesson 5: creating interactive maps in Python

#### **Folium**

- What is Folium?

  -`Folium` is a python library used for visualizing geospatial data. It is easy to use and yet a powerful library. Folium is a python wrapper for `Leaflet.js` which is a leading open-source JS library for plotting interactive maps.

- create a map object

```python
import folium
m=folium.Map(location=[lat, lon], width= , height= ,zoom_start= )
```

A tutorial on how to use Folium for network visualization: [click here](https://github.com/yizhiyan1992/GEO_docs/blob/main/tutorial_examples/folium_tutorial.ipynb)

## Lesson 6: Open street map data/OSMnx packages/Network analysis

### OSMnx package

### Networkx package

`Networkx` is a python module that provides tools for analyzing networks in various different ways. It also contains algorithms such as `Dijkstra` or `A*` algorithm that are commonly used to find shortest paths along transportation network.

Add later...
