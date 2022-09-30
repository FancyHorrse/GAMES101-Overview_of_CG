# Lesson 10 Geometry (Introduction)

Created: September 29, 2022 12:36 AM

### Contents

---

[Applications of textures](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6.md)

1. [Environment map](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6.md)
    
    [(1) **Spherical Environment Map**](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6.md)
    
    [(2) **Cube Map**](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6.md)
    
2. [Textures can affect shading](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6.md)
    
    [(1) **Bump Mapping**](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6.md)
    
    [(2) Displacement mapping](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6.md)
    
3. [3D Procedural Noise + Solid Modeling](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6.md)
4. [Provide Precomputed Shading](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6.md)
5. [3D Textures and Volume Rendering](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6.md)

Introduction to Geometry

[Examples of geometry](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6.md)

1. **[“Implicit” Representations of Geometry](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6.md)**
2. **[“Explicit” Representations of Geometry](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6.md)**

**[More Implicit Representations in Computer Graphics](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6.md)**

(1) Algebraic Surfaces; (2) Constructive Solid Geometry; (3) Distance Functions;
(4) Level Set Methods; (5) Fractals

[Implicit Representations - Pros & Cons](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6.md)

---

### Recap

Shading 1 & 2

-Blinn-Phong reflectance model

-Shading models / frequencies

-Graphics Pipeline

-Texture mapping

Shading 3

-Barycentric coordinates

-Texture antialiasing (MIPMAP)

-Applications of textures

## This Course

> Applications of textures
Introduction to geometry (2nd part of this course!)
    -Examples of geometry
    -Various representations of geometry
> 

---

### Applications of textures

In modern GPUs, texture = memory + range query (filtering)

= 一块内存，且可以做很快的查询

Many applications: Environment lighting, Store microgeometry, Procedural textures, Solid modeling, Volume rendering

1. Environment Map（用纹理表述环境光）（犹他茶壶 from 犹他大学）

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled.png)

默认环境光来自无限远处，因此环境光照只记录光照方向，不记录强度

(1) **Spherical Environment Map**（环境光记录在球面上）

Environment map (left) used to render realistic lighting

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%201.png)

Problem: prone to distortion (top and bottom parts)（如世界地图上，出现扭曲现象）

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%202.png)

(2) Solution: **Cube Map**

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%203.png)

A vector maps to cube point along that direction. The cube is textured with 6 square texture maps.

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%204.png)

1. Textures can affect shading

Textures doesn’t have to only represent colors:

It stores height / normal: **Bump** / **normal** mapping

（凹凸贴图/法线贴图：定义相对高度/直接定义法线方向，本质上都是为了改变法线）

Fake the detailed geometry

定义了对于基础表面，某一像素的相对高度（即法线发生变化，进而改变shading结果）

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%205.png)

(1) **Bump Mapping**

Adding surface detail without adding more triangles

-Perturb（扰动） surface normal per pixel (for shading computations only)

-”Height shift” per texel defined by a texture

（黑线为原本物体表面，黄线是经Bump texture凹凸贴图定义的表面）

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%206.png)

**How to perturb the normal**

(in flatland) Original surface normal n(p) = (0, 1)
Derivative at p is dp = c * [h(p+1) - h(p)]（切线）
Perturbed **normal** is then n(p) = (-dp, 1).normalized()（切线(1, dp)旋转得到法线）

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%207.png)

（in 3D）Original surface normal n(p) = (0, 0, 1)
Derivatives at p are -dp/d**u** = c1 * [h(**u**+1) - h(**u**)]（切线）
-dp/d**v** = c2 * [h(**v**+1) - h(**v**)]
Perturbed **normal** is then n(p) = (-dp/du, -dp/dv, 1).normalized()（法线）

Note that this is in **local coordinate**

(2) **Displacement mapping（位移贴图）**: a more advanced approach

-Uses the same texture as in bumping mapping

-Actually **moves the vertices**（真的移动了顶点）

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%208.png)

凹凸贴图在边缘、产生阴影处会露馅，因为实际上没有改变几何；
位移贴图要求物体建模足够细致，即三角形够多，否则不能完整反应纹理的变化；
（DirectX的动态曲面细分，能先判断是否需要细分更多三角形以使用位移贴图）

1. 3D Procedural Noise + Solid Modeling（三维的纹理，每个点；噪声函数）

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%209.png)

1. Provide Precomputed Shading

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%2010.png)

着色结果 * 提前生成好的环境光遮蔽纹理(Ambient occlusion，以后会讲) = 最终结果

1. 3D Textures and Volume Rendering（右图核磁共振，纹理记录了三维空间中每个点的信息）

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%2011.png)

---

## Introduction to geometry

### Examples of geometry

布料：**Fibe**r（纤维）-(扭曲)- **Ply** (一**股**纤维)  -(扭曲)- **Thread** -(扭曲)- 各种不同形状

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%2012.png)

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%2013.png)

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%2014.png)

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%2015.png)

Many Ways to Represent Geometry

Implicit（隐式几何）

-algebraic surface

-level sets

-distance functions

-…

Explicit（显式几何）

-point cloud

-polygon mesh

-subdivision, NURBS

-…

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%2016.png)

1. **“Implicit” Representations of Geometry**

Based on classifying points

-Points satisfy some specified relationship（如圆形：满足$x^{2}+y^{2}+z^{2}=1$的所有点）

满足$f(x,y,z)=0$的所有点

Implicit Surface - Sampling Can Be Hard

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%2017.png)

Inside / Outside Tests Easy

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%2018.png)

1. **“Explicit” Representations of Geometry**
    
    All points are **given directly** or **via parameter mapping**（参数映射）
    

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%2019.png)

Explicit Surface - Sampling Is Easy

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%2020.png)

Inside / Outside Tests Hard

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%2021.png)

**More Implicit Representations in Computer Graphics**

(1) Algebraic Surfaces（代数表达式）：不直观

(2) Constructive Solid Geometry（CSG构造实体几何）：
Combine implicit geometry via Boolean operations（通过布尔运算，即取交、并集等）

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%2022.png)

(3) Distance Functions（距离函数）：
Instead of Booleans, gradually **blend** surfaces together using **Distance Functions**: giving minimum distance (could be **signed** distance) from anywhere to object（空间中任意一点到物体表面的最小距离，可正可负）
之后将Distance Function转换回物体表面的操作，实际上是找使距离函数的值为0的点

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%2023.png)

对Blend Distance Functions的解释：

A（占1/3）+B（占2/3）希望得到占1/2，而blend后得到1/3黑1/3灰1/3白
SDF (Signed Distance Function): 函数值设置为到这条边的距离(左负右正，越近绝对值越小)，此时blend后再从SDF恢复原来颜色，得到占1/2的结果（实际上在做边的blend）

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%2024.png)

(4) Level Set Methods

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%2025.png)

此方法实际上就是用格子描述距离，同样找到0值处，可找到物体表面（类似等高线，同为xx值的一条线）

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%2026.png)

(5) Fractals（分形）

Exhibit self-similarity, detail at all scales
"Language" for describing natural phenomena
Hard to control shape!

![Untitled](Lesson%2010%20Geometry%20(Introduction)%2025fc1ea24f694132af76b05e5bf1b8e6/Untitled%2027.png)

**Implicit Representations - Pros & Cons**

Pros: 
compact description (e.g. a function)
certain queries easy (inside object, distance to surface)
good for ray-to-surface intersection (more later)
for simple shapes, exact description / no sampling error
easy to handle changes in topology (e.g. fluid)

Cons:
difficult to model complex shapes