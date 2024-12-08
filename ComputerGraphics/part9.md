# Computer Graphics

# Part 9 - Obj and Quaternion

#### 2024.12.08.(일)

### GPU rendering

- Among the operations performed by the vertex shader, the most essential is applying a series of transforms to the vertex.

`object space` -> (world transform) -> `world space` -> (view transform) -> `camera space` -> (projection transform) -> `clip space`

## Object with Frames

### Extrinsic Transformation of the Eye

- We explicitly store the matrix E

  - Object coordinates: c
  - World coordinates: Oc
  - Eye coordinates: E^-1Oc
  - Calculating the eye coordinates of every vertexes:

  [xe] [xo]
  [ye] = E^-1O [yo]
  [ze] [zo]
  [1] [1]

### World vs. Object Frame

- The relationship between the world frame and object frame:

  - Affine 4-by-4 matrix O (rigid body transformation: rotation + translation only)

- The meaning of O is the relationship between the world frame to the object's coordinate system.

- To move the position of the object frame itself, we change the matrix O.

- An object can be treated as being assembled by some fixed and movable subobjects.

- We just update its O matrix to the object frame, instead of relating it to the world frame.

### Moving limbs

- We just update its O matrix to the object frame, instead of relating it to the world frame.

## Quaternion

### Euler's Formula

### Multiplication of Complex Numbers

e^(iø1)e^(iø2) = e^(i(ø1+ø2))

The multiplication of unit complex number is the same as combining the rotations.

### Euler Angle

- When we successively rotate an object about the principal axes, the object acquires an arbitrary orientation. This method of determining an object's orientation is called `Euler transform`, and the rotations angles, (ø1, ø2, ø3), are called the `Euler` angles.

- Concatenating three matrices produces a single matrix defining an arbitrary orientation.

- The rotation axes are not necessarily taken in the order of x, y, and z. Shown below is the order of y,x, and z. Observe that the teapot has a different orientation from the previous one.

### Keyframe Animation

- In the traditional hand-drawn cartoon animation, the senior key artist would draw the `keyframes`, and the junior artist would fill the in-between frames.

- For a 30-fps computer animation, for example, much fewer than 30 frames are defined per second. They are keyframes. In real-time computer animation, the in-between frames are automatically filled at run time.

- The key data are assigned to the keyframes, and they are interpolated to generate the in-between frames.

- In the example, the center position p and orientation angle ø are interpolated.

### Keyframe Animation in 3D

- Keyframe animation in 3D: Seven teapot instances are defined by sampling the graphs seven times.

- Smoother animation may often be obtained using a higher-order interpolation.

### Problem Euler Angle

- Euler angles are not always correctly interpolated and so are not suitable for keyframe animation

### Problem Euler Angle

Gimbal Lock and Apollo 11 & 13

### William Rowan Hamilton (1805 - 1865)

> Here as he walked by on the 16th of October 1843 Sir William Rowan Hamilton in a flash of genious discovered the fundamental formula for quaternion multiplication, i^2 = j^2 = k^2 = ijk = -1

> q = a + bI + cJ + dK

I, J, and K are imaginary numbers:

I^2 = J^2 = K^2 = -1

IJ = K, JK = I, and KI = J.

The multiplication is not commutative.

### Algebra of Quaternion

I^2 = J^2 = K^2 = -1

IJ = K, JK = I, and KI = J.

Q.JI = ?

### Quaternion

- A quaternion is an extended complex number.

### 2D Rotation through Complex Numbers

- Recall 2D rotation

- Let us represent (x,y) by a complex number x+yi, and denote it by p.

- Given the rotation angle ø, let us consider a unit-length complex number, cosø + sinøi. We denote it by q. Then, we have the following:

pq = (x + yi)(cosø + sinøi)
= (xcosø - ysinø) + (xsinø + ycosø)^i

- Surprisingly, the real and imaginary parts of pq represent the rotated coordinates.

### 3D Rotation through Quaternion

- As extended complex numbers, quaternions can be used to describe 3D rotation.

- Consider rotating a 3D vector p about an axis u by an angle ø. Represent both "the vector to be rotated" and "the rotation" in quaternions.

  - Define a quaternion p using p.
  - Define a unit quaternion q using u and ø. (The axis u is divided by its length to make a unit vector u.)
  - Compute qpq^\*. Then, its imaginary part represents the rotated vector.

- Quaternions enable rotations about arbitrary axes.

- Let p' denote qpq^\*. It represents the rotated vector p'. Consider rotating p' by another quaternion r. The combined rotation is represented in rq.

rp'r^_ = r(qpq^_)r^*
= (rq)p(q^*r^_)
= (rq)p(rq)^_

- "Rotation about u by ø'' is identical to "rotation about -u by -ø."

### Interpolation of Quaternion

- Consider two unit quaternions, p and q, which represent rotations. They can be interpolated using parameter t in the range of [0, 1]:

- This is called spherical linear interpolation (slerp).

### Quaternion and Matrix

- A quaternion q representing a rotation can be converted into a matrix form. If q = (qx, qy, qz, qw), the rotation matrix is defined as follows:

- See the textbook for proof.

- Conversely, given a rotation matrix, we can compute its quaternion. It requires us to extract {qx, qy, qz, qw} given the above matrix.
  - Compute the sum of all diagonal elements.
  - So, we obtain qw.
  - Subtract m12 from m21 of the above matrix.
  - As we know qw, we can compute qz. Similarly, we can compute qx and qy.

### Summary

- Summary

  - An arbitrary 3D rotation is represented in a quaternion as well as in Euler transform.
  - Quaternions are correctly interpolated through slerp.
  - A quaternion can be converted into a rotation matrix.

- Given quaternions for the keyframes,
  - spherically interpolate them for the in-between frames, and
  - convert each interpolated quaternion into a rotation matrix.

## [[그래픽스] 오일러 변환 & Quaternion](https://velog.io/@yeomjinseop/%EA%B7%B8%EB%9E%98%ED%94%BD%EC%8A%A4-%EC%98%A4%EC%9D%BC%EB%9F%AC-%EB%B3%80%ED%99%98)

### Euler transform

- 세 개의 주축 중심 회전을 결합한 것.

- 주축은 world space에서 택할 수도 있고, object space에서 택할 수도 있다.

- 물체의 방향은 회전이 결정한다.
  물체의 방향을 정의하는 데 있어 오일러 각(Euler angle)은 매우 직관적인 수단이다.
  오일러 변환은 각도 3개로 표현된다.
  세 축 중심의 회전각 øx, øy, øz을 오일러 각(Euler angles)라고 부른다.

### Keyframe Animation과 Euler transform

- animator는 rendering될 모든 frame에서의 동작을 정의하진 않는다.

- key frame 사이의 in-between frame은 key frame의 key data들 간의 interpolation으로 생성한다.
  key data의 예로는 물체의 위치, 방향, scaling factor 등을 들 수 있다.

### 2D Keyframe Animation

- key frame의 key data를 각각 {p0, p1}와 {p1, p2}라고 할 때, (단, pi는 각 사각형 중심의 좌표, øi: 회전각 = 방향)

```
p(t) = (1-t)po + tp1
ø(t) = (1-t)ø0 + tø1
```

다음과 같이 interpolation 된다.

### 3D Keyframe Animation

- 가장 오른쪽의 orientation이 x,y,z 축 중심의 회전 각도를 보여주는데, 이것이 오일러 각(euler angle)이다.

- 부드러운 animation은 고차원(high-order) interpolation을 통해 얻어진다.

### Euler Angle Interpolation

- Euler angle이 가진 문제는, 올바르게 interpolation 된다는 보장이 없다. 따라서 interpolation이 주로 사용되는 keyframe animation에는 적합하지 않다.

- 반면, 임의의 방향이나 회전을 표현하는 또 다른 기법인 `quaternion`은 항상 올바르게 보간된다.

### Quaternion

- quaternion q는 복소수를 확장한 것으로, 다음과 같이 네 개의 항으로 표현된다.

(qx, qy, qz, qw) = qxi + qyj + qzk + qw

- 허수부(imaginary part): qx, qy, qz
  실수부(real part): qw  
  허수 단위(imaginary unit): i, j, k

허수부는 종종 qv로 줄여쓴다. 그래서 quaternion은 (qv, qw)로 표현하기도 한다.

- 허수 단위는 다음과 같은 특징을 지닌다.

i^2 = j^2 = k^2 = -1

또한, 두 개의 서로 다른 허수단위가 곱해지면, 다음과 같은 순환치환(cyclic permutation)적인 특징을 가진다.

ij = k, ji = -kjk = i, kj = -iki = j, ik = -j

- 일반 복소수처럼 quaternion도 켤레를 가진다. q의 켤레 quaternion은 다음과 같다.

q^\* = (-qv, qw)
= (-qx, -qy, -qz, qw)
= -qxi - qyj - qzk + qw

- quaternion의 크기 (norm)

|| q || = √qx^2+qy^2+qz^2+qw^2

만약, 크기가 1이라면 q는 단위 quaternion(unit quaternion)이라 부른다.

### Quaternion을 이용한 회전

#### 2차원 회전

- 2차원에서 (x,y) 좌표를 가진 벡터를 반시계방향으로 ø만큼 회전하는 것은 다음과 같이 행렬-벡터 곱셈으로 표현되었다.

- 이제 x를 실수부로, y를 허수부로 가지는 복소수 x + yi를 p라 표기하자. 한편, 회전각 ø가 주어졌을 때, 이를 크기가 1인 극형식(polar form)의 복소수로 표현하면, q = cosø + sinøi 가 된다.

- 위의 복소수를 이용해 2차원 회전을 표현하면, 다음과 같다.

pq = (x + yi)(cosø + sinøi)
= (xcosø - ysinø) + (xsinø + ycosø) ^ i

#### 3차원 회전

- 3차원 벡터 p를 u 중심으로 ø만큼 회전시키는 경우, 회전할 벡터와 회전 변환 각각을 quaternion으로 표현하면, 다음과 같다.

p = (pv, pw) = (p, 0)

(허수부 p, 실수부 O)

또한, 회전축 u와 회전각 ø로 또 다른 quaternion q를 다음과 같이 정의한다.
이때 회전을 표현하는 quaternion의 크기는 1이 되어야 하므로, u를 단위벡터로 만든다.
그러면 크기가 1인 극형식(polar form)의 quaternion은 다음과 같다.

q = (qv, qw) = (sin(ø/2)u, cos(ø/2))

p를 u 중심으로 ø만큼 회전하는 것은 다음과 같이 표현된다.

p' = qpq^\* = (x', y', z', )
(x', y', z'은 허수부로, p가 회전된 벡터이다.)

- 회전된 벡터 p'을 또 다른 quaternion r을 사용해 회전시키는 경우를 생각해 보자.

rp'r^_ = r(qpq^_)r^*
= (rq)p(q^*r^_)
= (rq)p(rq)^_

이는 rq가 두 회전이 결합된 quaternion임을 보여준다.

- 'u를 중심으로 ø만큼 회전하는 것'은
  '-u를 중심으로 -ø만큼 회전하는 것'과 같다.

아래 그림을 보자.

- q를 'u를 중심으로 ø만큼 회전하는 것'이라고 하고,
  r을 '-u를 중심으로 -ø만큼 회전하는 것'이라고 할 때,

q와 r이 동일한 quaternion임을 알 수 있다.

- q = -q 이다. 즉, 동일한 회전을 표현한다.

- quaternion q는 'u 중심 ø만큼 회전', 즉 q = (sin(ø/2)u, cos(ø/2))이고,
  quaternion s는 'u 중심 2π + ø 만큼 회전' 이라고 할 때,

이는 q의 모든 원소의 부호를 바꾼 것이므로 s = -q로 쓸 . 수있다.
따라서 q = -q이므로, q와 -q는 동일한 회전을 표현한다.

### Quaterntion의 interpolation

- 두 quaternion q와 r을 생각해보자. 이들은 [0,1] 범위에서 정규화된 parameter t를 사용해 보간된다.
  다음은 구체 선형 보간 식이다. (spherical linear interpolation)

- 여기에서 q와 r 사이의 각도인 ø는 다음과 같이 계산된다.

### Quaterntion과 회전 행렬

- quaternion (qx, qy, qz, qw)가 주어졌을 때, 이를 회전 행렬로 전환하면 다음과 같다.
