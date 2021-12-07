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
d=p1.distance(p2) #get the distance between p1 and p2
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
```

other less common geometry objects:

- multipoints
- multilines
- multipolygon
