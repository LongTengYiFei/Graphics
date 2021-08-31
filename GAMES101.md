### 图形学和计算机视觉

图形学：model->graphic

计算机视觉： graphic->model



### 光栅化

把几何primitives投向屏幕。

打破刚才的投影，打成碎片（像素）。

光栅=屏幕（德语）

光栅化：把东西画在屏幕上

像素是一个点，像素有大小，像素有一个中心点，判断像素的中心在不在三角形的内部。

信号变化太快，采样速度跟不上，导致瑕疵。

对于高频率的函数，需要更快的采样，如果对周期小高频率的函数使用频率低的采样，是很难恢复出原来的函数的。

傅里叶变换可以把任意一个周期函数拆成正弦函数的线性组合加上一个常数项。

先对原图进行模糊操作，再采样，可以抗锯齿。如果先对原图采样，再对采样的结果模糊，这样是不行的，效果不行。



### 走样：aliasing

对两个频率截然不同的函数使用相同的采样办法，就是采样间隔相同， 如果得到的函数值相同，我们无法区分这两个函数，这就是走样（第六课）。



### 傅里叶变换

visualizing image frequency content

一个图片，像素变化剧烈的部分就是高频信息，信号频率大。（图片中的一些明显的边界就是高频段）

高通滤波，只留下图片的高频信息，然后图片看起来还是有轮廓的，因为主要的信息留下来了。

如果对图片用低通滤波器，只让低频信息通过，图片中的边界就变模糊。

滤波=卷积=平均

盒型滤波=低频滤波



### 光栅化：抗锯齿 antialiasing

1.增加采样率，换个分辨率大的屏幕。

2.先模糊，再采样（把高频信息过滤掉）。  

![image-20210822153702898](C:\Users\chenyifei01\AppData\Roaming\Typora\typora-user-images\image-20210822153702898.png)



### MSAA-Multisample anti-aliasing

1.把像素内部分成均匀的n*n个点

2.average n*n samples "inside" each pixel

3.结果

![image-20210822154703329](C:\Users\chenyifei01\AppData\Roaming\Typora\typora-user-images\image-20210822154703329.png)

代价：

用更多的点测试在不在三角形内，增加了计算量。

 另外两种方案：

FXAA-Fast Approximte(先生成锯齿图，然后找边界，把所有边界替换成不锯齿的边界)

TAA-Temporal



### 光栅化：可见性/遮挡 visibility/occlusion 

画家算法：先画远处的东西，再画近处的东西，把远处的东西遮挡住。单有例外，如果遇到互相遮挡的三角形，就失效了。

 ![image-20210822210814796](C:\Users\chenyifei01\AppData\Roaming\Typora\typora-user-images\image-20210822210814796.png)

深度缓冲 Z buffer

当我们生成一帧时，记录每个像素点的深度。

为什么叫z呢？因为我们的相机位于原点，朝-z方向看去。

深度缓存这个算法和我们先画哪个后画哪个三角形是没关系的，不管你怎么画，结果都是一样的。

z buffer处理不了透明物体



### 叉乘

可以判断两个向量的左右关系。

可以判断一个点是不是在三角形内部。

**光栅化**时我们必须知道一个像素点是不是在某个三角形的内部，这样我们好接下来给像素点着色。



### 齐次坐标 -仿射变换

可以把平移变换和线性变换合在一起，一个矩阵就可以既包含平移又包含线性变换。这种变换叫仿射变换。

先线性变换再平移。



### 旋转矩阵

旋转矩阵的逆矩阵正好就是旋转矩阵的转置矩阵

数学上：这种矩阵叫正交矩阵



### 视图/相机变换

把所有物体和相机一起变换，相对位置不变，拍出来的照片一样。

直到相机在原点，up在Y轴，看向-Z方向。



### 投影变换 -正交投影orthographic

没有近大远小的感觉，相当于把摄像机拉得无限远，四棱锥的那个尖角在无限远处，那么近处平面和远处平面相对于摄像机来说都是很远的。



### 投影变换-透视投影perspective

近大远小，一叶障目。

把摄像机当成点，发出一个四棱锥，近处的平面和远处的平面之中的物体会被看到。

先把远处的平面挤压，挤压成近处平面的大小，然后再正交投影。 

如何定义一个我们想看到的屏幕？1宽高比，2视角 。

摄像机的位置（一个点）和屏幕的四个角连起来，就是视野范围。



### 三角形

最基础的多边形

任何多边形都可以被拆成三角形

三角形的三个点一定在同一个平面

三角形的内外定义非常清晰

 

## 着色

### Shading着色

镜面高光specular highlights

漫反射diffuse reflection

环境光ambient lighting



### shading point

输入：三个单位向量

view direction

surface normal

light direction

输入：表面参数（color shiniess）



### 漫反射：lambert余弦定理-Diffuse Trem

点光源离我们的着色点的距离已知，光照强度和距离成平方反比。

距离知道，夹角知道。

如果光线和着色点法线平行，那么弹射的光的强度最大。

某个点弹射光，实际就是吸收一部分光，弹射一部分光，也就是特定波长的光吸收，有的不吸收。每个点我们都定义一个三维向量就是RGB，每个分量上弹射多少光。

漫反射系数？KD

不管从哪个方向观测着色点，看到的值都是一样的，因为是漫反射。



### Specular Term

镜子是绝对光滑的，一个入射光，一个反射光。

金属是比较光滑的，一个入射光，反射光沿着应该反射的方向有个分布，就是入射一根线，反射一根比较粗的线。

使用入射方向和观察方向计算半程向量，如果半程向量和法线接近，说明观察方向和反射方向接近。



如果单纯用余弦来计算两个向量的夹角，会发现，cos45是大约0.7左右，其实离得已经很远了，我们不希望这样，我们希望只要稍微有个夹角，就看不到高光。我们cos<sup>p</sup>a，对cos做一个p次方，p是大于1的数，p越高，我们就越不能容忍夹角。Blinn-Phong模型下，p大约是100-200.

![image-20210824200934632](C:\Users\chenyifei01\AppData\Roaming\Typora\typora-user-images\image-20210824200934632.png)

这里是高光项和漫反射项合在一起的情况，但是只考虑高光内的参数。ks是高光系数，发现越大，我们看到的高光越亮。p是cos指数，p越大，我们越不能容忍夹角，就是说只要有一个角度，我们就看不到高光。

 

### Ambient Term

环境光不讲究从哪射入光，把所有地方提升一个亮度，保证没有一个地方是完全黑的。这是一个大胆的简化。



### 布林冯反射模型：把所有的项加起来

![image-20210824201657979](C:\Users\chenyifei01\AppData\Roaming\Typora\typora-user-images\image-20210824201657979.png)

记得好像哪本书有这个图。



### Shading Frequency

flat shading，计算三角形的法线，然后三角形每个点着色相同。

gouraud shading，计算每个顶点的法线，然后对每个三角形的顶点着色，内部的点通过插值计算颜色。

phong shading，计算每个顶点的法线，然后三角形内所有点的法线通过插值计算出来，然后对每个点着色。（不是布林冯模型）

![image-20210824203518428](C:\Users\chenyifei01\AppData\Roaming\Typora\typora-user-images\image-20210824203518428.png)

每一行都是一样的模型。

对简单的模型（第一行）使用复杂的phong着色方法能达到很光滑的感觉。

当模型足够复杂，也就是面数足够多时，发现，即使是简单的着色方法flat也可以做的很好。

- flat着色：恒定的表面着色

- Gouraud着色：颜色插值着色

- Phong着色：顶点法线插值着色

  

- flat shading：三角形的顶点没有法向量，三角形整个面才有法向量，打光时整个三角形只呈现一种颜色。

- Gouraud shading：三角形的顶点都有各自的法向量，打光时三个顶点有各自的颜色，接着做双线性内插（bilinear interpolation）来求得颜色，使整个三角形有渐层的颜色变化。

- Phong shading：三角形的顶点都有各自的法向量，先对三角形整个面作法向量的双线性内插，接着打光来求整个三角形的颜色。

  https://dawnarc.com/2018/06/shadingflatgouraudphong-shading%E7%9A%84%E5%B7%AE%E5%88%A5/

  

### 顶点法线

这个顶点相邻有一些三角形，把所有三角形的法线求个平均，当做这个点的法线。

然而，相邻的三角形大小各不相同，求平均时，做加权平均，权就是三角形的面积。



### shader

一个通用的程序，每个顶点或者像素都会执行，而不用指定哪个像素执行。（当然，是特定模型上的）

![image-20210824212319217](C:\Users\chenyifei01\AppData\Roaming\Typora\typora-user-images\image-20210824212319217.png)



### 纹理映射texture mapping

有了tm，我们可以定义一个点的基本属性，包括漫反射系数，漫反射系数应该是4维向量，代表颜色。

任何一个三维物体的表面都是二维的。

![image-20210824214917302](C:\Users\chenyifei01\AppData\Roaming\Typora\typora-user-images\image-20210824214917302.png)

我怎么知道一个三角形在texture的哪里呢？我们假设已经知道了，我们不研究这个。

纹理也有坐标，通常用uv表示。

不管纹理是不是方块，uv都被限定在0-1

![image-20210824215715243](C:\Users\chenyifei01\AppData\Roaming\Typora\typora-user-images\image-20210824215715243.png)

通过纹理可视化我们发现，纹理被重复贴了，纹理间有缝隙。

![image-20210824215748177](C:\Users\chenyifei01\AppData\Roaming\Typora\typora-user-images\image-20210824215748177.png)

但是实际效果很好，因为这是纹理设计的好，不管怎么重复帖都没有明显的缝隙。



### 重心坐标

三角形中的一个点可以由三个顶点的坐标的线性组合表示出来，线性组合系数是（a,b,c）。

那么三角形中的某个点的属性，也可以由三个顶点的属性线性组合而来，组合系数就是上面那个，重心坐标。



### 纹理

1.纹理太小，屏幕太大，屏幕上的点映射到纹理上就是非整数的。（为什么？）

然后四舍五入，屏幕上的点还是会映射到纹理上的整数坐标。

纹理太小的话，屏幕中多个像素会被映射到纹理上同一个texel上，比如说8*8的屏幕像素。

效果就是![image-20210825212708632](C:\Users\chenyifei01\AppData\Roaming\Typora\typora-user-images\image-20210825212708632.png)

解决方案：双线性插值Bilinear interpolation



2.纹理太大

离摄像头近的像素点，覆盖的纹理相对较小，离摄像头远的像素点，覆盖的纹理区域很大。

远方的信号变化太快，我们的采样速度跟不上，造成走样。因为我们一个像素点可以覆盖远方的很多纹理点。

如果用一个像素中心落在远方纹理上的某个纹理点的值代表整个像素的值（用一个纹理点的值代表一片纹理的值），肯定是不行的，漏了很多信息。

解决方案：mipmap

三线性插值





![image-20210825224704399](C:\Users\chenyifei01\AppData\Roaming\Typora\typora-user-images\image-20210825224704399.png)

但是，屏幕上的一个点，可不一定是对应贴图上一个被等比例压缩的点。就比如说左上角那个点，对应的居然是斜着被压缩的一片区域。

各向异性过滤

ewa过滤



### 环境光

![image-20210828152657989](C:\Users\chenyifei01\AppData\Roaming\Typora\typora-user-images\image-20210828152657989.png)

我在屋子里放一个金属球或者镜子球，球面发射的就是环境， 这是一个存储环境光的说法。



### Textures can affect shading

我们之前用纹理是为了替换布林冯模型里的kd，漫反射项系数，就是颜色。 

**Bump mapping凹凸贴图** /**Normal mapping法线贴图**

Example：橘子

橘子是球，但是是凹凸不平的，本来如果是圆球我用一两百个面渲染就行了，然是如果要表示凹凸不平的我得用更多的面。

如果用凹凸贴图，定义每个点的相对高度，这个点的高度变化，法线就会变化（为什么），然后着色结果就会变化。我们看着就感觉球是凹凸不平的。 3

如果我们用了凹凸贴图或者法线贴图，就是我们人为做了一个假的法线出来，**假的着色结果，欺骗人的眼睛**，实际上我们并没有改变几何模型。

**displacement mapping位移贴图** 

凹凸贴图是改变顶点的位置，然后计算假的法线，能看左边的球，的边缘，露馅了，我们看起来是光滑的，而且自身的凸起没有给自身投射阴影。

而右边的位移贴图，真的会改变顶点的位置。



 ![image-20210828160827251](C:\Users\chenyifei01\AppData\Roaming\Typora\typora-user-images\image-20210828160827251.png)



纹理还可以存储之前已经计算好的东西，之后看你在着色器里如何去解释它。

![image-20210828161407586](C:\Users\chenyifei01\AppData\Roaming\Typora\typora-user-images\image-20210828161407586.png)





## 几何Geometry

隐式表示

距离函数



### 显示表示

要么直接表示

要么通过参数表示



### 显示表示 ：点云point clouds

最简单的方法，（x，y，z）list，可以表示任何形状的几何。



### 显示表示 ：三角面Traingle Mesh/多边形面Polygon Mesh

最广泛应用的方法

三角形之间的关系较为复杂



### 显示表示 ：Bezier surfaces/curves

piecewise bezier curves

没必要用很多控制点定义一个贝塞尔曲线，每次我用很少的控制点定义一段贝塞尔曲线，然后把这些线连起来。

大家都很愿意用4个点来定义一段贝塞尔曲线。（3次贝塞尔曲线，4个点）



### 网格操作

mesh subdivision(upsampling)细分，过采样。

mesh simplification

mesh regularization



### 面细分

Loop Subdivision（Loop是姓氏）

Catmull Clark Subdivision(General Mesh)



### 面简化

边坍缩edge collapse，找到一条边，这个边有两个顶点，把这两个点捏在一起。

哪些边重要，我不能捏？哪些边不重要，我可以捏。



### Shadow

shadow mapping：一个点不在阴影里面，就代表，光能看到这个点，摄像机也能看到这个点。

只能处理点光源投射下来的阴影。



硬阴影：一个点要么在阴影里，要么不在。阴影边界明显。

软阴影:

如果有软阴影，说明点光源有一定的大小，点很大。



## 光线追踪 Ray Tracing

### 为什么用光线追踪？

光栅化很难处理全局的效果

soft shadow

glossy reflection

indirect illumination



光栅化：快，实时

光线追踪：慢，准确



### 关于光线的假设（虽然不对，但是先用）

1.光沿直线传播。

2.两束光线就算发生碰撞，也没有关系。

3.光有办法通过不断的反射打到人的眼睛，人的眼睛也可以发出射线不断反射打到光源。



### 递归光线追踪

![image-20210830214647559](C:\Users\chenyifei01\AppData\Roaming\Typora\typora-user-images\image-20210830214647559.png)

### 光线怎么和三角形求交点？

光线和模型的所有三角形逐一判断

问题转换：三角形肯定在一个平面内，先用光线和平面求交，然后再判断点在不在三角形内。（点在不在三角形内，这个问题已经讲过了，用叉乘）

MT算法



### 加速判断三角形和光线是否相交

1.bounding volume



### AABB

算法的全称是 - **axis aligned bounding box (轴对齐-边界盒)** 



### 加速方法-Uniform grids

画格子，预先划分场景。

如果场景中的物体分布很均匀，基本上每个格子里都有物体，那么这个方法就很好。

如果场景里的物体分布不均匀，这个方法效率就低。



### 加速方法-Spatial paritions

oct tree

kd tree

bsp tree



### 加速方法-物体划分

![image-20210831204412934](C:\Users\chenyifei01\AppData\Roaming\Typora\typora-user-images\image-20210831204412934.png)



![image-20210831205431932](C:\Users\chenyifei01\AppData\Roaming\Typora\typora-user-images\image-20210831205431932.png)





### 基础辐射度量学



### <问题>

绕任意轴旋转怎么回事？

如何通过顶点法线插值得到三角形内部所有点的法线呢?

我知道三角形三个点的uv，那么怎么知道三角形内部点的uv呢？
