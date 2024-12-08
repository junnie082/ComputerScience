# Computer Graphics

## Part11-2 - Texturing2

#### 2024.12.08.(일)

### Texture Filtering - Magnification & Minification

- Consider the quad shown below. For each fragment located at (x,y) in the screen, its texture coordinates (s,t) are projected onto (s',t') in the texture space.

- Unlike the contrived examples given so far, (s',t') may not fall onto the center of a texel. Instead, texels around (s',t') are collected and combined to determine the color of the fragment. This process is called `texture filtering`.

- The screen-space quad appears larger than the texture, and therefore the texture is `magnified` so as to fit to the quad. There are more fragments (henceforth, simply pixels) than texels.

- In contrast, the screen-space quad may appear smaller than the texture. Then, the texture is `minified`.

  - There are more texels than pixels.
  - Sparsely projected onto the texture space, the pixels take large jumps in the space.

- Summary
  - Magnification: more pixels than texels
  - Minification: more texels than pixels

### Filtering - Magnification

- Option 1: Nearest point sampling

  - A block of projected pixels can be mapped to a single texel.
  - Consequently, a blocky image is often produced.

- Option 2: Bilinear interpolation
  - Four texels surrounding a projected pixel are bilinearly interpolated.
  - The adjacent pixels are likely to have different texture colors, and the textured result does not suffer from the blocky-image problem.

### Filtering - Minification

- Consider a checker-board image texture and a smaller quad.

  - If all pixels are surrounded by dark-gray texels, the quad appears dark gray.
  - If all pixels are surrounded by light-gray texels, the quad appears light gray.

- This problem is an instance of `aliasing`. It refers to a sampling error that occurs when a high-frequency signal (in our example, the checkerboard image texture) is sampled at a lower resolution (in our example, the sparsely projected pixels).

- We need an anti-aliasing technique to reduce the aliasing artifact.

### Aliasing

- Polygon Aliasing

  - When the polygon is too small to be on a scan line, the process ignore the polygon.

- Aliasing
  - When the sampling rate is too low.

### Mipmap

- The aliasing problem observed in minification is caused by the fact that we have more texels than pixels. The pixels take large jumps in the texture space, leaving many texels not involved in texture itering.

- Then, a simple solution is to reduce the number of texels such that the texel count becomes as close as possible to the pixel count.

- For decreasing the texture size, down-sampling is used.

- Given an original texture of 2^l x 2^l resolution, a pyramid of (l+1) levels is constructed, where the original texture is located at level 0. The pyramid is called a `mipmap`. The level to filter is named `level of detail`, and is denoted by λ. We will look for the level whose texel count is close to the pixel count.

### Mipmapping

- A pixel covers an area on the screen. For simplicity, take the area as square. Then a pixel's projection onto the texture space is not a point but an area centered at (s', t'). The projected area is called the `footprint` of the pixel.

- In this contrived example, the quad and level-1 texture have the `same size`. A pixel's footprint covers 2x2 texels in level 0, but covers a single texel in level 1.

- When a pixel footprint covers mxm texels of level-0 texture, λ is set to log2m.

- In the example, the screen-space quad and level-2 texture have the same size. A pixel's footprint covers exactly a single texel in level-2 texture, which is then filtered.

- In this and previous examples, observe that all texels are involved in filtering.

- In the following example, λ = log2(3) = 1.585.., and so we use levels 1 and 2. In general, they are λ and λ

- Between λ and λ, which one to choose?

  - We can take the nearest level, λ+0.5.
    - At the level, we can either take the nearest point
    - or use bilinear interpolation.
  - We can take both of λ and λ and linearly interpolate the filtering results.
    - At each level, we can either take the nearest point
    - or use bilinear interpolation.

- The last case is called `trilinear` interpolation.

### Vertex and Fragment Shaders

- What are provided for the fragment shader?

  - The per-fragment inputs, v_normal and v_texCoord.
  - Uniforms such as textures.

- The fragment shader may output multiple colors. For the sake of simplicity, however, this book considers outputting only a single color.

## [개발용어) 밉맵(Mipmap)](https://drehzr.tistory.com/666)

밉맵은 3D 그래픽스에서 텍스쳐를 씌우는 과정에서 렌더링 속도를 향상하기 위한 축소된 패턴이다. 밉맵(mipmaps, Mip map)이라고도 표현을 하는데 의미는 같다.

2D에서는 패턴이 비슷한 개념이다. 밉맵은 LOD(Load of Detail)에 의해서 결정되는데 가까이 갈수록 비율이 큰 텍스쳐를 세팅하는 방식이다. 스케일링을 해야 하기 때문에 2의 배수로 세팅을 해야 한다.

256 x 256 텍스쳐를 읽으면 내부에서는 (128 _ 128, 64 _ 64, 32 _ 32, 16 _ 16... 2 \* 2)의 텍스쳐를 만들어서 메모리에 올려둔다.

이렇게 세 부화된 밉맵이 있으면 실제 적용되는 건 다음과 같다.

카메라를 기준으로 화면이 이중 선형 필터링해서 텍스쳐를 그려준다. 단순하게 밉맵을 사용하면 저렇게 경계선이 보이는 등, 왜곡이 생기기 때문에 특별한 필터링이 필요하다.
복도 바닥을 예시로 보면 원근감 때문에 수평과 수직의 맵 스케일링을 달리한다.

반대로 밉맵을 처리 안하고 보게 되면 다음과 같다.

왼쪽은 밉맵이 적용되지 않았는데 한 화면에 많은 픽셀이 복잡하게 있을 경우에는 많은 양의 노이즈가 생기는 듯하게 보인다. 밉맵의 장점을 잘 활용해서 완성된 텍스쳐가 아닌 밉맵으로 용량도 줄이고 자연스러운 렌더링을 만들어야 한다.

실제 밉맵을 사용하면 메모리는 30%가 늘어난다고 한다. 런타임 성능은 좋아진다. 3D를 표현하는 대부분의 그래픽에서는 'Mipmap'의 옵션이 있다.
