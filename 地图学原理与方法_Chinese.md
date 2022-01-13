## 第二章：地图学基础

### 基本概念：

1. 大地水准面 （geoid)： the hypothetical shape of the earth, coinciding with mean sea level and its imagined extension under or over land areas.

2. 地球椭球体 (earth ellipsoid): a mathematical figure approximating the earth's form, used a s a reference frame for computations in geodesy, astronomy, and geosciences.

3. 参考椭球体 (reference ellipsoid): commonly used reference ellipsoid: Bessel ellipsoid, Hayford ellipsoid, WGS84 ellipsoid

4. 大地基准面 (geodetic datum) : a global datum reference for precisely measuring locations on Earth. Local datum (xian, beijing), and global datum: WGS84

   椭球体和大地基准面是一对多的关系，同一个椭球体可以用于多个不同的基准面。基准面通过平移，旋转，以及缩放来进行互相转换。

5. 地理坐标系 (geographical coordinate system): 分为地心坐标系和参心坐标系。指用经纬度表示地面点位的球面坐标系。

6. 大地坐标系：

- 大地经纬度坐标系 (L,B,H)
- 空间直接坐标系 (X, Y, Z)

7. 高程 elevation

8. 大地控制网 Geodetic control:

9. GPS 全球定位系统 (global positioning system): a satallite-based radionavigation system owned by US. GPS works in any weather conditions, anywhere in the world with no subscrition fees or setup charges.

10. 常见坐标系统

WKID： well-known ID (can be found at epsg.io)

WGS84: WKID=4326

CGCS2000: WKID=4490

NAD27

NAD83

### 地图投影基本理论：

```
x=f1(B,L)
y=f2(B,L)
```

1. 投影的类型：

- 透视方位投影
- 透视圆锥投影
- 透视圆柱投影

2. 三种形变：

- 长度形变
- 面积形变
- 角度形变
