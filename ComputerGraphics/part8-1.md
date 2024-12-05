# Computer Graphics

# Part 8-1 - Spaces

#### 2024.12.05.(목)

### Rendering

- Main stages in the pipeline
  - The vertex processing stage operates on every input vertex stored in the vertex buffer and performs various operations such as transform. (vertex shader)
  - The rasterization stage assembles polygons from the vertices and converts each polygon into a set of fragments. (A fragment refers to a set of data needed to update a pixel in the color buffer. The color buffer is a memory space storing the pixels to be displayed on the screen.)
  - The fragment processing stage operates on each fragment and determines its color.
  - In the output merging stage, the fragment competes or combines with the pixel in the color buffer to update the pixel color.

`vertex processing` -> `rasterization` -> `fragment processing` -> `output merging`

- Two programmable and two hard-wired (fixed) stages
  - The vertex and fragment processing stages are programmable. The programs for the stages are called vertex program and fragment program. Using the programs, for example, you can apply any transform you want to the vertex, and you can determine the fragment color through any way you want.
  - The rasterization and output merging stages are fixed or hardwired. They are not programmable but are just configurable through user-defined parameters.

### Spaces and Transforms for Vertex Processing

- Typical operations on a vertex

  - Transform
  - Lighting
  - Animation

- Transforms at the vertex processing stage
  - World transform
  - View transform
  - Projection transform

`object space` -> (world transform) -> `world space` -> (view transform) -> `camera spce` -> (projection transform) -> `clip space`

## Spaces and Transforms

### Scaling

- 2D scaling with the scaling factors, sx and sy

[sx 0] [x] = [sxx]
[0 sy] [y] = [syy]

- When a polygon is scaled, all of its vertices are processed by the same scaling matrix.

- Examples

### Rotation

- 2D rotation

- An example

- By default, the rotation is counter-clockwise.

- The matrix for the clockwise rotation by ø is obtained by inserting -ø into the 2D rotation matrix.

- Note that rotation by -ø is equivalent to rotation by 2πø,

### Translation and Homogeneous Coordinates

- Unlike scaling and rotation, translation is represented as vector addition.

- Fortunately, we can describe translation as matrix multiplication if we use the homogeneous coordinates.

  - Homogeneous coordinates for the point to be translated: (x, y) -> (x, y, 1)
  - Translation matrix [1 0 dx]
    [0 1 dy]
    [0 0 1]

- Given a 2D point, (x,y), its homogeneous coordinates are not necessarily (x,y,1) but (wx, wy, w) with non-zero w.

- For example, the Cartesian coordinates (2,3) can be converted into multiple homogeneous coordinates, (2,3,1), (4,6,2), (6,9,3), etc.

- The line connecting (2, 3, 1) and the origin represents infinitely many points in homogeneous coordinates.

- Suppose that we are given the homogeneous coordinates, (X, Y, w). By dividing every coordinate by w, we obtain (X/w, Y/w, 1).
  This corresponds to projecting a point on the line onto the plane, w = 1. We then take the first two components, (X/w, Y/w), as the Catesian coordinates.

- For handling the homogeneous coordinates, the 3x3 matrices for scaling and rotation need to be altered.

### Transform Composition

- An object may go through multiple transforms.

- Matrix multiplication is not commutative.

- Rotation (R) followed by translation (T) vs. T followed by R

- Compare the result with the previous page's.

- So far, rotations are about the origin. Let us now consider rotation about an arbitrary point, which is not the origin.

- Example: rotation of (5,2) about (3,2)

- Rotating a point at (x,y) about an arbitrary point, (a,b)
  - Translating (x,y) by (-a, -b)
  - Rotating the translated point about the origin
  - Back-translating the rotated point by (a,b)

### Affine Transform

- Affine transform

  - Linear transform
    - Scaling (S),
    - Rotation (R),
    - and many others.
  - Translation (T)

- No matter how many affine matrices are given, they can be combined into a matrix.

- When a series of affine transforms is concatenated to make a single 3x3 matrix, the third row is always (0 0 1).

- Ignoring the third row of an affine matrix, we often denote the remaining 2x3 elements by [L|t], where L is a 2x2 matrix and t is a 2D column vector.

  - L represents a 'combined' linear transform, which does not include any terms from the input translations.
  - In contrast, t represents a 'combined' translation, which may contain the input linear-transform terms.

- Revisit T(7,0)R(90˚)

- Conceptual decomposition of [L|t]

  - L is applied first.
  - The linear-transformed object is translated by t.

- Regardless of how affine matrices are combined, we obtain a single matrix [L|t].
  The way [L|t] transforms a point, p, is Lp + t.

### Rigid Motion

- Consider a combination of rotations and translations only, e.g., no scaling is involved.

  - When the combined affine matrix applies to an object, the pose (position + orientation) of the object is changed but its shape is not.
  - In this sense, the transform is named rigig-body motion or simply rigid motion.

- No matter how many rotations and translations are combined, the resulting matrix is of the structure, [R|t].

  - R represents the 'combined' rotation, which does not include any translation terms.
  - t represents the 'combined' translation, which usually includes the rotation terms.

- Transforming an object by [R|t] is conceptually decomposed into two steps: R is applied first and then the rotated object is translated by t, i.e., the way [R|t] transforms a point, p, is Rp + t.
