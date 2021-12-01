## Learning OpenStreetMap

## Table of Content

- Basic features of OSM
- B
- C
- d

---

## Introduction of OSM

### 1. sign in for OSM

### 2. Walkthrough of OSM:

- The walkthrough teaches how to edit the points, lines, and areas.
- we use the word "features" to describe the things that appear on the map. Map features aree represented by points, lines, and areas.
- when a feature is selected, the feature editor is displayed alongside the map. (for street, it can show oneway, number of lanes and other attributes.)
- Points: used to represent features such as shops, restaurants and monuments. (we can add a point in the map and edit its information)
- Areas: used to show the boundaries of features like buildings and residential areas.
- Lines: used to represent features such as roads, railroads, and rivers. (we can create, edit the roads and its connecting nodes)

## 3. OSM data introduction

- Data format

  - .osm: it is a data format specific to OSM. This data format is designed to be easily sent and received across the internet. .osm files coded in XML, and contain geographical data in structured, ordered format.
  - .osm.bz2 and .osm.pbf are essentially .osm files. They are compressed format for the purpose of saving space.
  - shapefile (.shp): shapefiles are actually a collection of several different files. A shapefile that contains building data might have files with the following extensions: .shp, .shx and .dbf. A shapefile must be designated to hold only one type of features (points, lines, or polygons). **.osm data can be converted into shapefiles.**
  - Database: OpenStreetMap data is often stored in a PostgreSQL db with PostGIS extensions. There are tools for importing raw OSM data into a PostgreSQL db. Another type of db is known as SQLite.

- Get OSM data
  - [GeoFabrik](http://download.geofabrik.de/) : .shp and .osm data are provided, and the data is updated everyday. However, the data is extracted by continent or contries.
  - [BBBike](https://download.bbbike.org/osm/bbbike/): provides .shp and .osm data for cities around the workd. This is useful if you are looking for data extracts for a single city.
  - HOT export
  - Overpass API: can extract arbitrary amount of data. You can either use the API directly by generating a http-request or through the overpass turbo interface. the overpass turbo API [HERE](http://overpass-turbo.eu/) and the docs [HERE](https://wiki.openstreetmap.org/wiki/Overpass_API/Overpass_QL), and examples of using the API [HERE](https://wiki.openstreetmap.org/wiki/Overpass_API/Overpass_API_by_Example).
- Use OSM data
  - Use OSM data with QGIS: install the plugin QuickOSM.
  - Store OSM data into a PostgreSQL database (what are PostgreSQL and PostGIS?- know more about spatial indexing and ORDBMS)
