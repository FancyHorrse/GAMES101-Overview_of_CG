# Lesson.05 Rasterization 1 (Triangles)

Created: September 23, 2022 4:02 PM

### Contents

---

**[What’s after MVP?](Lesson%2005%20Rasterization%201%20(Triangles)%201f3085d05bc44b0a99f267ee7162cff3.md)**

**[Canonical Cube to Screen](Lesson%2005%20Rasterization%201%20(Triangles)%201f3085d05bc44b0a99f267ee7162cff3.md)**

[Rasterization: Drawing to Raster Displays](Lesson%2005%20Rasterization%201%20(Triangles)%201f3085d05bc44b0a99f267ee7162cff3.md)

---

### Review

![Untitled](Lesson%2005%20Rasterization%201%20(Triangles)%201f3085d05bc44b0a99f267ee7162cff3/Untitled.png)

补充：Aspect ratio (长宽比), FOV (水平，垂直可视角度)

![Untitled](Lesson%2005%20Rasterization%201%20(Triangles)%201f3085d05bc44b0a99f267ee7162cff3/Untitled%201.png)

What’s next?

-Rasterization (光栅化)

-Occlusions and Visibility

## This Course

### **What’s after MVP?**

**Screen**

-An array of pixels

-Size of the array: resolution

-A typical kind of raster display (光栅成像设备)

**Understand the Definitions:**

Raster == screen in German, Rasterize == drawing onto the screen

Pixel == a little square with uniform color (color is a mixture of (red, green, blue) )

**Screen space**

Pixel’s indices (x, y), both x and y are integers

Pixel’s indices are from (0, 0) to (width-1, height-1)

Pixel (x, y) is centered at (x+0.5, y+0.5), so it covers range (0, 0) to (width, height)

![Untitled](Lesson%2005%20Rasterization%201%20(Triangles)%201f3085d05bc44b0a99f267ee7162cff3/Untitled%202.png)

### **Canonical Cube to Screen**

-Irrelevant to z

-Transform in xy plane: [-1, 1]^2 to [0, width] x [0, height]

-Viewport transform matrix（视口变换）:

$M_{viewport}=\begin{bmatrix}
\frac{width}{2} & 0 & 0 & \frac{width}{2} \\
0 & \frac{height}{2} & 0 & \frac{height}{2} \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}$

**拓展**：Cathode Ray Tube (CRT 阴极射线管)

Frame Buffer: Memory for a Raster Display

![Untitled](Lesson%2005%20Rasterization%201%20(Triangles)%201f3085d05bc44b0a99f267ee7162cff3/Untitled%203.png)

![Untitled](Lesson%2005%20Rasterization%201%20(Triangles)%201f3085d05bc44b0a99f267ee7162cff3/Untitled%204.png)

LCD (Liquid Crystal Display 液晶显示) Pixel

LED (Light emitting diode 发光二极管) Array Display

Electrophoretic (Electronic Ink 电子墨水屏) Display

### Rasterization: Drawing to Raster Displays

**Triangles - Fundamental Shape Primitives**

-Most basic polygon (多边形)

-Break up other polygons

-Unique properties

-Guaranteed to be planar (平面的)

-Well-defined interior(内部)

-Well-defined method for interpolating values at vertices over triangle

**Rasterization As 2D Sampling**

Sample if each pixel center is inside triangle

![Untitled](Lesson%2005%20Rasterization%201%20(Triangles)%201f3085d05bc44b0a99f267ee7162cff3/Untitled%205.png)

By using Cross Products: P0P1xP0Q, P1P2xP1Q, P2P3xP2Q 

若三者同号（点Q在三条边的同侧），则在三角形内（边缘情况无所谓）

![Untitled](Lesson%2005%20Rasterization%201%20(Triangles)%201f3085d05bc44b0a99f267ee7162cff3/Untitled%206.png)

Checking all pixels on the screen?

![Untitled](Lesson%2005%20Rasterization%201%20(Triangles)%201f3085d05bc44b0a99f267ee7162cff3/Untitled%207.png)

即用P0, P1, P2中最小、最大的x、y值来确定一个正方形，之后再逐个像素检测

**拓展：Rasterization on Real Displays**

![Untitled](Lesson%2005%20Rasterization%201%20(Triangles)%201f3085d05bc44b0a99f267ee7162cff3/Untitled%208.png)

about Galaxy S5（人眼对绿色更敏感，因此绿色更多，所以此屏幕绿色点密度更高）

**Aliasing (Jaggies) 锯齿**

![Untitled](Lesson%2005%20Rasterization%201%20(Triangles)%201f3085d05bc44b0a99f267ee7162cff3/Untitled%209.png)