# Computer Graphics

# Part 7.1 - 3D Computer Graphics

#### 2024.12.02.(월)

### 3D Computer Graphics

- Three-dimensional (3D) computer graphics takes as input 3D representations of objects and performs various calculations on them to produce images called frames.

- An illusion of movement is generated on the screen by displaying a sequence of changing frames.

* If the frames are produced online, we call it real-time graphics.

  - More than 30 frames per second (fps)
  - Games, virtual/augmented reality, and interactive user interfaces (UIs)

* On the other hand, visual effects in films often take as long as minutes or hours for a single frame. In return, we may obtain photorealistic images.

* The algorithms and techniques adopted in real-time graphics are fairly different from those in off-line graphics.

* This class is on real-time graphics.

* Major steps in computer graphics production

`modeling` -> `rigging` -> `animation` -> `rendering` -> `post-processing`

- Graphic artists and programmers are the key players.
  - Modeling, rigging, and animation are off-line tasks performed by the artists.
  - At run time, computer programs replay the animation and perform rendering and post-processing.

### 3D Computer Graphics - modeling

- A model is referred to as a computer representation of an object, and modeling is the process of creating the objects comprising the virtual scenes.

- Almost all 3D models in real-time graphics are represented in polygon meshes.

- The scope of modeling includes creating textures. The simplest form of a texture is an image that is pasted on an object's surface at run time.

### 3D Computer Graphics - rigging

- The baseball player should be able to hit a ball, run, and slide into a base, i.e., we need to animate the player.

- For this purpose, we usually specify the skeleton or rig of the player. We then define how the skeleton motion deforms the player's polygon mesh such that, for example, the polygons of the arm are made to move when the arm bone is lifted. This process is often referred to as rigging.

### 3D Computer Graphics - animation

- The graphics artist creates a sequence of skeleton motions. At run time, the skeleton motions are replayed and the polygon mesh is animated over frames.

- The artists perform modeling, rigging, and animation in an off-line mode. Dedicated programs such as Autodesk 3ds Max and Autodesk Maya are popularly used.

* Rendering is the process of generating a 2D image from a 3D scene. The image makes up a frame.

* Realistic rendering is a complicated process, in which lightning as well as texturing is an essential component. For example, the shadow shown below is a result of lightning.

### 3D Computer Graphics - post-processing

- As an optional step, post-processing uses a set of special operations to give additional effects to the rendered images.

- An example is motion blur. When a camera captures a scene, the resulting image represents the scene over a short period of time. Consequently, rapidly moving objects may result in motion blur.

- Unlike modeling, rigging, and off-line animation made by artists, run-time animation, rendering, and post-processing are by an application program.

- In games, the application is typically built upon a game engine. A game engine is a development tool that provides a suite of indispensable modules for animation, rendering, and post-processing.

- In general, a game engine is built upon 3D graphics APIs such as Direct3D and OpenGL. OpenGL ES(OpenGL for Embedded Systems) is a 3D API for handheld and embedded devices and is defined as a subset of OpenGL.

## Math Basic for Computer Graphics

### Matrices and Vectors

- This chapter delivers an explicit presentation of the basic mathematical techniques, which are needed throughout this book.

- mxn matrix

[a11 a12 ... a1n]
[a21 a22 ... a2n]
[ . . . ]
[ . . . ]
[ . . . ]
[am1 am2 ... amn]

- If m=n, the matrix is called square.

- Matrix-matrix multiplication

AB = [a11 a12] [b11 b12 b13]
[a21 a22] [b21 b22 b23]
[a31 a32]

= [a11b11 + a12b21 a11b12 + a12b22 a11b13 + a12b23]
[a21b11 + a22b21 a21b12 + a22b22 a21b13 + a22b23]
[a31b11 + a32b21 a31b12 + a32b22 a31b13 + a32b23]

- If A's dimension is I x m and B's dimension is mxn, AB is an I x n matrix.

- The typical representation of a 2D vector, (x,y), or a 3D vector, (x,y,z), is called a row vector. Instead, we can use a column vector:
  [x] [x]
  [y] [y]
  [z]

- Matrix-vector multiplication

Mv = [a b] [x]
[c d] [y]
[e f]

    = (ax + by)
      (cx + dy)
      (ex + fy)

- Transpose denoted by M^T (a c e)
  (b d f)

                    = (ax + by cx + dy ex + fy)

- Whereas OpenGL uses the column vectors and the vector-on-the-right representation for matrix-vector multiplication, Direct3D uses the row vectors and the vector-on-the-left representation.

- Identity matrix denoted by I

[1 0] [1 0 0]
[0 1] [0 1 0]
[0 0 1]

- For any matrix M, MI = IM = M.

[a11 a12 a13] [1 0 0] [a11 a12 a13]
[a21 a22 a23] [0 1 0] = [a21 a22 a23]
[a31 a32 a33] [0 0 1] [a31 a32 a33]

[1 0 0] [a11 a12 a13] [a11 a12 a13]
[0 1 0] [a21 a22 a23] = [a21 a22 a23]
[0 0 1] [a31 a32 a33] [a31 a32 a33]

- If two square matrices A and B are multiplied to return an indentity matrix, i.e., AB = I, B is called the inverse of A and is denoted by A^-1. By the same token, A is the inverse of B.

- Theorems

  - (AB)^-1 = B^-1A^-1 <- (AB)(B^-1A^-1) = A(BB^-1)A^-1 = AIA^-1 = AA^-1 = I
  - (AB)^T = B^TA^T (Revisit Mv and v^TM^T presented in the previous page)

- Consider the coordinates of a 2D or 3D vector, v:
  (vx, vy) (vx, vy, vz)

- Its length denoted by ||v||

- Dividing a vector by its length is called normalization.
  v / ||v||

- Such a normalized vector is called the unit vector since its length is 1.

### Coordinate System and Basis

- Coordinate system = origin + basis

- Throughout this book, we use the terms, coordinate system and space, interchangeably.

- In the 2D space, every vector can be defined as a linear combination of basis vectors. Consider (3,5) for the following three examples.

- An orthonormal basis is an orthogonal set of unit vectors.

- 3D standard basis, {e1, e2, e3}, where e1 = (1,0,0), e2 = (0,1,0), and e3 = (0,0,1).

- It is also orthonormal.

- Of course, we can consider non-standard orthonormal bases in 3D space. You will see soon.

- All 2D and 3D bases presented from now on will be orthonormal.

### Dot Product

- Given two n-dimensional vectors, a and b, whose coordinates are (a1, a2, .., an) and (b1, b2, .., bn), respectively, their dot product a\*b is defined to be a1b1 + a2b2 + ... + anbn.

- Geometric interpretation of the algebraic formula: When the angle between a and b is denoted as ø, a\*b can be also defined as ||a||||b||cosø.

  - If a and b are perpendicular to each other, a\*b = 0.
  - If ø is an actual angle, a\*b > 0.
  - If ø is an obtuse angle, a\*b < 0.

- Note that, if v is a unit vector, v\*v = 1.

- Every orthonormal basis has an interesting and useful feature. See two examples.

  - 2D standard basis, {e1, e2}

    - e1*e1 = 1 and e2*e2 = 1
    - e1*e2 = 0 and e2*e1 = 0.

  - 3D standard basis, {e1, e2, e3}
    - If i=j, ei\*ej=1.
    - Otherwise, ei\*ej=0.

- This feature applies to non-standard orthonormal bases.

### Cross Product

- The cross product takes as input two 3D vectors, a and b, and returns another 3D vector which is perpendicular to both a and b. It's denoted by axb and its direction is defined by the right-hand rule.

- The length of axb equals the area of the parallelogram that a and b span:
  ||a||||b||sinø.

- If a=b, axb returns the zero vector, often denoted as 0.

- The right-hand rule implies that the direction of bxa is opposite to that of axb, i.e., bxa = -axb, but their lengths are the same.

- In the book, [Note: Derivation of cross product] shows how the coordinates of axb are derived from those of a and b. See below for an intuitive derivation.

- If a=(ax, ay, az) and b=(bx, by, bz), a x b = (aybz - azby, azbx - axbz, axby - aybx).

### Line, Ray, and Linear Interpolation

- A line defined by two end points, p0 and p1: p(t) = p0 + t(p1 - p0)

- If t is in [-infinite, infinite], p(t) is an infinite line. If [0, infinite], p(t) is a ray, which starts from p0 and is infinitely extended along the direction vector, p1 - p0.

- When t is restricted to [0, 1], p(t) represents the line segment, which corresponds to the linear interpolation of p0 and p1.

- It is a weighted sum of p0 and p1.

- Linear interpolation in 3D space

p(t) = [x(t)] [(1-t)x0 + tx1]
[y(t)] = [(1-t)y0 + ty1]
[z(t)] [(1-t)z0 + tz1]

- Whatever attributes are associated with the end points, they can be linearly interpolated. Suppose that the endpoints are associated with colors c0 and c1, respectively, where c0 = (R0, G0, B0) and c1 = (R1, G1, B1). Then, the color c(t) is defined as follows:

                          [(1-t)R0 + tR1]

  c(t) = (1-t)c0 + tc1 = [(1-t)G0 + tG1]
  [(1-t)B0 + tB1]
