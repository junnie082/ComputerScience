# Computer Graphics

# Part 8-3 - Vertex Processing

#### 2024.12.08.(일)

## Vertex Processing

### GPU rendering

- In the GPU, rendering is done in `pipeline` architecture.

`vertex shader` -> `rasterizer` -> `fragment shader` -> `output merger`

- A `shader` is a synonym of a program. We have to provide the pipeline with two programs, i.e., the vertex and fragment shaders. In contrast, the rasterizer and output merger are `hard-wired` stages that perform fixed functions.
  - The vertex shader performs various operations on every input vertex stored in the vertex array.
  - The rasterizer assembles triangles from the vertices and converts each triangle into `fragments`. A fragment is defined for a pixel location of the triangle on the screen and refers to a set of data used to determine a color.
  - The fragment shader computes the fragment color.
  - The output merger uses the fragment color to determine the pixel color.

### GPU rendering

- Among the operations performed by the vertex shader, the most essential is applying a series of transforms to the vertex.

`object space` -> (world transform) -> `world space` -> (view transform) -> `camera space` -> (projection transform) -> `clip space`

- We've learned the world transform.

### Transform - normal

- Previously, the world transform was applied to vertex positions in the vertex array. How about vertex normals?

- If the world transform is in [L|t], the normals are affected only by L, not by t.

- If L includes a non-uniform scaling, it cannot be applied to surface normals. See the triangle normal example shown below.

- The triangle's normal scaled by L is not any longer orthogonal to the triangle scaled by L.

- Instead, we have to use inverse transpose of L, i.e., (L^-1)^T.

- It is simply denoted by L^-T.

- If L does not contain any non-uniform scaling, n can be transformed by L. However, Ln and L^-Tn have the same direction even though their magnitudes may be different. Therefore, we can transform n consistently by L^-T regardless of whether L contains a non-uniform scaling or not.

- Vertex normals are transformed by L^-T.

- Note that the vertex normals transformed by L^-T will be finally normalized.

### View Transform

- Camera pose (position + orientation) specification in the world space

  - EYE: camera position
  - AT: a reference point toward which the camera is aimed
  - UP: view up vector that describes where the top of the camera is pointing. (In most cases, UP is set to the vertical axis, y-axis, of the world space.)

- The camera space, {u, v, n, EYE}, can be created. Note that {u, v, n} is an orthonormal basis.

- A point is given different coordinates in distinct spaces.

- If all the world-space objects can be newly defined in terms of the camera space in the manner of the teapot's mouth end, it becomes much easier to develop the rendering algorithms.

- In general, it is called `space change`. The space change from the world space {e1, e2, e3, O} to the camera space {u, v, n, EYE} is the `view transform`.

- The space change can be intuitively described by the process of superimposing the camera space, {u, v, n, EYE}, onto the world space, {e1, e2, e3, O}.

- First, EYE is translated to the origin of the world space. Imagine invisible rods connecting the scene objects and the camera space. The translation is applied to the scene objects.

- Due to translation, the world space and the camera space now share the origin.

- We then need a rotation, R, which transforms {u, v, n} into {e1, e2, e3}.

- As the world and camera spaces are made identical, the world-space positions of the transformed objects can be taken as the camera-space ones.

- The rotation is called `basis change`, i.e., space change = translation + basis change.

- Mview is applied to all objects in the world space to transform them into the camera space. Mview is of the form, [R|t], where R happens to be identical to R and t is different from the last column of T.

- The space/basis change plays quite an important role in many algorithms of computer graphics, computer vision, augmented reality, and robotics. It also frequently appears later in this class.

- Right-hand system vs. left-hand system

- An inconsistency is observed if an object defined in the RHS is ported to the LHS as is together with the same camera parameters.

- In order to avoid the reflected image shown in the previous page, the z-coordinates of both the objects and view parameters should be negated.

- Note that z-negation is conceptually equivalent to the z-axis inversion.

### View Frustum

- Let us denote the camera-space axes by {x, y, z} instead of using {u, v, n}.

- EYE, AT and UP defined external parameters of the camera. Now let's define its internal. It is analogous to choosing a lens and controlling zoom-in/zoom-out.

- An infinite pyramid is defined by fovy and aspect. Its apex is at the origin and axis is the -z axis.

- Then, n and f, which stand from the distances to the near and far planees respectively, define a truncated pyramid. It's called `view frustum`.

- The near and far planes run counter to the real-world camera or human vision system, but have been introduced for the sake of computational efficiency.

### View Frustum culling

- The cylinder and sphere would be discarded by the so-called view-frustum culling whereas the teapot would survive. View-frustum culling that is implemented on CPU has been studied for a long time, and there exist good algorithms.

- Only the teapot will proceed to the GPU rendering pipeline.

- If a polygon intersects the boundary of the view frustum, it is `clipped` with respect to the boundary, and only the portion inside the view frustum is processed for display.

### Projection Transform

- The pyramidal view frustum is not used for clipping. Instead, a transform is used that deforms the view frustum into the axis-aligned 2x2x2-sized cube centered at the origin. It is called `projection transform`. The camera-space objects are projection-transformed and then clipped against the cube.

- The projection-transformed objects are said to be the `clip space`.

- The view frustum can be taken as a convergent pencil of projection lines. The lines converge on the origin, where the camera (EYE) is located.

- All 3D points on a projection line are mapped onto a single 2D point in the projected image. It brings the effect of `perspective projection`, where objects farther away look smaller.

- The projection transform ensures that the projection lines become parallel to the z-axis. Now viewing is done along the universal projection line. The projection transform brings the effect of perspective projection "within a 3D space."

- Projection transform matrix

- Derivation of the matrix is presented in the textbook.

- Projection transform matrix

- The projection-transformed objects will enter the rasterizer.

- Unlike the vertex shader, the rasterizer is implemented in hardware, and it assumes that the clip space is left-handed. In order for the vertex shader to be compatible with the hard-wired rasterizer, the objects should be z-negated.

- Projection transform from the camera space (RHS) to the clip space (LHS)

## [[그래픽스] 정점 처리(Vertex Processing)](https://velog.io/@yeomjinseop/%EA%B7%B8%EB%9E%98%ED%94%BD%EC%8A%A4-%EC%A0%95%EC%A0%90-%EC%B2%98%EB%A6%ACVertex-Processing)

### GPU rendering

- GPU는 polygon mesh를 입력으로 받아서, 각각의 3차원 polygon을 스크린에 그려질 2차원 형태로 바꾸고, 이 2차원 polygon 내부를 차지하는 pixel 들의 색상을 결정한다. 이 pixel은 color buffer에 기록되는데, 이는 스크린에 나타날 pixel 전체를 저장하는 메모리 공간을 말한다. color buffer는 주기적으로 스크린에 복사된다.

- vertex는 program과 동의어이다. GPU rendering을 위해서 vertex shader와 fragment shader 두 가지 프로그램을 작성해야 한다.

- rasterizer와 output merger는 하드웨어로 고정된 단계로, 정해진 연산을 수행한다.

- vertex shader는 vertex array에 저장된 모든 vertex에 대해서 transform을 비록한 다양한 연산을 수행한다.

- rasterizer는 변환된 vertex들로 삼각형들을 조립한 후, 각 삼각형 내부를 차지하는 fragment를 생성한다.
  한 fragment는 color buffer의 한 pixel을 갱신하는 데 필요한 데이터를 총칭한다.

- 예를 들어, 스크린에서 100개의 pixel을 차지하는 삼각형이 있다면, 이 삼각형으로부터 총 100개의 fragment가 만들어진다.

- rasterizer가 출력한 fragment는 하나하나 fragment shader로 입력되어, ligting과 texturing 작업을 거쳐 색상이 결정된다.

- output merger는 이러한 fragment와 현재 color buffer에 저장된 pixel 중 하나를 선택하거나 혹은 이들의 색을 결합하여, color buffer를 갱신한다.

### Vertex Shader

- vertex shader가 수행하는 세 가지 transform

`object space` -> (world transform) -> `world space` -> (view transform) -> `camera space` -> (projection transform) -> `clip space`
