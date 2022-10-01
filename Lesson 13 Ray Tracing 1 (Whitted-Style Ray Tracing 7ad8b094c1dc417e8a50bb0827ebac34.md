# Lesson 13 Ray Tracing 1 (Whitted-Style Ray Tracing)

Created: October 1, 2022 1:31 PM

### Contents

---

[Why Ray Tracing?](https://www.notion.so/Lesson-13-Ray-Tracing-1-Whitted-Style-Ray-Tracing-7ad8b094c1dc417e8a50bb0827ebac34)

**Basic Ray-Tracing Algorithm**

[Three ideas about light rays](https://www.notion.so/Lesson-13-Ray-Tracing-1-Whitted-Style-Ray-Tracing-7ad8b094c1dc417e8a50bb0827ebac34)

[Ray Casting](https://www.notion.so/Lesson-13-Ray-Tracing-1-Whitted-Style-Ray-Tracing-7ad8b094c1dc417e8a50bb0827ebac34)

[Generating Eye Rays: **Pinhole Camera Model**](https://www.notion.so/Lesson-13-Ray-Tracing-1-Whitted-Style-Ray-Tracing-7ad8b094c1dc417e8a50bb0827ebac34)

**[Recursive (Whitted-Style) Ray Tracing](https://www.notion.so/Lesson-13-Ray-Tracing-1-Whitted-Style-Ray-Tracing-7ad8b094c1dc417e8a50bb0827ebac34)**

1. **[Ray-Surface Intersection](https://www.notion.so/Lesson-13-Ray-Tracing-1-Whitted-Style-Ray-Tracing-7ad8b094c1dc417e8a50bb0827ebac34)**
    
    (1) **[Ray Equation](https://www.notion.so/Lesson-13-Ray-Tracing-1-Whitted-Style-Ray-Tracing-7ad8b094c1dc417e8a50bb0827ebac34) (2) [Ray Intersection With Sphere](https://www.notion.so/Lesson-13-Ray-Tracing-1-Whitted-Style-Ray-Tracing-7ad8b094c1dc417e8a50bb0827ebac34)
    (3) [Ray Intersection With Triangle Mesh](https://www.notion.so/Lesson-13-Ray-Tracing-1-Whitted-Style-Ray-Tracing-7ad8b094c1dc417e8a50bb0827ebac34) 
    (*Moller Trumbore (MT) Algorithm)**
    
2. **[Accelerating Ray-Surface Intersection](https://www.notion.so/Lesson-13-Ray-Tracing-1-Whitted-Style-Ray-Tracing-7ad8b094c1dc417e8a50bb0827ebac34)**

**[Bounding Volumes](https://www.notion.so/Lesson-13-Ray-Tracing-1-Whitted-Style-Ray-Tracing-7ad8b094c1dc417e8a50bb0827ebac34)**

**[Ray Intersection with Axis-Aligned Box](https://www.notion.so/Lesson-13-Ray-Tracing-1-Whitted-Style-Ray-Tracing-7ad8b094c1dc417e8a50bb0827ebac34)**

(1)  **[2D example](https://www.notion.so/Lesson-13-Ray-Tracing-1-Whitted-Style-Ray-Tracing-7ad8b094c1dc417e8a50bb0827ebac34) (2) [3D case](https://www.notion.so/Lesson-13-Ray-Tracing-1-Whitted-Style-Ray-Tracing-7ad8b094c1dc417e8a50bb0827ebac34) (3)** [Some problems about 3D case](https://www.notion.so/Lesson-13-Ray-Tracing-1-Whitted-Style-Ray-Tracing-7ad8b094c1dc417e8a50bb0827ebac34)

---

**Why Ray Tracing?**

Rasterization couldn’t handle **global** effects well

-(Soft) shadows

-And especially when the light bounces **more than once**

![Untitled](Lesson%2013%20Ray%20Tracing%201%20(Whitted-Style%20Ray%20Tracing%207ad8b094c1dc417e8a50bb0827ebac34/Untitled.png)

- Rasterization is fast, but quality is relatively low
- Ray tracing is accurate, but is **very slow**
    
    -Rasterization: **real-time**, ray tracing: **offline**
    
    -~10k CPU core hours to render **one frame** in production
    

# Basic Ray-Tracing Algorithm

### **Three ideas about light rays:**

(1) Light travels in straight lines (though this is wrong)

(2) Light rays do not "collide" with each other if they cross (though this is still wrong)
(3) Light rays travel from the light sources to the eye (but the physics is invariant under path reversal - reciprocity)

## **Ray Casting**（光线投射）

Appel 1968 - Ray casting

(1) Generate an image by casting one ray per pixel

(2) Check for shadows by sending a ray to the light

![Untitled](Lesson%2013%20Ray%20Tracing%201%20(Whitted-Style%20Ray%20Tracing%207ad8b094c1dc417e8a50bb0827ebac34/Untitled%201.png)

### Generating Eye Rays: **Pinhole Camera Model**

1. 从eyepoint向场景中投出射线，寻找最近的与物体的交点

![Untitled](Lesson%2013%20Ray%20Tracing%201%20(Whitted-Style%20Ray%20Tracing%207ad8b094c1dc417e8a50bb0827ebac34/Untitled%202.png)

1. 从light到交点连线，判断是否处于阴影，之后计算着色信息，再把信息返回到image plane

![Untitled](Lesson%2013%20Ray%20Tracing%201%20(Whitted-Style%20Ray%20Tracing%207ad8b094c1dc417e8a50bb0827ebac34/Untitled%203.png)

以上其实只考虑了光线经过一次折射/反射，下面能够解决多次折射/反射

### **Recursive (Whitted-Style) Ray Tracing**

"An improved Illumination model for shaded display" - T.Whitted, CACM 1980

(1) 透明物体圆球，发生了反射和折射

![Untitled](Lesson%2013%20Ray%20Tracing%201%20(Whitted-Style%20Ray%20Tracing%207ad8b094c1dc417e8a50bb0827ebac34/Untitled%204.png)

(2) 每个点都与light连线，判读是否在阴影中，并计算着色信息，全部返回给eye
（再定义：primary ray，即eye ray；发生折射、反射后的光线为secondary ray）

![Untitled](Lesson%2013%20Ray%20Tracing%201%20(Whitted-Style%20Ray%20Tracing%207ad8b094c1dc417e8a50bb0827ebac34/Untitled%205.png)

其中有许多技术问题，下面一一讲解

1. **Ray-Surface Intersection**（交点怎么求）

(1) **Ray Equation**

Ray is defined by its origin and a direction vector
（实际就是射线，由起点和方向定义）

$Ray\ equation:r(t)=o+td,\ 0\le t\lt \infty$

![Untitled](Lesson%2013%20Ray%20Tracing%201%20(Whitted-Style%20Ray%20Tracing%207ad8b094c1dc417e8a50bb0827ebac34/Untitled%206.png)

(2) **Ray Intersection With Sphere**

$Ray:\ r(t)=o+td,\ 0\le t\lt \infty \\
Sphere:\ p:(p-c)^{2}-R^{2}=0$

The intersection **p** must satisfy both **ray equation** and **sphere equation**

![Untitled](Lesson%2013%20Ray%20Tracing%201%20(Whitted-Style%20Ray%20Tracing%207ad8b094c1dc417e8a50bb0827ebac34/Untitled%207.png)

Solve for intersection: $(o+td-c)^{2}-R^{2}=0$

$at^{2}+bt+c=0,where\\
a=d\cdot d\\
b=2(o-c)\cdot d\\
c=(o-c)\cdot (o-c)-R^{2}\\
t=\cfrac{-b\pm\sqrt{b^{2}-4ac}}{2a}$

![Untitled](Lesson%2013%20Ray%20Tracing%201%20(Whitted-Style%20Ray%20Tracing%207ad8b094c1dc417e8a50bb0827ebac34/Untitled%208.png)

*Ray Intersection with Implicit Surface

Ray: $r(t)=o+td,0\le t\lt \infty$

General implicit surface: $p:f(p)=0$

Substitute ray equation: $f(o+td)=0$

Solve for **real, positive** roots

(3) **Ray Intersection With Triangle Mesh**

-Rendering: visibility, shadows, lighting …

-Geometry: inside/outside test
(从物体内部向外射线与封闭表面相交，交点奇数个在内、偶数个在外，2D 3D都适用)

![Untitled](Lesson%2013%20Ray%20Tracing%201%20(Whitted-Style%20Ray%20Tracing%207ad8b094c1dc417e8a50bb0827ebac34/Untitled%209.png)

Triangle is in a plane: Ray-plane intersection, test if hit point is inside triangle

$Plane\ Equation:\ p:(p-p')\cdot N=0$

Solve for intersection: $(p-p')\cdot N=(o+td-p')\cdot N=0$

$t=\cfrac{(p'-o)\cdot N}{d\cdot N}\\ Check:0\le t\lt \infty$

![Untitled](Lesson%2013%20Ray%20Tracing%201%20(Whitted-Style%20Ray%20Tracing%207ad8b094c1dc417e8a50bb0827ebac34/Untitled%2010.png)

*一种快速判断是否在三角形内的方法：
**Moller Trumbore (MT) Algorithm**

-A faster approach, giving barycentric coordinate directly

-Derivation in the discussion section

![Untitled](Lesson%2013%20Ray%20Tracing%201%20(Whitted-Style%20Ray%20Tracing%207ad8b094c1dc417e8a50bb0827ebac34/Untitled%2011.png)

1. **Accelerating Ray-Surface Intersection** (Performance Challenges)

Simple ray-scene intersection:

-Exhaustively test ray-intersection with every object

-Find the closest hit (with minimum t)

Problem:

-Naive algorithm = #pixels x # objects (x #bounces)

-Very slow!

**Bounding Volumes**（包围盒）

Quick way to avoid intersections: bound complex object with a simple volume

-Object is fully contained in the volume
-If it doesn't hit the volume, it doesn't hit the object
-So test BVol first, then test object if it hits

![Untitled](Lesson%2013%20Ray%20Tracing%201%20(Whitted-Style%20Ray%20Tracing%207ad8b094c1dc417e8a50bb0827ebac34/Untitled%2012.png)

Understanding: **box is the intersection of 3 pairs of slabs**（三组对面形成的）

Specifically: We often use an Axis-Aligned Bounding Box (AABB)（轴对齐包围盒）
i.e. any side of the BB is along either x, y, or z axis

以下介绍光线与AABB相交

![Untitled](Lesson%2013%20Ray%20Tracing%201%20(Whitted-Style%20Ray%20Tracing%207ad8b094c1dc417e8a50bb0827ebac34/Untitled%2013.png)

**Ray Intersection with Axis-Aligned Box**

(1) (3D is the same) **2D example**: 
Compute intersections with slabs and take intersection of t_min / t_max intervals
计算射线与两组对面相交时的两组t_min（进时间），t_max（出时间）；而后再对得到的两组进出时间取交集，得到最终进出时间（图3）

![Untitled](Lesson%2013%20Ray%20Tracing%201%20(Whitted-Style%20Ray%20Tracing%207ad8b094c1dc417e8a50bb0827ebac34/Untitled%2014.png)

(2) **3D case** (three pairs of infinitely large slabs)

Key idea: The ray **enters** the box **only when** it enters all pairs of slabs;
The ray **exits** the box **as long as** it exits any pair of slabs

-For each pair, calculate the t_min and t_max (negative is fine)

-For the 3D box, $t_{enter}=max\{t_{min}\},\ t_{exit}=min\{t_{max}\}$

-If t_enter < t_exit, we know the rays **stays a while** in the box
(so they must intersect)

(3) **Some problems about 3D case**

However, ray is not a line（不是直线）: Should check whether t is negative

t_exit < 0: The box is “behind” the ray - no intersection

t_exit ≥ 0 and t_enter < 0（比如光线起点再盒子内）: have intersection

SUM: **Ray and AABB intersect only if**  $t_{enter}\lt t_{exit}\  \&\& \ t_{exit}\ge 0$

*Why Axis-Aligned?

![Untitled](Lesson%2013%20Ray%20Tracing%201%20(Whitted-Style%20Ray%20Tracing%207ad8b094c1dc417e8a50bb0827ebac34/Untitled%2015.png)