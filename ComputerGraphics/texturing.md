# Computer Graphics

# [CG - 7. Texture](https://jsdysw.tistory.com/193)

#### 2024.12.12.(목)

### 1. texture

- color의 경우 vertex들의 interpolation을 속을 채운다고 했다.
- 질감도 마찬가지로 특정한 무늬 패턴들의 vertex를 잡아서 interpolation을 수행한다.
- 모양이 복잡하면 복잡할수록 triangle 수가 많아지고 따라서 rendering time이 늘어난다.
- texture 이미지를 3차원 모델에 mapping 시켜서 구현한다...
- 정육면체의 12개의 삼각형에 다음과 같은 질감을 surface에 mapping하는 것이다.

### 2. Texel

- texture에 각각의 pixel을 texel이라고 한다. 각각 2차원의 (u,v) coordinate으로 되어 있다. [0.0, 1, 0]으로 normalize 되어 있다.

- 결국 3D trianglemesh에 2D texture를 매핑시켜야 한다는 것은 각각 하나의 triangle에 texture를 mapping 시키는 것과 같다.

- 각 triangle vertex에 texture coordinate을 정의한다.

### 3. Texture coordinate interpolation

- 다음으로 삼각형이 fragment shader를 거쳐 2D로 표현된다.

- 세 vertex의 texture coordinate를 가지고 특정 픽셀의 texture coordinate를 interpolation으로 구한다.

- 그럼 texture map에서 아까 구한 texture coordinate를 가지고 해당 픽셀의 texture color를 구한다.

- 이전에 구한 texture coordinate(u,v)를 가지고 texture map([0.0, 1, 0])에서 해당 color를 선택할 때 가장 가까운 지점의 color를 고르는 방식이 nearest neighbor 방식이다.

- 이 방식은 aliasing이 심하게 나타날 수 있다...

- 따라서 가장 가까운 점을 선택하는 것이 아니라 네 점으로 interpolation을 수행해서 최종 texture color를 선택하게 된다.

- 정리를 해보자.

- 먼저 scan line conversion 중에 각 픽셀에 대해 texture coordinate를 구한다.

- 이렇게 픽셀마다 texture coordinate를 구하고 나면 2차원의 texture image를 매핑하는 것이다.

### 4. Texture mapping

- planar mapping
- cylindrical mapping은 아래와 같다.
- Spherical Mapping
- cube mapping
- 좀더 사실적인 mapping을 위해서는 surface를 unfold한 map을 만들 수 있다.
- unfold를 쉽게 하기 위한 방법도 있다.
- 물체 표면을 여러 조각으로 나누어 unfold 하고 하나의 surface에 이어 붙이는 것이다.

## [CG - Image Texturing](https://woochan-autobiography.tistory.com/942)

이제 우리는 vertex shader로부터 rasterizer 까지 살펴 보았다. 하나의 삼각형 안에 들어가는 모든 fragment를 계산했다는 것이고, 그 fragment에는 선형보간을 통해 얻어진 normal과 texture coordinate가 저장이 되어있을 것이다. 그 다음 단계는 fragment shader이고 각 fragment의 색상을 결정한다. fragment shader는 texturing과 lighting을 진행한다.

### 1. Texture Coordinates

texture는 모델링한 polygon mesh에 입히는 옷이다. 다음과 같은 원통에 texturing을 진행한다고 해보자.

이미지와 mesh가 주어졌을 때 mesh 표면에다가 texture를 입히는 것이다.
texture는 `texel`이라는 elements로 이루어져 있다. 이전에 계산된 fragment에는 해당 점들에 대응하는 texture 좌표(texel)이 존재하게 된다. 따라서 texture image의 점들(texel)을 polygon mesh의 vertex에 할당하고 이전에 진행했던 것과 같은 방식으로 선형 보간한다.

Texture Coordinates도 역시나 normalize 하는 것이 관례이다. 아래 그림에서 보듯 s축, t축 모두 0과 1 사이의 정사각형 파라미터 범위 안에 존재한다.

위의 그림에서는 6개의 점에 대해서만 texture 좌표(s,t)를 보여주고 있지만 실제로는 모든 정점마다 (s,t) 좌표를 할당한다.

### 2. Surface Parameterization

이러한 방식으로 polygon mesh의 각 vertex에 texture 좌표(s,t)를 할당하는 것을 `surface parameterization`이라고 한다. 이를 위해 mesh를 평면에 펼치는 작업을 해야 한다. 3차원 polygon mesh를 펼치는 것은 왜곡을 가져오는데 이를 최소화하는 것도 매우 중요하다. b처럼 입체적인 것을 평면으로 피는 알고리즘은 구체적으로 다루지는 않겠다.

### 3. Chart & Atlas

polygon mesh 전체를 피는 것이 쉬운 일은 아니다. 얼굴, 상체, 하체 등으로 나눠서 진행해야 한다.

(a)처럼 펼쳐놓은 것을 `chart`라고 하고, (b) chart들의 모임을 `atlas`라고 한다.

### 4. Texture Filtering (Magnification & Minification)

위와 같은 polygon mesh가 주어진다면 scan conversion을 하고, 각각의 fragment마다 (s,t)가 만들어진다. 그러한 (s,t)에다가 오른쪽 그림의 texture의 가로, 세로 해상도를 곱해서 s',t'로 들어간다. 그리고 rgb 값을 가져온다.

여기서 중요한 한가지 문제는 texture의 크기와 polygon mesh의 크기가 다를 때 발생한다.
즉, 해상도가 다른 경우의 해결에 대한 문제이다.

점선 grid에는 원래 texel이 하나씩 있어야 하는데, texel에 비해서 투영된 pixel(fragment)이 훨씬 많을 때가 문제이다.
magnification 문제는 이처럼 texel 보다 pixel이 많을 때 발생한다.

화면에 그려질 때, polygon mesh의 해상도가 50x50 인데 실제는 100x100 이라고 해보자. pixel의 개수랑 texel의 개수랑 비교하면 texel이 훨씬 크다. 그럼 pixel이 large jump를 하게 된다. minification 문제는 이처럼 pixel보다 texel이 많을 때 발생한다.

### 4.1. Filtering for Magnification

먼저 texture image가 작은 경우(pixel이 많은 경우) 텍스쳐 이미지를 확대해야 한다. 반대로 texture image가 큰 경우 축소해야 한다.

texture 확대의 방법은 nearest point sampling, bilinear interpolation 등이 있다.

먼저 `nearest point sampling`은 위의 그림과 같이 n개 (여기선 4개)의 픽셀 모두 하나의 texel로 표현하는 방식이다. 가장 가까운 texel을 가져오는 방식으로 간단하지만 blocky image가 만들어진다. 즉 깨지는 현상이 발생한다.

```
ca = (1-p)c1 + pc2
cb = (1-p)c3 + pc4
c = (1-q)ca + qcb
```

다음으로 가능한 방식은 `bilinear interpolation`이다.
texel의 중심 좌표와 pixel의 투영 좌표를 이용하면, 투영된 pixel의 상대적인 위치를 알 수 있다. (두 texel 사이의 상대적인 위치)

즉, 2번 선형보간하여 해당 pixel의 rgb 값을 선택하는 방식이다. 이를 통해 계단 형태의 blocky image를 줄일 수 있다.

이 외에도 여러 방법이 있지만 여기서는 두가지 간단한 방법을 소개했다.

### 4.2. Filtering for Minification

다음으로는 texture image가 더 큰 경우 texture를 축소해야 한다. 즉, texel의 갯수가 pixel의 갯수보다 많을 때이다.

texture가 높은 경우 아무 문제 없을 것 같지만 그렇지만은 않다. 만약 가장 가까운 점들을 단순 선택한다면 이미지의 특징을 잃어버릴 수 있게 된다.

texturing에 참여하지 못하는 texel들이 있기 때문에 발생하는 문제이다.

(a) 4개의 픽셀 모두 짙은 회색에 둘러 쌓여 있는데, (b) 4개의 픽셀은 모두 밝은 회색에 둘러 쌓여 있다.
두 픽셀이 다른 이미지를 나타내게 된다. 따라서 Minification 문제가 상당하다.

이러한 문제를 앨리어싱(aliasing)이라고 한다. 즉 고주파를 낮은 빈도로 샘플링하는 경우 발생하는 오류를 뜻한다. 이러한 앨리어싱으로 인한 문제를 최소화하기 위한 방법을 안티 앨리어싱이라고 한다.

대표적인 안티앨리어싱 기법으로 `Mipmap` 방식이 있다.

4개의 픽셀을 바인딩해 하나의 픽셀로 만들고 이를 level 1 이라고 한다. 다시 level 1의 2x2를 하나로 바인딩해 level 2 ... level n 까지 단계적으로 texel을 만든다.

level 1에서 pixel과 texel의 비율을 `람다`라고 하고 로그 람다를 이용해 어떤 level을 사용할 것인지 선택한다.
즉, Down sampling을 하는 것이다.

log 람다가 2일 때는 딱 맞게 사용할 수 있다. 만약 람다가 정수가 아닌 유리수라면 어떻게 해야할까? 람다가 3으로 log 람다 = 1.585일 때를 가정해보자.

이때는 level 1 에서도 2에서도 1대1 mapping이 되지 않는다. 이때는 level 1 에서 bilinear interpolation을 진행하고 level 2 에서도 bilinear interpolation을 진행한다. 그 후 마지막으로 level 1 과 level 2에 대해 0.585:1-0.585의 비율로 다시 보간한다.

이런 방식으로 texture filtering을 진행한다. fragment shader가 진행하는 대표적인 두 작업 중 첫번째인 image texturing을 진행하였다.
