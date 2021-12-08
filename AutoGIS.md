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

```GeometryCollection()``` can view all geometry objects.

How to get the coordinates of objects?
```python
np.array(obj.coords) # return a np.ndarray
list(obj.coords) # return a list of tuples
```

other useful methods:

```obj.project()```

```obj.interpolate()```

```obj.intersection()```

```polygon.contains(point)```

reads:
- [Geospatial adventures. Step 1: shapely](https://towardsdatascience.com/geospatial-adventures-step-1-shapely-e911e4f86361)
