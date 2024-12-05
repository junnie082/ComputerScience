# Computer Graphics

# Part 7.2 - Polygon Mesh

#### 2024.12.05.(목)

### Polygon Mesh

- Implicit representation vs. explicit representation (in polygon mesh)

- The polygon mesh representation is preferred in real-time applications because the GPU is optimized for processing polygons.

- Note that the mesh's vertices are the points that sample the smooth surface and therefore the polygon mesh is not an accurate representation but an approximate one.

- A triangle is the simplest polygon and the triangle mesh is most popularly used.

- In a typical closed mesh, the number of triangles is approximately twice the number of vertices, e.g., given 100 vertices, we have about 200 triangles.

- Even though the triangle mesh is far more popular in general, the quad mesh is often preferred especially for modeling step.

- A straightforward method to convert a quad mesh into a triangle mesh is to split each quad into two triangles.

- The vertex count of a polygon mesh is decribed as a resolution or level of detail (LOD).

- Tradeoff between accuracy and efficienty: As the resolution increases, the shape of the mesh becomes closer to the original smooth surface, but the time needed for processing the mesh also increases.

### Polygon Mesh - Non-indexed Representation

- The vertices are enumerated in a memory space, named vertex array.

- Three vertices are read in linear order to make up a triangle.

- It is inefficient because the vertex array contains redundant data.

### Polygon Mesh - Indexed Representation

- A better method is using a seperate index array.

  - A vertex appears "only once" in the vertex array.
  - Three indices per triangle are stored in the index array.

- The data stored in the vertex array are not restricted to vertex positions but include a lot of additional information. (All of these data will be presented one by one throughout this class.) Therefore, the vertex array storage saved by removing the duplicate data outweighs the additional storage needed for the index array.

### Surface Normals

- Surface normals play a key role in computer graphics.

- Given triangle <p1, p2, p3>, let v1 denote the vector connecting the first vertex (p1) and the second (p2). Similarly, the vector connecting the first vertex (p1) and the third (p3) is denoted by v2. Then, the triangle normal can be computed using the cross product based on the right-hand rule.

- Every normal vector is made to be a unit vector by default.

- Note that p1, p2, and p3 are ordered counter-clockwise(CCW).

- What if the vertices are ordered clockwise (CW), i.e., <p1, p3, p2> ?

- The vector connecting the first vertex (p1) and the second (p3) and that connecting the first vertex (p1) and the third (p2) define the triangle normal.

- The normal is in the opposite direction!

- The normal direction depends on the vertex order, i.e., whether <p1, p2, p3> or <p1, p3, p2>.

- In computer graphics, surface normals are supposed to point out of the polyhedron. For this, we need CCW ordering of the vertices, i.e., <p1, p2, p3>, instead of <p1, p3, p2>.

- In the index array, the vertices are ordered CCW.

- We have discussed the triangle normals, but more important are the vertex normals. A normal can be assigned to a vertex such that the vertex normal approximates the normal of the smooth surface's point that the vertex samples.

- A vertex normal can be defined by averaging the normals of all the triangles sharing the vertex.

- Modeling packages such as 3ds Max do compute vertex normals.

- Vertex normals are an indispensable component of the vertex array.

### Traingle Meshes
