
---
### 一、刚体运动

- 平面：2+1个自由度
- 空间：3+3个自由度

#### 1、刚体运动状态表示

![[Pasted image 20251115202205.png]]

##### 向量可以表达空间关系的两种方式——平移、旋转

- 利用 $\vec{p}$ 向量来表示  {B} 原点相对于 {W} 的位置

 ![[Pasted image 20251115202349.png]]
 $$
\vec{p}=\begin{bmatrix}
{p_{x}}\\
{p_{y}}\\
{p_{z}}\\
\end{bmatrix}
$$
- 利用向量来表示{B}相对于{A}的姿态（在A参考系看$\hat{X}、 \hat{Y} 、\hat{Z}$ ）写成列坐标的形式

![[Pasted image 20251115203815.png]]
   写成投影如图示
![[Pasted image 20251115204252.png]]
 >![[Pasted image 20251115204736.png]]
 

---


### 二、Rotation Matrix

### 1、 特性：
#### 特性1

  ![[Pasted image 20251115205153.png]]
旋转矩阵横着看，每一行是{A}投影到{B}的向量的转置；竖着看，每一列是{B}投影到{A}的向量，于是旋转矩阵转置即可实现坐标系的相对变换。

#### 特性2

![[Pasted image 20251116150359.png]]
旋转矩阵与自己转置相乘，会得到单位矩阵
$$QQ^{T}=Q^{T}Q=1$$
**因此旋转矩阵是正交矩阵，有**
1. $$ 
Q^{-1}=Q^{T}
$$
2. Columns: orthonormal basis
	- Length=1
	- Mutually perpendicular(彼此正交）

虽然矩阵有九个数字，但有六个条件（正交、长为1），故其实只有**3DOFs**
### 2、作用
![[Pasted image 20251116154817.png]]
#### 1）Rotation matrix 用于描述一个Frame相对于另一个Frame的状态
![[Pasted image 20251116154610.png]]

#### 2）Rotation matrix 用于坐标系的相互转换

  ![[Pasted image 20251116152118.png]]

*我觉得整个下来就是对坐标系进行各种线性变换，实现拉伸、旋转*
*注意是左乘*

#### 3） Rotation matrix 用于描述物体转动的状态
- 关于z轴
![[Pasted image 20251116152508.png]]简写成
$$R_{\hat{Z}_{A}}(\theta)=\begin{bmatrix}
c\theta & -s\theta & 0\\
s\theta & c\theta & 0\\
0 & 0 & 1\\
\end{bmatrix}
$$
- 同理关于x、y轴
![[Pasted image 20251116153354.png]]

#### 3、rotation matrix 的拆解

 关键是确定先后顺序和转动轴

 有两种拆解方式——Fixed Angles 和 Euler Angles

#### 1）、Fixed Angles

![[Pasted image 20251117145035.png]]

$$
^A_{B}R_{X,Y,Z}(\gamma,\beta,\alpha)=R_Z(\alpha)R_Y(\beta)R_X(\gamma)
$$
**注意：这代表系B参照A旋转，且，拆分之后，Z的旋转在前，X的旋转在后。这是因为，以向量的角度来看是依次向左乘    $v'=^A_{B}Rv=R_3R_2R_1v$**

依照之前的旋转方式，可得
![[Pasted image 20251117145947.png]]

当我们知道旋转矩阵，尝试反推
![[Pasted image 20251117150706.png]]
其中atan2含义如下
![[Pasted image 20251117151037.png]]

#### （2）、 Z-Y-X Euler Angles
![[Pasted image 20251117151522.png]]

$$
^A_{B}R_{Z',Y',X'}(\alpha,\beta,\gamma)=R_{Z'}(\alpha)R_{Y'}(\beta)R_{X'}(\gamma)
$$

与固定角不同的是先转Z，再转Y，再转X。且相乘的顺序的最先转的放在最前面。


![[Pasted image 20251117152248.png]]

可以看出Euler Angle 和 FIxed Angle 存在对应关系 

![[Pasted image 20251117152559.png]]

### 三、Euler Angles

#### Z-Y-Z Euler Angles 拆解

![[Pasted image 20251117162007.png]]
![[Pasted image 20251117162049.png]]


同一个角度有不同的拆解方法

![[Pasted image 20251117162237.png]]

#### ·小结
- 同一种角度对Euler/Fixed angles各有12种拆解（$3*2*2$）Euler 和Fixed 一一对应


---

### 四、其他表示旋转的方法

#### 1、Euler Angles 的局限性
##### 1）、 不易在任意方向的旋转轴插值

欧拉角是用 **三个固定轴的旋转组合（比如 X→Y→Z）** 来描述姿态的。  
但如果你想在两个任意姿态之间做平滑过渡（插值），用欧拉角会出现：

- 插值路径不自然、不连续
- 经过奇怪的中间姿势
- 不同的欧拉角表达可能表示同一个姿态，导致插值不可预测

因此 **欧拉角不适合做平滑旋转插值**（比如动画、机器人姿态补间）。  
##### 2）、 万向节死锁
- **两个旋转轴重合**（依据欧拉角描述的固定顺序转动会导致转轴重合问题m?）
- 导致系统失去一个自由度
- 某些方向的旋转无法表达或表现得异常敏感
结果：
- 你想绕某一轴旋转，但系统理解为绕另一个轴旋转
- 控制会突然变得非常奇怪或不稳定
这是欧拉角最致命的缺点，也是为什么很多 3D 系统不再使用它。

链接：[无伤理解欧拉角中的“万向死锁”现象_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Nr4y1j7kn/?spm_id_from=333.337.search-card.all.click&vd_source=2a3190a9948c251b41f7ff9991d12d51)
##### 3）、旋转的次序无法确定（注意欧拉角和旋转矩阵的差别，旋转矩阵不存在这个问题，因为旋转矩阵是固定的）
欧拉角必须指定一个顺序，比如：
- XYZ（先 X，再 Y，再 Z）
- ZYX
- YXZ  

**不同的顺序会导致完全不同的最终姿态**。

#### 2、 Angle-axis表达法
![[Pasted image 20251117162745.png]]


#### 3、 Quatermion表达法
![[Pasted image 20251117162804.png]]
 
[带你探秘四维的神秘数字——四元数_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1654y1k7B8/?spm_id_from=333.337.search-card.all.click&vd_source=2a3190a9948c251b41f7ff9991d12d51)

**四元数的定义：**

$$ q=a+bi+cj+dk$$
$$\vec{v}=(b,c,d)$$
$$q=[a,\vec{v}]=a+\vec{v}$$

它是一个四维的数。相当于三个维度垂直于实轴，再把向量沿着实轴平移

其加法和标量乘法满足分配律

四元数乘法定义如下：
$$q_1q_2=(a+\vec{v})(e+\vec{u})
        =(ae-\vec{v}·\vec{u})+(a\vec{u}+e\vec{v}+\vec{v}×\vec{u}) $$

乘法逆运算定义如下
$$若qq^{-1}+q^{-1}q=1$$
$$q=a+\vec{v}$$
$$q^*=a-\vec{v}$$
$$发现q^*q=qq^*=1$$
$$则有q^{-1}=\frac{q^*}{|q|^2}   和逆矩阵的除法很像$$

![[Pasted image 20251117212618.png]]![[Pasted image 20251117212746.png]]
可以看到，u是单位的，且是纯的（实部为0），$u^2=-1$,且垂直于实轴，则u和实轴张开的平面和复平面其实是等价的。

对u的欧拉公式 有 $e^{u\theta}=cos\theta+usin\theta$   也是单位的

那么如何利用四元数进行旋转呢？

对一个三维向量 $v$，是一个纯四元数，也可以写成   $\vec{v}=[0,\vec{v}]$ ，想把此向量按右手法则按单位向量 $\vec{u}$ 所在的方向转 $\theta$ 得到 $v^{'}$ 
则: 
$$v^{'}=e^{\frac{\theta}{2}u}ve^{-\frac{\theta}{2}u}$$
或者写成：
$$
\begin{cases}
\vec{v^{'}}=p\vec{v}p^{-1}=p\vec{v}p^{*}\\
p=\begin{bmatrix}cos(\frac{\theta}{2})&sin(\frac{\theta}{2})\vec{u}
\end{bmatrix}
\end{cases}

$$

其证明[【较难慎入】证明四元数三维旋转公式_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1jt4y1v7Fs?spm_id_from=333.788.videopod.sections&vd_source=2a3190a9948c251b41f7ff9991d12d51)
文字推导[彻底搞懂“旋转矩阵/欧拉角/四元数”，让你体会三维旋转之美_欧拉角判断动作-CSDN博客](https://blog.csdn.net/weixin_45590473/article/details/122884112)


### 旋转矩阵与四元数
 ![[Pasted image 20251118104445.png]]
### 四元数与欧拉角的转化

![[Pasted image 20251118104636.png]]

---

### Q&A:

##### 1、

![[Pasted image 20251118112229.png]]


##### 2、齐次矩阵（Homogeneous Transformation）
我们把旋转和平移合成：$$T=\begin{bmatrix}R&t\\​0&1\
\end{bmatrix}$$这是一个 **4×4矩阵，称为位姿矩阵（pose matrix）**

点用齐次坐标表示：$p=[x,y,z,1]^T$

一次变换：$p^{′}=Tp$
![[Pasted image 20251120121233.png]]


##### 3、多个坐标系变换可以使用链式乘法

$$T_0^n=T_0^1T_1^2T_2^3⋯T_{n−1}^n​
$$
这就是机器人运动学的基础。

##### 坐标系转换：如何把点从 A 坐标系转换到 B？

假设：

点在 **A坐标系** 下是：$p_A$  ， 已知 **A → B 的位姿矩阵**：$T_A^B$

那么点在 B坐标系下：**$p_B=T_A^Bp_A$**

从 B 转回来 A :    **$p_A=(T_A^B)^{-1}p_A$


#### 4、
![[Pasted image 20251118111552.png]]

---
上一篇：这是第一篇
下一篇：[[轨迹规划]]
