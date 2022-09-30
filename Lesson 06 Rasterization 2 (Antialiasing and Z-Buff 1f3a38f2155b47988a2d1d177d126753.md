# Lesson.06 Rasterization 2 (Antialiasing and Z-Buffering)

Created: September 24, 2022 10:56 PM

### Contents

---

**Antialiasing**

**Sampling theory**

1. [Sampling Artifacts](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753.md)
2. [Antialiasing Idea: Blurring Before Sampling](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753.md)
3. Reasons
    1. [Frequency Domain](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753.md)
    2. [Filtering = Getting rid of certain frequency contents](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753.md)
    3. [Filtering = Convolution(卷积) (= Averaging)](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753.md)
    4. [Sampling = Repeating Frequency Contents](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753.md)
    5. [Aliasing = Mixed Frequency Contents](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753.md)

**[Antialiasing in practice](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753.md)**

a solution

[MSAA](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753.md)

补充

---

### Review

Viewing

-View + Projection + Viewport

Rasterizing triangles

-Point-in-triangle test

-Aliasing

## This Course

> Antialiasing（反走样）
> 
> 
> -Sampling theory
> 
> -Antialiasing in practice
> 

> Visibility / occlusion
> 
> 
> -Z-buffering（深度缓冲）
> 

### Antialiasing

锯齿，学名：走样

### **Sampling theory**

1. Sampling Artifacts (瑕疵) in CG due to sampling
    
    Jaggies
    
    Moire Patterns （摩尔纹，跳过奇数行和列）
    
    ![Untitled](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753/Untitled.png)
    
    Wagon Wheel Illusion (False Motion 快速旋转的轮子在视觉上倒转)
    
    [Many more] …
    

本质: Signals are changing too fast (high frequency), but sampled too slowly

1. Antialiasing Idea: Blurring (Pre-Filtering 滤波) Before Sampling
    
    ![Untitled](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753/Untitled%201.png)
    

![Untitled](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753/Untitled%202.png)

![Untitled](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753/Untitled%203.png)

what if Sample then Filter? Blurred Aliasing (WRONG)

1. Reasons
    
    Why undersampling (采样过疏) introduces aliasing? Why pre-filtering then sampling can do antialiasing?
    
    **(1) Frequency Domain (频域)**
    
    ![Untitled](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753/Untitled%204.png)
    
    (傅里叶展开: Represent a function as a weighted sum of sines and cosines)
    
    Fourier Transform Decomposes A Signal Into Frequencies
    
    ![Untitled](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753/Untitled%205.png)
    

Higher Frequencies Need Faster Sampling

![Untitled](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753/Untitled%206.png)

![Untitled](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753/Untitled%207.png)

此时高、低频信号采样结果相同，此情况下无法判断二者是否不同，因此产生“走样”

**(2) Filtering = Getting rid of certain frequency contents**

Visualizing Image Frequency Content

![Untitled](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753/Untitled%208.png)

右图：Frequency Domain，中心向四周频率变高，中心亮则代表低频区信息更多

（水平和竖直两条明显的亮线：图像处理时默认将一张图片上下左右拼接本身图片，在过渡边图像发生剧烈信号变化，因此发生高频）

High-pass filter (高通滤波，只留下高频信号)

Filter out low frequencies only (Edges), 

高频多在物体边界变化剧烈处，因此图像中物体边缘被留下

![Untitled](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753/Untitled%209.png)

Low-pass filter (低通滤波，只留下低频信号)

Filter out high frequencies (Blur), 

同理，物体边缘被模糊

![Untitled](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753/Untitled%2010.png)

Filter out low and high frequencies

留下不是很明显的边缘特征

![Untitled](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753/Untitled%2011.png)

**(3) Filtering = Convolution(卷积) (= Averaging)**

Convolution (图形学中简化了的定义)

![Untitled](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753/Untitled%2012.png)

Convolution theorem: Convolution in the spatial domain is equal to multiplication in the frequency domain, and vice versa (在时域上做卷积相当于在频域上做乘积，反之亦然)

![Untitled](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753/Untitled%2013.png)

For example: 

![Untitled](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753/Untitled%2014.png)

Box Filter (归一化操作：防止图片过亮)

![Untitled](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753/Untitled%2015.png)

Wider filter kernel = Lower frequencies

(设想：用超级大的box，所有像素都变得差不多(模糊)，频率变低；用超级小的box，相当于完全没有做滤波，那么频率变高)

**(4) Sampling = Repeating Frequency Contents**

![Untitled](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753/Untitled%2016.png)

如对(a)采样，即只取其中若干个点；相当于(a)乘(c)冲激函数

既然是时域（左边一列）上的乘积，对应到频域（右边一列）上就是卷积

而在频域上的操作实际上是把频率复制粘贴了

**(5) Aliasing = Mixed Frequency Contents**

密集、稀疏采样，稀疏即采样速度不够快。
上图中信号正常采样，下图中因采样慢，采样点之间距离很大，信号已经到来（即频谱之间发生了混合），则发生了aliasing

![Untitled](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753/Untitled%2017.png)

### Antialiasing in practice

1. How can we reduce aliasing error?
    1. Increase sampling rate
    2. Antialiasing   i.e. Filtering out high frequencies before sampling

 Antialiasing = Limiting, then repeating

![Untitled](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753/Untitled%2018.png)

Antialiasing by Averaging values in Pixel area

solution: Convolve, then sample

![Untitled](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753/Untitled%2019.png)

1. Antialiasing by Supersampling (MSAA Multi-Sample Antialiasing，用更多的采样点)
    
    Approximate the effect of the 1-pixel box filter by sampling multiple locations within a pixel and averaging their values: （每个像素内部多加一些采样点，然后对这些点平均）
    

![Untitled](Lesson%2006%20Rasterization%202%20(Antialiasing%20and%20Z-Buff%201f3a38f2155b47988a2d1d177d126753/Untitled%2020.png)

（NxN supersampling，即每个像素内部有N个采样点）

Cost: 增加了计算量

1. **补充：**Milestones: 
    
    FXAA (Fast Approximate AA)，先得到有锯齿的图，找到边界后再换成无锯齿的边框
    
    TAA (Temporal AA)，取前一帧的采样，相当于把MSAA分布在时间上
    
    Super resolution(超分辨率) / super sampling
    
    -From low resolution to high resolution
    
    -Essentially still ”not enough samples” problem
    
    -DLSS (Deep Learning Super Sampling)