# Computer Graphics

## Part11-1 - Texturing1

#### 2024.12.08.(일)

## Texturing 1

### Fragment Shader

- While the vertex shader outputs the clip-space vertices, the rasterizer outputs a set of fragments at the screen space.

- The per-fragment attributes may include a normal and the texture coordinates.

- Using these data, the fragment shader determines the final color of each fragment.

- Two most important things to do in the fragment shader
  - Lighting
  - Texturing

### Texture Coordinates

- The simplest among the various texturing methods is `image texturing`.

- A texture is typically represented as a 2D array of texels (texture elements). A texel's location can be described by the coordinates of its center.

- Image texturing is often described as pasting an image on an object's surface.

- For texturing, we assign the `texture coordinates`, (s,t), to each vertex of the polygon mesh "at the modeling step," as will be presented soon.

- The rasterizer interpolates them for the fragments.

- The texture coordinates (s,t) are projected into the texture space.

- It is customary to `normalize` the texture coordinates such that s and t range from 0 to 1. The normalized texture coordinates do not depend on a specific texture resolution and can be freely plugged into various textures.

- Multiple images of different resolutions can be glued to a surface without changing the texture coordinates.

### Surface Parameterization

- The texture coordinates are assigned to the vertices of the polygon mesh. This process is called `surface parameterization` or simply parameterization. In general, parameterization requires unfolding a 3D surface onto a 2D planar domain.

### Chart and Atlas

- A complex polygon mesh is usually subdivided into a number of patches such that the patches are unfolded individually. Once unfolded, it is straightforward to assign the texture coordinates to the vertices of the patch.

- The artist draws an image on the unfolded patch using an image editing tool such as Adobe Photoshop.

- An image texture for a patch is often called a `chart`. Multiple charts are usually packed and arranged in a larger texture, which is called an `atlas`.

### Texture Wrapping

- The texture coordinates (s,t) are not necessarily in the range of [0, 1]. The texture wrapping mode handles (s,t) that are outside of the range.
  - Clamp-to-Edge: The out-of-range coordinates are rendered with the edge colors.
  - Repeat: The texture is tiled at every integer junction.
  - Mirrored-Repeat: The texture is mirrored or reflected at every integer junction.

## [[그래픽스] Fragment Shader - Texturing](https://velog.io/@yeomjinseop/%EA%B7%B8%EB%9E%98%ED%94%BD%EC%8A%A4-Fragment-Shader-Texturing)

- Rasterizer 가 출력하는 fragment 별 attribute는 대체로 normal과 texture coordinate을 포함한다.
  fragment shader는 이들을 이용해 fragment의 색상을 계산한다.

- fragment shader가 만드는 최종 영상의 품질을 결정하는 핵심 알고리즘은 ligthning과 texturing으로 구분된다.

### Texturing

- texture는 texel들의 배열이다. (texel은 pixel과 동일한 개념)

- 이 배열이 색상 정보를 저장하고 있다면, 이를 image texture 라고 부른다.

- polygon mesh의 각 vertex 마다 texture coordinate을 할당해야 한다.

- 2차원 texture coordinate은 보통 (s,t) 혹은 (u,v)로 표현하는데, 각 좌표는 원칙적으로 [0,1] 범위 안에 정규화 되어 있다.

### Texture coordinate Interpolation

- Scan conversion 알고리즘을 이용한다.
  먼저 변을 따라 texture coordinate을 interpolation하고,
  이후 scan line을 따라 texture coordinate을 interpolation한다.

### Texture Mapping

- 해상도가 rx x ry인 texture가 주어졌을 때, 정규화된 texture coordinates (s,t)는 아래와 같이 실제 texture space 로 매핑된다.
  이를 가리켜, texture coordinates (s,t)가 texture space (s', t')로 projection 된다고 표현한다.

s' = s x rx
t' = t x ry

### Surface parameterization

- polygon mesh의 각 vertex에 texture coordinate (s,t)를 할당하는 작업을 말한다.

- 이 작업을 위해서는 3차원 mesh를 2차원 평면에 펼쳐야 한다.
  이렇게 펼쳐진 2차원 직사각형 mesh를 [0,1]의 범위를 갖는 st parameter space에 매핑하면,
  각 vertex에 (s,t) texture coordinate이 할당된다.

- 이는 vertex array에 기록되어, vertex shader에게 전달된다.

- 참고로, 복잡한 polygon mesh는 여러 개의 patch로 나눠져 각각 parameterization 된다. parameterization이 완료된 2차원 patch 포토샵 등을 이용해 이미지를 그린다, 이 이미지를 chart라고 부르며, 여러 chart를 하나의 커다란 texture에 모은 것을 atlas라고 부른다.

### Texture Wrapping

- (a) texture
  (b) [0,1] 범위를 벗어난 texture coordinate
  (c) 경계 고정 모드(clamp-to-edge): [0,1] 범위 밖의 texture coordinate를 [0,1] 범위의 경계로 고정
  (d) 반복 모드(repeat): [0,1] 범위 밖의 texture coordinate을 그 소수점 이하만 이용 (ex. 1.1 -> 0.1)
  (e) 반사 반복 모드(mirrored repeat): 1 - 반복모드 값
  (f) 반복 모드 + 반사 반복 보드

### Texture Filtering

- fragment의 texture 좌표 (s,t)는 texture space의 (s', t')으로 투영된다. 그러면, (s', t') 주변의 texel을 고려해서 해당 fragment의 color를 결정해야 하는데, 이를 texture filtering이라고 부른다.

#### texture 확대

1. nearest point sampling

- (s', t')와 가장 가까운 texel 사용

2. bilinear interpolation

- (s', t') 주위를 둘러싸는 네 개의 texel을 bilinear interpolation하여, 해당 pixel의 texture color를 결정.

- 보다 부드러운 texturing 결과.

#### texture 축소

- aliasing: high-frequency 신호를 낮은 frequency로 샘플링할 때에 발생하는 오류. pixel에 비해 texel이 너무 많아서 texturing에 참여하지 못하는 texel이 발생하기 때문에 나타남.

ex) 모든 pixel이 검은색 texel에 둘러싸이게 되면, texturing 결과가 검은색으로 나타남.

- 따라서 anti-aliasing 기술이 필요함.
  해결 방법은 texture를 작게 만들어서 texel 수를 pixel 수에 맞추면 된다.

#### Mipmapping

- 원본 texture의 resolution을 downsampling 한다.
  이때 원본 및 downsampling 된 texture들은 (l+1)개의 레벨을 갖는 texture pyramid로 묶일 수 있다.
  이 pyramid를 `mipmap`이라 부른다.

### Mipmap Filtering

- texture space에 projection된 pixel은 (s', t')에 놓인 '점'이 아니라 (s', t')를 중심으로 일정한 '영역'을 차지하게 된다.
  이 영역을 pixel footprint라고 부른다.

- 0번 level texture 오른쪽 아래 구석 예시처럼 pixel footprint는 2x2 texel 영역을 차지한다.

(연두색: pixel, 노란색: texel)

texel이 pixel보다 너무 많다.

- level을 올라가면서 (점점 texel 개수가 적어지겠지)
  pixel과 texel의 개수가 같은 레벨을 선택한다.

- nearest sampling 혹은 bilinear sampling을 통해 filtering 된다.

### vertex shader and fragment shader
