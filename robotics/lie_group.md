## 刚体运动的指数表示

**旋转运动**

根据**欧拉定理**，任意三维空间旋转运动可以表示为绕某一单位轴$w\in \mathcal{R}^3$转动角度$\theta$,则旋转矩阵可以表示为矩阵指数的形式：
$$R=e^{\hat{w}\theta} \in \mathcal{R}^{3\times 3}  \\
\hat{w}=\begin{bmatrix}0 &-w_3 & w_2 \\w_3 & 0 & -w_1 \\-w_2 & w_1 & 0 \end{bmatrix}$$
$$so(3)与SO(3)的指数映射\text{(Exponential map)}关系  \{\begin{array}{lr} 
        \hat{w}\in so(3)\in \mathcal{R}^{3\times 3} \\
        \quad \\ 
        \theta \in  \mathcal{R} 
\end{array}  \Rightarrow  e^{\hat{w}\theta}\in SO(3)$$
angles: $\theta=cos^{-1}(\frac{tr(R)-1}{2})$

rotation angle: $[n]_\times=\frac{R-R^T}{2sin(\theta)}$,   $n=[-[n]_\times(2,3),[n]_\times(1,3),-[n]_\times(1,2)]$

In **unit quaternions(四元数)**: q=$(cos(\theta/2),\omega sin(\theta/2))$

**刚体运动**

与旋转相似，根据**Chasels定理**，任意刚体运动均可以通过绕一轴的运动加上平行于该轴的移动实现:
$$T=e^{\hat{\xi}\theta} \in \mathcal{R}^{4\times 4}  \\
\hat{\xi}=\begin{bmatrix} \hat{w} & \upsilon \\ 0& 0 \end{bmatrix}  \\
e^{\hat{\xi}\theta}=\begin{bmatrix} e^{\hat{w}\theta}& (I-e^{\hat{w}\theta})(w\times \theta)+ww^T\upsilon\theta\\0 &1 \end{bmatrix}$$

$$se(3)与SE(3)的指数映射(Exponential map)关系  \{\begin{array}{lr} 
        \hat{\xi}\in se(3)\in \mathcal{R}^{3\times 3} \\
        \quad \\ 
        \theta \in  \mathcal{R} 
\end{array}  \Rightarrow  e^{\hat{\xi}\theta}\in SE(3)$$

## 正运动学

- for revolute joint: $\theta_i \in S^1$,unit circle in the plane
- for prismatic joint: $\theta_i \in \mathcal{R}$
- $T^p$ to denote the p-torus, defined to be the **Cartesian product** of p copies of $S^1$: $$T^p=S^1\times S^1\times \cdots \times S^1$$
- joint space(configuration space) of a manipulator with p revolute joints and r prismatic joints: $Q=T^p \times R^
r$
- the forward kinematics map: $g_{st}$, t-tool frame and s-base frame:
 $$g_{st}(\theta)=g_{sl_1}(\theta_1)g_{l_1l_2}(\theta_2)\cdots g_{l_nt}$$

 

**指数积公式**

If $\xi$ is a twist, then the rigid motion associated with rotating and translating along the axis of the twist is given by:
$$g_{ab}(\theta)=e^{\hat{\xi}\theta}g_{ab}(0)$$
g: is a pose and also a transformation

The product of exponential formula(POE):
$$g_{st}(\theta)=e^{\hat{\xi}_1\theta_1}e^{\hat{\xi}_2\theta_2}\dots e^{\hat{\xi}_n\theta_n}g_{ab}(0)$$

Generalize this procedure:

- Define the **reference configuration**(初始位置) of the manipulator to be the configuration corresponding to $\theta=0$.  
- For each joint, construct a twist $\xi_i$ at $g_{st}(0)$
    - $\omega_i \in \mathcal{R}^3$,unit vector in the direction of th twist axis
    - $q_i \in \mathcal{R}^3$,any point on the axis(determine the direction)
    - $v_i \in \mathcal{R}^3$,unit vector pointing in the direction of translation  
    - all vectors are specified **relative to the base coordinate frame**
$$\begin{align}
\text{for revolute } \xi_i&=\begin{bmatrix}-\omega_i\times q_i \\ \omega_i  \end{bmatrix} or \begin{bmatrix}q_i\times \omega_i \\ \omega_i  \end{bmatrix} \\
\text{for prismatic } \xi_i&=\begin{bmatrix}v_i \\ 0  \end{bmatrix}
\end{align}$$
    - 指数积公式只能描述末端关节对基坐标系的变换，而不能得知每个关节相对于坐标系的变换。 

## only math

matrix exponential(矩阵指数):

$$e^A = \sum_{k=0} \frac{1}{k!}A^k$$

for a skew-symmetric matrix:

$$C=\left[\begin{array}{ccc}
0 & -a_{3} & a_{2} \\
a_{3} & 0 & -a_{1} \\
-a_{2} & a_{1} & 0
\end{array}\right]$$

Let $x=\sqrt{a_1^2 + a_2^2 + a_3^2 }$， then $C^3 = -x^2C$, Hence:

$$C^{2m+1}=(-1)^mx^{2m} \\
C^{2m}=(-1)^{m-1}x^{2m-2}C^2$$

Therefore:

$$\begin{aligned}
&e^{C}=\sum_{n=0}^{\infty} \frac{1}{n !} C^{n}=I+\sum_{m=0}^{\infty} \frac{1}{(2 m+1) !} C^{2 m+1}+\sum_{m=1}^{\infty} \frac{1}{(2 m) !} C^{2 m} \\
&=I+\sum_{m=0}^{\infty} \frac{(-1)^{m} x^{2 m}}{(2 m+1) !} C+\sum_{m=1}^{\infty} \frac{(-1)^{m-1} x^{2 m-2}}{(2 m) !} C^{2} \\
&=I+\frac{1}{x} \sum_{m=0}^{\infty} \frac{(-1)^{m} x^{2 m+1}}{(2 m+1) !} C-\frac{1}{x^{2}} \sum_{m=1}^{\infty} \frac{(-1)^{m} x^{2 m}}{(2 m) !} C^{2} \\
&=I+\frac{\sin x}{x} C+\frac{1-\cos x}{x^{2}} C^{2}
\end{aligned}$$



## ref

- paper
    - [A tutorial on SE(3) transformation parameterizations and on-manifold optimization](https://ingmec.ual.es/~jlblanco/papers/jlblanco2010geometry3D_techrep.pdf)
    - [2018_A micro Lie theory for state estimation in robotics]()
- project
    - [c++: manif](https://github.com/artivis/manif)
    - [c++: sophus](https://github.com/strasdat/Sophus)
    - [python: liegroups](https://github.com/utiasSTARS/liegroups)
