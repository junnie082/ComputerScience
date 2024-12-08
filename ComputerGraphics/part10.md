# Computer Graphics

# Part 10 - Rasterizer

#### 2024.12.08.(일)

## Rasterizer

### Rendering

- Main stages in the pipeline
  - The `vertex` processing stage operates on every input vertex stored in the vertex buffer and performs various operations such as transform.
  - The `rasterization` stage assembles polygons from the vertices and converts each polygon into a set of fragments. (A fragment refers to a set of data needed to update a pixel in the color buffer. The color buffer is a memory space storing the pixels to be displayed on the screen.)
  - The fragment processing stage operates on each fragment and determines its color.
  - In the output merging stage, the fragment competes or combines with the pixel in the color buffer to update the pixel color.

`modeling` -> `animation` -> `rendering`

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

### Rasterizer

- The vertex shader passes the clip-space vertices to the rasterizer, which assembles the vertices into triangles and then breaks up each triangle into fragments.

`vertex shader` -> `rasterizer` -> `fragment shader` -> `output merger`

- The rasterizer performs the following:
  - Clipping
  - Perspective division
  - Back-face culling
  - Viewport transform
  - Scan conversion

### Clipping

- Clipping is performed in the clip space, but the following figure presents its concept in the camera space, for the sake of intuitive understanding.

  - Triangle t1 is completely outside of the view frustum and is discarded.
  - Triangle t2 is completely inside and is passed as is to the next step.
  - Triangle t3 intersects the view frustum and is thus clipped.

- As a result of clipping, vertices may be added to and deleted from the triangle.

### Perspective Division

- Unlike affine transforms, the last row of Mproj is not (0 0 0 1) but (0 0 -1 0). When Mproj is applied to (x,y,z,1), the w-coordinate of the transformed vertex is -z.

- In order to convert from the homogeneous (clip) space to the Cartesian space, each vertex should be divided by its w-coordinate, which equals -z.

- Note that -z is a positive value representing the distance from the xy-plane of the camera space. Division by -z makes distant objects smaller. It is `perspective division`. The result is said to be in NDC(normalized device)

### Back-face Culling

- The polygons facing away from the viewpoint of the camera are called the `back faces`. They are culled. The polygons facing the camera are called the `front faces`. They are preserved.

- The projection transform defines a universal projection line parallel to the z-axis.

- Let us take only the x- and y-coordinates of every vertex. It corresponds to projecting the triangles along the universal projection line onto the xy-plane. A 2D triangle with CCW vertex order is a front face, and a 2D triangle with CW vertex order is a back face.

- Compute the following determinant, where the first row represents the 2D vector connectinv v1 and v2, and the second row represents the 2D vector connectinv v1 and v3.

| (x2-x1) (y2-y1) |
| (x3-x1) (y3-y1) |

- If it is positive, CCW and so front face.
- If negative, CW and so back face.
- If 0, edge-on face.

* The back faces are not always culled.

  - Consider rendering a hollow translucent sphere. For the back faces to show through the front faces, no face should be culled.
  - Consider culling only the front faces of the sphere. Then the cross-section view of the sphere will be obtained.

* Various GL capabilities are enabled by `glEnable` and disabled by `glDisable`.

* An example is `glEnable (GL_CULL_FACE)`, which enables face culling.

* The default value is `GL_FALSE`.

* When face culling is enabled, `glCullFace()` specifies whether front or back faces are culled. It accepts the following symbolic constants:

  - GL_FRONT
  - GL_BACK
  - GL_FRONT_AND_BACK

* The default value is GL_BACK, and back faces are culled.

* Then, `glFrontFace()` specifies the vertex order of front faces. It accepts the following:

  - GL_CW
  - CL_CCW

* The default value is GL_CCW.

### Viewpoint

- A window at the computer screen is associated with its own `window space` or `screen space`.

- A viewpoint defines a screen-space rectangle into which the scene is projected. It is not necessarily the entire window, but can be a sub-area of the window.

  - void glViewport(GLint minX, GLint minY, GLsizei w, GLsizei h)

- In reality, the screen space is 3D and so is the viewport.

  - void glDepthRangef(GLcampf minZ, GLclampf maxZ)
  - The default values of minZ and maxZ are 0.0 and 1.0, respectively.

- Note that the screen space is left-handed.

- The viewport transform converts the objects in NDC into the screen space.

- In most applications, minZ and maxZ are set to 0.0 and 1.0, respectively, and both of minX and minY are 0.

### Scan Conversion

- Scan conversion breaks up each triangle into a set of `fragments`. It identifies the pixel covered by the triangle and interpolates the vertex attributes (such as normals and texture coordinates) for each pixel location.

- The per-vertex attributes usually do not include RGB color. Just for presentation, however, let's assume color attributes and use R color for scan conversion.

- A horizontal line of pixels is named a scan line.

- Bilinear interpolation - along the edges first, and then along the scan lines

- Color interpolation examples

### Normal Interpolation

- Given (nx, ny, nz) per vertex, each of nx, ny and nz is independently interpolated.

- Then we have the following interpolated normals.

- The per-fragment attributes produced by the rasterizer usually include a normal vector and texture coordinates.
