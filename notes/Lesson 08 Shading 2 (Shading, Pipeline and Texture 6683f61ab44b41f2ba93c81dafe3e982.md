# Lesson.08 Shading 2 (Shading, Pipeline and Texture Mapping)

Created: September 26, 2022 11:29 PM

### Contents

---

-Blinn-Phong reflectance model
    -[Specular](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982.md) and [ambient](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982.md) terms
-[Shading frequencies](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982.md)

-Flat, Gouraud, Phong shading

-[Graphics pipeline](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982.md)

-[Texture mapping](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982.md)

-[Barycentric coordinates](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982.md)

---

### Review

Shading 1

-Blinn-Phong reflectance model

-Diffuse

-Specular

-Ambient

-At a specific shading point

![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled.png)

> Shading 2
-Blinn-Phong reflectance model
    -Specular and ambient terms
-Shading frequencies
-Graphics pipeline
-Texture mapping
-Barycentric coordinates
> 

## This Course

---

### Specular Term (Blinn-Phong): **Depends** on view direction

Bright near mirror reflection direction (**R**)

![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled%201.png)

V close to mirror direction == half vector near normal (**h**)
(measure “near” by dot product of unit vectors)

![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled%202.png)

(Blinn-Phong模型使用I与v的半程向量h与n比较(计算量小)，而直接使用v和R比较的是Phong模型)

[ Notice the “p” ] Increasing p narrows the reflection lobe
即希望高光仅存在于非常小的区域（cos α 高光区域太大），通常 p = 100~200

![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled%203.png)

![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled%204.png)

### Ambient Term: does not depend on anything

-Add constant color to account for disregarded illumination and fill in black shadows

-This is approximate / fake! （实际上是常数，几乎就是物体表面颜色）

![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled%205.png)

### Blinn-Phong Reflection Model

![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled%206.png)

---

### Shading frequencies (着色频率)

Shading 应用在每个片元上、顶点上、像素上（逐片元、逐顶点、逐像素）：

![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled%207.png)

1. Shade each triangle (**Flat shading**)

-Triangle face is flat — one normal vector

-Not good for smooth surfaces

![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled%208.png)

1. Shade each vertex (**Gouraud shading**)

-Interpolate colors from vertices across triangle

-Each vertex has a normal vector

![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled%209.png)

1. Shade each pixel (**Phong shading**)

-Interpolate normal vectors across each triangle

-Compute full shading model at each pixel

-**Not the Blinn-Phong Reflectance Model**

![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled%2010.png)

![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled%2011.png)

（Num Vertices增加：即模型变复杂的时候，三者或许会有不同的着色结果）

1. Defining Per-Vertex Normal Vectors（顶点的法线是什么？）

![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled%2012.png)

相邻面法线求平均

Defining Per-Pixel Normal Vectors

Barycentric（质心的） interpolation of vertex normals (should be normalized)

![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled%2013.png)

---

### Graphics (Real-time Rendering) Pipeline 图形渲染（实时渲染）管线

![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled%2014.png)

**Vertex Processing** - Model, View, Projection transforms （MVP变换）

![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled%2015.png)

**Rasterization** - Sampling triangle coverage（采样）

![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled%2016.png)

**Fragment Processing** - Z-Buffer Visibility Tests（深度测试）

![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled%2017.png)

**Fragment Processing & Vertex Processing** - Shading

![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled%2018.png)

（可编程部分，即代码(称为shader)决定着色在哪一步进行）

**Fragment Processing & Vertex Processing** - Texture mapping（纹理映射）

![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled%2019.png)

**Shader Programs**

-Program vertex and fragment processing stages

-Describe operation on a single vertex (or fragment)

Example GLSL fragment shader program

![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled%2020.png)

(每个顶点/片元执行一次，顶点着色器、片元着色器)

---

### Texture Mapping（纹理映射）

1. **Surfaces are 2D**

Every 3D surface point also has a place where it goes in the 2D image (**texture**)

![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled%2021.png)

1. **Texture applied to surface**

![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled%2022.png)

1. **Visualization of Texture Coordinates**
    
    Each triangle vertex is assigned a texture coordinate (u, v) (u, v 都在0~1间)
    
    ![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled%2023.png)
    

1. **Textures can be used multiple times (Tilable texture 可无限平铺)**

![Untitled](Lesson%2008%20Shading%202%20(Shading,%20Pipeline%20and%20Texture%206683f61ab44b41f2ba93c81dafe3e982/Untitled%2024.png)

---

### Interpolation Across Triangles: Barycentric Coordinates

in the next lesson~