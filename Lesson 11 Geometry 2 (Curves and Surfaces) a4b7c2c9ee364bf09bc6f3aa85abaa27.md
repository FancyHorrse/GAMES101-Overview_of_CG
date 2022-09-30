# Lesson 11 Geometry 2 (Curves and Surfaces)

Created: September 29, 2022 1:58 PM

### Contents

---

[Explicit Representations in CG](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27.md)

Point Cloud, Polygon Mesh

**Curves**

[Bezier Curves](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27.md)

[De Casteljau’s algorithm](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27.md)

1. Examples: (1) [Consider three points](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27.md); (2) [Cubic Bezier Curve](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27.md)
2. [Algebraic Formula](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27.md)
3. [Properties of Bezier Curves](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27.md)
4. [Piecewise Bezier Curves](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27.md)

[Other types of splines](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27.md): Splines, B-splines

Surfaces

[Bezier surfaces](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27.md)

[Mesh operations: Geometry processing](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27.md)

---

### Recap

Introduction to geometry

-Examples of geometry

-Various representations of geometry

-Implicit

-Explicit

## This Course

> -Explicit Representations
Curves
    -Bezier curves
    -De Casteljau’s algorithm
    -Brief introduction of B-splines, etc.
Surfaces
    -Bezier surfaces
    -Triangles & quads
        -Subdivision, simplification, regularization
> 

---

### Explicit Representations in CG

triangle meshes, Bezier surfaces, subdivision surfaces, NURBS, point clouds

1. Point Cloud（点云）
    
    ![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled.png)
    

1. Polygon Mesh
    
    Store vertices & polygons (often triangles or quads)
    
    Easier to do processing / simulation, adaptive sampling
    
    More complicated data structures
    
    Perhaps most common representation in graphics
    
    ![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%201.png)
    

**The Wavefront Object File (.obj) Format**

Commonly used in Graphics research

Just a text file that sepcifies vertices, normals, texture coordinates and their connectivities

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%202.png)

---

## Curves

For example: Camera Paths, Animation Curves, Vector Fonts

### Bezier Curves（贝塞尔曲线）

Defining Cubic Bezier Curve With Tangents

下图定义了起始点、终止点为p0, p3，起始、终止切线为t0, t1

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%203.png)

### De Casteljau’s algorithm

给定任意多个点，如何计算出贝塞尔曲线？

1. For example:

(1) Consider three points (quadratic Bezier)

例如从b0到b1经过时间1，那么问题被转化成：找到时间t的点在哪

Insert a point using linear interpolation

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%204.png)

Insert on both edges

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%205.png)

Repeat recursively（递归地）

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%206.png)

Run the same algorithm for every t in [0, 1] 
（t从0~1变化时，记录所有图中点 $b^{2}_{0}$ 的位置，即得到贝塞尔曲线）

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%207.png)

(2) Cubic Bezier Curve - de Casteljau

Four input points in total
Same recursive linear interpolations

4点 3条线 - 3点 2条线 - 2点 1条线

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%208.png)

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%209.png)

1. **Algebraic Formula**

de Casteljau algorithm gives a pyramid of coefficients（系数）

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%2010.png)

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%2011.png)

Bernstein form of a Bezier curve of order n:

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%2012.png)

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%2013.png)

拓展：（Bernstein多项式的图像）

$B^{n}_{i}(t)=\begin{pmatrix}n\\i \end{pmatrix}t^{i}(1-t)^{n-i}=(下图中写法)=B_{i,n}(t)$

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%2014.png)

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%2015.png)

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%2016.png)

1. **Properties of Bezier Curves**

(1) Interpolates endpoints
-For cubic Bezier: $b(0)= b_{0};\ b(1)= b_{3}$

(2) Tangent to end segments（对4点3线情况，起始与终止切线斜率是确定的）
-Cubic case: $b'(0)= 3(b_{1}一 b_{0});\ b'(1)= 3(b_{3} 一 b_{2})$

(3) Affine transformation property（仿射变换后不变，即可平移，[链接到transformation课程处](Lesson%2003%202D%20Transforms%200439c099eb3241a98b69046240f45183.md)）
-Transform curve by transforming control points

(4) Convex hull property（凸包性质，贝塞尔曲线在几个控制点内）
-Curve is within convex hull of control points

1. **Piecewise（逐段的） Bezier Curves**

控制点多了后，不是很方便操控
因此逐段控制点（每四个点，即piecewise cubic Bezier），应用非常广泛

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%2017.png)

**关于连续**:：1-2为上一段曲线终止的线段，2-3为下一段曲线开始的线段，二者如果长度相等且共线，那么这两条曲线在点2处拼接是连续的（即切线相同）

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%2018.png)

$C^{0}\ continuity:a_{n}=b_{0}$

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%2019.png)

$C^{1}\ continuity:a_{n}=b_{0}=\frac{1}{2}(a_{n-1}+b_{1})$（切线也连续，可以理解成一阶导数连续）

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%2020.png)

### Other types of splines

1. Spline（样条）

-a continuous curve constructed so as to pass through a given set of points and have a certain number of continuous derivatives

- ln short, a curve under control

1. B-splines

-Short for basis splines（基函数样条，即伯恩斯坦多项式）

-Require more information than Bezier curves

-Satisfy all important properties that Bezier curves have (i.e. superset)

---

## Surfaces（曲面）

### Bezier surfaces

Extend Bezier curves to surfaces

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%2021.png)

**Bicubic Bezier Surface Patch**

Bezier surface and 4x4 array of control points

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%2022.png)

4条灰线为已得到的贝塞尔曲线，从它们上继续各取一点作贝塞尔曲线，遍历后得到贝塞尔曲线

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%2023.png)

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%2024.png)

**Evaluate Surface Position For Parameters (u, v)**

先用u来确定4条灰线上各一个点的位置，然后再用v确定蓝线上一个位置，得到坐标(u, v)对应的点
（这样也可以看出贝塞尔曲线是显示表示，因为使用了参数映射）

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%2025.png)

### Mesh Operations: Geometry Processing

Mesh subdivision（细分，图3）
Mesh simplification（图4）
Mesh regularization（图5）

![Untitled](Lesson%2011%20Geometry%202%20(Curves%20and%20Surfaces)%20a4b7c2c9ee364bf09bc6f3aa85abaa27/Untitled%2026.png)