# Lesson 09. Shading 3 (Texture Mapping Cont.)

Created: September 27, 2022 11:08 PM

### Contents

---

[Interpolation Across Triangles: Barycentric Coordinates](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b.md)

[Barycentric coordinates](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b.md)

[Texture Magnification](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b.md) (像素多而纹素少): Nearest, Bilinear, Bicubic

[Texture Minification](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b.md) (像素少而纹素多): (solution) Supersampling or Mipmap

[Mipmap](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b.md)

查询D不连续问题与解决：[Trilinear Interpolation](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b.md)

[Mipmap limitations: overblur](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b.md)

solution: (1) [Anisotropic filtering](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b.md)

(2) [EWA filtering](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b.md)

---

### Recap

Shading 1 & 2

-Blinn-Phong reflectance model

-Shading models / frequencies

-Graphics Pipeline

-Texture mapping

## This Course

> **Shading 3**
    -Barycentric coordinates
    -Texture queries
    -Applications of textures
**Shadow mapping**
> 

### Interpolation Across Triangles: Barycentric Coordinates（重心坐标）

1. **Why do we want to interpolate?**
    
    -Specify values at vertices
    
    -Obtain smoothly varying values across triangles
    

We interpolate (插值的内容): Texture coordinates, colors, normal vectors, …

We interpolate by: **Barycentric coordinates**

1. **Barycentric coordinates（重心坐标系）**

**A coordinate system** for triangles (α, β, γ)**（如上，实际上是指一个坐标系）**

![Untitled](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b/Untitled.png)

![Untitled](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b/Untitled%201.png)

由于α + β + γ = 1，则实际上只需要两个数即可表示；三者都非负，则重心在三角形内

如果α = β = γ = 1/3，则得到重心

![Untitled](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b/Untitled%202.png)

(Barycentric coordinates are not invariant under projection! 投影后的重心坐标可能会发生变化)

1. Applying Textures

Simple Texture Mapping: Diffuse Color

```csharp
for each rasterized screen sample(x,y):  // Usually a pixel's center
	(u,v) = evaluate texture coordinate at (x,y)  // Using barycentric coordinates!
	texcolor = texture.sample(u,v);
	set sample's color to texcolor;  // Usually the diffuse albedo Kd
```

1. Texture Magnification (**Easy Case**: What if the texture is too small?，**如墙面很大、像素多，而纹理很低清、纹素少)**

A pixel on a texture — a texel (纹理元素、纹素)

![Untitled](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b/Untitled%203.png)

很多pixel被映射到同一个texel上——纹理太小（即纹素太少）

(1) Nearest（取最近值，如四舍五入）

即像素映射到纹理时，纹理太小（纹素太少），因此像素可能会映射到非整数坐标的纹理处，Nearest则通过近似值方法解决

(2) Bilinear Interpolation（双线性插值）**（以下图片背景是texture，每个黑点代表整数纹理坐标）**

![Untitled](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b/Untitled%204.png)

![Untitled](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b/Untitled%205.png)

![Untitled](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b/Untitled%206.png)

(s, t)都在0~1间

**Linear interpolation (1D)**
$lerp(x, v0, v1)= v0 + x(v1-v0)$

(插值lerp这一操作的定义)

![Untitled](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b/Untitled%207.png)

**Two helper lerps (horizontal)**

$u0=lerp(s,u00,u10)\\u1=lerp(s,u01,u11)$

**Final vertical lerp, to get result:**

$f(x,y)=lerp(t,u0,u1)$

因此，红点即考虑了周围u00, u01 u10, u11四个点的信息，使像素过渡更加平滑

(3) Bicubic（双三次）

任意一点取周围16个点，每次使用4个点做一次非线性插值（上文的Bilinear取了4个）

1. Texture Minification (**Hard Case**: What if the texture is too large?)
    
    Point Sampling Textures - Problem (远处摩尔纹，近处严重锯齿)
    

![Untitled](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b/Untitled%208.png)

![Untitled](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b/Untitled%209.png)

（背景同样是texture）左：一块像素映射到较小纹理区域，因此作插值；

右：一块像素映射到较大纹理区域**（如墙面很小、像素少，而纹理很高清、纹素多）**，无法简单的去采样

Solution：(1) [Supersampling](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753.md)（←链接点回rasterization部分内容，利于思考）

如MSAA，即每个像素内部增加采样点以多次采样（原每个像素只采样一次）

但是代价很大

(2) Get the average value within a range

Point Query vs. (Avg.) Range Query

（点查询：给出点，如双线性插值也属于此；范围查询：给定区域立刻可得所需值，此处需要的是平均值）

Mipmap给出解决方案

---

### Mipmap - Allowing (fast, approx., square) range queries

![Untitled](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b/Untitled%2010.png)

![Untitled](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b/Untitled%2011.png)

1. or So-called “Image Pyramid” in CG

最后所占存储量：仅仅是原先的4/3

（等比数列求和问题）

![Untitled](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b/Untitled%2012.png)

如蓝、红色点组、分别从屏幕坐标 (x, y) 映射到纹理坐标 (u, v)，然后进行一顿运算

![Untitled](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b/Untitled%2013.png)

求红点间距（通常有规定的两组，这里用微分表示，因为从xy到uv是用函数实现的），取最大值为L

助于理解：右图粉红区域可以近似一个正方形来代表这个区域，其边长就是L

![Untitled](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b/Untitled%2014.png)

因为Mipmap每level 是前一level mipmap的像素（纹素）数量的1/4（如128x128 - 64x64），所以做底数为2的对数运算即可得到D（举例：L大，说明一个像素映射到许多纹素，使用像素更低的纹理，则level高）

1. **关于通过L查询到的D不连续的问题 - Trilinear Interpolation**

![Untitled](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b/Untitled%2015.png)

**Trilinear Interpolation（三线性插值）**

![Untitled](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b/Untitled%2016.png)

在D、D+1层分别做双线性插值，之后再做一次插值（层间），即成为三线性插值

（而且本身计算开销并不大，非常的好用）

1. **Mipmap Limitations - Overblur**

![Untitled](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b/Untitled%2017.png)

![Untitled](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b/Untitled%2018.png)

**Solution** (to part of the problem): 

(1) **Anisotropic Filtering（各向异性过滤）**

**Ripmap**s and summed area tables

-Can look up axis-aligned rectangular zones

-Diagonal footprints still a problem

每行高度不变，宽度每次变为原1/2；每列宽度不变，高度每次变为原1/2

代价：内存变为原3倍

（游戏中“多少多少X”，即计算到了多少层，如两层即左上角2x2区域，显存足够开最高即可）

![Untitled](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b/Untitled%2019.png)

![Untitled](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b/Untitled%2020.png)

Irregular Pixel Footprint in Texture

Screen space中一个像素映射到Texture space中有可能是不规则形状（如长条形），并不总是正方形；
如果仍使用正方形（即Mipmap）来采样，会得到一个比本来所需更大正方形区域的平均值，则更加模糊，发生overblur

![Untitled](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b/Untitled%2021.png)

如右图左上的长条是斜着摆放的，因此各向异性过滤仍不能完美解决问题，因此说是解决了一部分问题（对各种矩形的采样）

(2) EWA filtering

-Use multiple lookups

-Weighted average

-Mipmap hierarchy still helps

-Can handle irregular footprints

（多次查询一个椭圆区域，但是计算代价相应变大）

![Untitled](Lesson%2009%20Shading%203%20(Texture%20Mapping%20Cont%20)%20dcd61812fc4e4790bd26acc40d15537b/Untitled%2022.png)