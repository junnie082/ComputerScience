# Computer Graphics

# Part 8-2 - 3D Transforms

#### 2024.12.05.(목)

### 3D Scaling

- 3D scaling

- If all of the scaling factors are identical, the scaling is called uniform.
  Otherwise, it is a non-uniform scaling.

- Unlike 2D rotation which requires a center of rotation, 3D rotation requires an axis of rotation.

- Let's consider 3D rotations about x-axis (Rx), y-axis (Ry), and z-axis (Rz).

- First of all, Rz'.

- Observation for Rx.

  - Obviously, x'=x.
  - When the thumb of the right hand is aligned with the rotation axis, the other fingers curl from the y-axis to the z-axis.
  - Returning to Rz, observe the fingers curl from the x-axis to the y-axis.
  - Shifting from Rz to Rx, the x-axis is replaced with the y-axis, and the y-axis is replaced with the z-axis.
  - By making such replacements, the matrix for Rx is obtained.

- In the same manner, we can define the matrix for Ry.

### 3D Rotation

- CCW vs. CW rotations

  - If the rotatio is CCW with respect to the axis pointing toward you, the rotation angle is positive.
  - If the rotation is CW, its matrix is defined with the negated rotation angle.

- Note that rotation by ø is equivalent to rotation by 2πø.

### 3D Translation

- Matrix-point multiplication

- The 3x3 matrices developed for 3D scaling and rotation are extended to 4x4 matrices. See the scaling example.

### World Transform

- The coordinate system used for creating an object is named object space.

- The object space for a model typically has no relationship to that of another model. The world transform 'assembles' all models into a single coordinate system called world space.

### 3D Affine Transform

- The discussions we had on 2D affine transforms apply to 3D affine transforms.

- First of all, matrix multiplication is not commutative.

- When 3D scaling, rotation, and translation matrices are concatenated to make a 4x4 matrix, the fourth row is always (0 0 0 1).

- Ignoring the fourth row of an affine matrix, we often denote the remaining 3x4 elements by [L|t], where L is a 3x3 matrix that represents a 'combined' linear transform, and t is a 3D column vector that represents a 'combined' translation.

- [L|t] is conceptually decomposed into two steps: L is applied first and then the linear-transformed object is translated by t.

### 3D Rotation

- Once an object is created, it is fixed within its object space. An object can be thought of as being stuck to its object space.

- Initially the object space can be considered identical to the world space.

  - The object-space basis is {u, v, n}.
  - The world-space basis (standard basis) is {e1, e2, e3}.

- A rotation applied to an object changes its orientation, which can be described by the 'rotated' basis of the object space.

- R relates e1 and u, which was initially identical to e1.

- Similarly, R transforms e2 and e3 into v and n, respectively.

- The above three are combined.

- R's columns are u,v, and n. Given the 'rotated' object-space basis, {u, v, n}, the rotation matrix is immediately determined, and vice versa.

- The observation we have made holds in general.

### Inverses of Translation and Scaling

- Inverse translation

- Invers transform in inverse matrix

- Inverse scaling

### Inverses of Rotation

- Given a rotation matrix R, its columns (u, v, and n) make up an orthonormal basis, i.e., u*u = v*v = n*n = 1 and u*v = v*n = n*u = 0.

- Let's multiply R's transpose (R^T) with R:

- This says that R^-1 = R^T, i.e., the inverse of a rotation matrix is its transpose.

- Because u,v, and n form the columns of R, they form the rows of R^-1.

- Recall that u, v, and n form the columns of R. As R^-1 = R^T, u, v, and n form the rows of R^-1.
