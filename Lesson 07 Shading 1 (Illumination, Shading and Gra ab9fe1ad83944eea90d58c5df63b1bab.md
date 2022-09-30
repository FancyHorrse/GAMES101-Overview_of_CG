# Lesson.07 Shading 1 (Illumination, Shading and Graphics Pipeline)

Created: September 25, 2022 9:24 PM

### Contents

---

上节课未讲完部分：Visibility / occlusion

1. [Painter’s Algorithm](Lesson%2007%20Shading%201%20(Illumination,%20Shading%20and%20Gra%20ab9fe1ad83944eea90d58c5df63b1bab.md)
2. [Z-Buffer](Lesson%2007%20Shading%201%20(Illumination,%20Shading%20and%20Gra%20ab9fe1ad83944eea90d58c5df63b1bab.md)

This Course

A simple shading model (Blinn-Phong Reflectance Model)

1. [Perceptual observations](Lesson%2007%20Shading%201%20(Illumination,%20Shading%20and%20Gra%20ab9fe1ad83944eea90d58c5df63b1bab.md) (Specular, Diffuse, Ambient)
2. [Compute light reflected toward camera at a specific shading point](Lesson%2007%20Shading%201%20(Illumination,%20Shading%20and%20Gra%20ab9fe1ad83944eea90d58c5df63b1bab.md)
3. [Diffuse Reflection](Lesson%2007%20Shading%201%20(Illumination,%20Shading%20and%20Gra%20ab9fe1ad83944eea90d58c5df63b1bab.md): [Lambertian (Diffuse) Shading](Lesson%2007%20Shading%201%20(Illumination,%20Shading%20and%20Gra%20ab9fe1ad83944eea90d58c5df63b1bab.md)

---

> Visibility / occlusion
> 
> 
> -Z-buffering
> 

> Shading
> 
> 
> -Illumination & Shading
> 
> -Graphics Pipeline
> 

## 上节课未讲完部分

### Visibility / occlusion

1. Painter’s Algorithm
    
    Paint from back to front, **overwrite** in the framebuffer
    
    Can have unresolvable depth order
    
    ![Untitled](Lesson%2007%20Shading%201%20(Illumination,%20Shading%20and%20Gra%20ab9fe1ad83944eea90d58c5df63b1bab/Untitled.png)
    

1. Z-Buffer
    
    Idea: Store current min. z-value **for each sample (pixel)**
    
    Needs an additional buffer for depth values
    
    -frame buffer stores color values
    
    -depth buffer (z-buffer) stores depth
    
    IMPORTANT: For simplicity we suppose **“z is always positive”**
    
    (smaller z → closer, larger z → further，原来的相机面朝-Z轴方向)
    

![Untitled](Lesson%2007%20Shading%201%20(Illumination,%20Shading%20and%20Gra%20ab9fe1ad83944eea90d58c5df63b1bab/Untitled%201.png)

During rasterization:

```csharp
for (each triangle T)
	for (each sample(x,y,z) in T)
		if (z < zbuffer[x,y])  // closest sample so fat
			framebuffer[x,y] = rgb;  // update color
			zbuffer[x,y] = z;  // update depth
		else
			;  // do nothing, this sample is occluded
```

Drawing triangles in different orders doesn’t change the result.

Most important visibility algorithm: Implemented in hardware for all GPUs

## This Course

### Recall what we’ve covered so far

![Untitled](Lesson%2007%20Shading%201%20(Illumination,%20Shading%20and%20Gra%20ab9fe1ad83944eea90d58c5df63b1bab/Untitled%202.png)

### Shading

 The process of applying a material to an object.

### A simple shading model (Blinn-Phong Reflectance Model)

1. **Perceptual observations**
    
    ![Untitled](Lesson%2007%20Shading%201%20(Illumination,%20Shading%20and%20Gra%20ab9fe1ad83944eea90d58c5df63b1bab/Untitled%203.png)
    

**Specular** 高光反射（光直线反射）

**Diffuse** 漫反射（表面不光滑，光向四面八方反射）

**Ambient** 环境光照（假设任何物体都会受到来自四面八方的间接光，即其他物体反射而来的光）

1. Compute light reflected toward camera at a specific **shading point**

Inputs: (Shading is Local 局部的)

-Viewer direction, V

-Surface normal, n

-Light direction, l (for each of many lights)

-Surface parameters (color, shininess)

(默认都是单位向量)

![Untitled](Lesson%2007%20Shading%201%20(Illumination,%20Shading%20and%20Gra%20ab9fe1ad83944eea90d58c5df63b1bab/Untitled%204.png)

**No shadows** will be generated! (shading ≠ shadow)

1. **Diffuse Reflection**

Light is scattered uniformly in all directions

-Surface color is the same for all viewing directions

![Untitled](Lesson%2007%20Shading%201%20(Illumination,%20Shading%20and%20Gra%20ab9fe1ad83944eea90d58c5df63b1bab/Untitled%205.png)

**-Lambert’s cosine law**

![Untitled](Lesson%2007%20Shading%201%20(Illumination,%20Shading%20and%20Gra%20ab9fe1ad83944eea90d58c5df63b1bab/Untitled%206.png)

每单位面积接收到光的能量和cosθ（入射光与表面法向量夹角）成正比

（可借助冬天、夏天太阳直射角度与冷暖的关系来理解）

**Lambertian (Diffuse) Shading**: **independent** of view direction

(I / r^2 表示光源向外一个球形范围内射出，到达shading point的能量)

![Untitled](Lesson%2007%20Shading%201%20(Illumination,%20Shading%20and%20Gra%20ab9fe1ad83944eea90d58c5df63b1bab/Untitled%207.png)

![Untitled](Lesson%2007%20Shading%201%20(Illumination,%20Shading%20and%20Gra%20ab9fe1ad83944eea90d58c5df63b1bab/Untitled%208.png)

Specular and Ambient part will be taught next course.