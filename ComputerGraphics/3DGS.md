# Computer Graphics

# [[논문 리뷰] A Survey on 3D Gaussian Splatting](https://xoft.tistory.com/74)

#### 2024.12.13.(금)

이번에 리뷰 할 논문은 최초의 3D Gaussian Splatting (3DGS) Survey 논문이다. 2024년 1월 8일에 arXiv에 공개되었다. 위 그래프는 2023년에 발표한 3DGS 논문 갯수 통계이다. 3DGS는 7월에 발표되었고 . 그이후 조금씩 발표되다가 11월부터 논문 갯수가 대폭 증가한 것을 볼 수 있다.

개인적으로 3DGS가 Novel-view Synthesis, 3D Reconstruction, Graphics 등 다양한 분야에서 2024년 트랜드가 될 것이라고 생각하고 있기에, survey 논문을 깊이 있게 리뷰해 보았다.

### Novel-view Synthesis 의 변화

2019년 이전까지는 Novel-view Synthesis 분야에 Light Field, SfM(structure-from-motion), MVS(Multi View Stereo) 연구들이 주를 이루었다.

2020년. NeRF(Neural Radiance Field)라는 연구가 발표된다.
3D 좌표를 입력으로 color와 density 출력하는 neural network를 설계하여, 적은 수의 이미지로 photorealistic한 랜더링을 할 수 있게 됐다. continuous하고 volumetric한 scene을 생성할 수 있었고, 이전에는 볼 수 없었던 디테일과 현실감을 가진 이미지를 합성해내게 된다. 발표 당시 Computer Graphics과 3D Reconstruction 분야에서 이정표(milestone)가 되는 연구였고, Image Synthesis 분야에서 새로운 패러다임이었다.

그 이후 수많은 NeRF 논문들이 나왔으나 연산 효율성(computation efficiency)와 제어성(controllability) 관점에서 한계가 있었다. pixel 마다 ray marching으로 3D point를 샘플링하기에 학습시간과 랜더링 시간이 오래 소요된다. 또한 랜더링시에 리소스(CPU, GPU, RAM)가 많이 사용된다. 고해상도일수록 랜더링이 어렵다.

### 3D Gaussian Splatting의 등장

이러한 흐름 속에 3D Gaussian Splatting이 발표된다. 단순한 성능 개선이 아니라 Scene Representation과 Rendering 분야에 새로운 패러다임을 만드는 방식으로 등장한다.

NeRF가 implicit representation과 coordinate-based 모델이었다면, 3DGS는 기본적으로 수많은 3D 가우시안으로 scene을 표현하는 방식이며, explicit representation, differentiable(미분가능한) pipeline, point-based rendering, no neural network를 특징으로 가진다. (implicit은 neural network의 weight처럼 parameter 간의 관계성을 해석할 수 없는 경우에 사용되고, explicit은 parameter를 해석 가능할 경우에 사용된다.)
이에 기반하여 병렬처리, 효율적인 연산, 효율적인 랜더링이 가능하게 된다.

위 그림은 본 논문에서 소개하고 있는 3DGS 알고리즘이다. 리마인드하며 아래 글을 읽을 때 참조하길.

- NeRF와 동일하게 high-quality image synthesis를 위해 필요한 continuous volumetric radiance field의 특성을 가지면서, NeRF에서는 하지 못했던 empty 한 공간에서의 비효율적인 연산을 피할 수 있게 되었다.

- 3D gaussian 들을 image plane으로 projection하고, 각 pixel 마다 gaussian 들을 sorting 한다. 언뜻보기에 연산량이 많아 보이지만, 1개의 이미지를 여러 개의 Patch 이미지(Tile)로 나누어 병렬적으로 연산함으로써 연산 속도와 연산 효율성을 향상시킨다.

- Anisotropic Gaussian 으로 설계하면서 compact하게 공간을 표현하여 메모리 load를 줄이게 되었고, scene을 정교하게 표현할 수 있게 되었고, 다양한 scale로 복잡한 scene을 표현할 수 있게 되었다. 특히 dynamic scene도 잘 표현한다.

3DGS는 높은 visual quality를 가지며 realtime 랜더링이 가능하고, 다양한 조명 환경과 복잡한 geometries 를 가진 시나리오에서 explicit representation을 통해 dynamic하게 scene 수정(control)이 가능하게 되면서, VR, AR, 영화분야까지 다양한 application 제작 가능성이 열리게 되었다.

### Terminology

- Radiance Field: 3차원 공간에서 light의 distribution 표현(representation) 방법 중에 하나이다. light와 surface/material의 상호작용을 보여주는 field(장)으로 볼 수 있다. 모든 방향에서 light의 양을 설명하는 함수로 표현한다.

- Implicit Radiance Field: scene의 geometry를 explicit하게 정의하는 것 없이 light의 distribution을 나타낸다. radiance가 explicit하게 저장되지 않는다. 즉각적인 쿼리를 통해 획득한다.

- Explicit Radiance Field: discrete spatial structure(이산화된 공간 구조)에서 light의 distribution을 나타낸다. radiance를 각 공간상 위치에 저장한다. voxel grid 방법과 point set 방법이 explicit 하다고 볼 수 있다. 빠르다는 장점과 함께 메모리가 많이 소모된다는 단점이 있다.

- Scene Reconstruction: 여러 이미지들 또는 데이터들의 집합으로부터 scene의 3D model을 만든다.

- Rendering: 컴퓨터가 읽을 수 있는 정보를 pixel-based image로 바꾼다.

- Neural Rendering: 딥러닝과 Graphic 기술을 활용한 Rendering이다.

- Volumetric Representation: (material 공간 또는 empty 공간을 채우는 형태의) surface와 volume으로 object와 scene을 표현한다. fog, smoke, 반투명한 것을 세밀하게 랜더링하여 표현할 수 있다.

- Ray-Marching: volume representation에서 volume을 지나가는 light의 경로를 점진적으로 추적하여 이미지 랜더링을 위해 사용되는 기술이다. NeRF에서 quality를 향상시키기 위해 사용되지만 연산량이 많은 문제가 있다.

- Point-based Rendering: 전통적인 polygon(=3D mesh를 구성하는 다각형) 방식이라기 보다 point를 사용해서 3D Scene을 visualization하는 기법이다. 복잡하고 구조가 없고 sparse(=드문드문)한 geometric data를 랜더링하는데 특히 효과적이다. 다만 scene에 hole 생기거나 aliasing(=배율에 따른 계단현상)이 발생할 수 있다.

### Applications & Tasks

3DGS가 쓰이는 Robotics, Scene Reconstruction, AI 생성 콘텐츠, 자율주행 분야에 대해 소개한다.

SLAM (Simultaneous Localization and Mapping)
전통적인 SLAM은 point/surfel clouds 또는 voxel grid를 사용했지만 3DGS는 anisotropic gaussian을 이용한다. 아래는 3DGS를 사용한 5가지 연구이다.

- GS-SLAM: 3DGS로 Scene을 표현했다. 3DGS의 efficiency(=적은 수의 gaussian으로 scene을 표현)와 accuracy(=gaussian으로 detail한 scene을 표현)에 균형을 맞추고, map optimization을 상당히 개선하고 RGB-D mapping을 re-rendering 했다.

- SplaTAM: post가 주어지지 않는 monocular RGB-D로 3D Gaussian을 사용했다. rendering speed와 map accuracy를 향상시켰다.

- Photo-SLAM: Neural Rendering과 SLAM을 통합해서, 3DGS로 효율적이로 high-quality의 map을 만들어냈다. portable한 장비에서 Photorealistic한 map을 만들고 rendering 속도를 상당히 개선하게 된다.

- PVG, Street gaussians: dynamic한 urban scene을 모델링했다.
