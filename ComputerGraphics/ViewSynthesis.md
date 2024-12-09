# Computer Graphics

# View Synthesis - NeRF and 3DGS -

#### 2024.12.08.(목)

### 목차

- Novel View Synthesis

- 기본 배경 지식

  - 장이론 (Field theory)
  - 인공신경망 (Multi-layer perception; MLP)
  - 3D 렌더링 방법들

- Neural Radiance Field (NeRF)

- 3D Gaussian Splatting (3DGS)

### Novel view synthesis

- 컴퓨터 비전 task

- 사물 또는 장면을 촬영한 이미지로부터 새로운 위치, 각도에서의 모습을 렌더링

- 응용: VR/AR, 주행 계획, SLAM, 물체 인식, 3D 모델링, etc.

### 배경

- 장이론 (Field theory)

  - 특정 물리 값을 시간, 공간에 대한 함수 F로 표현
  - 함수의 입력 및 출력 값은 스칼라, 벡터, 텐서

- 인공신경망 (Multi-layer perceptron; MLP)

  - 이전 층의 노드(입력)들에 대하여 특정 출력 값을 갖기 위한 가중치 학습
  - 결과 값을 이용해 손실 함수 계산 및 backpropagation을 통한 가중치 업데이트

- 3D 렌더링 방법들
  - Forward Rendering: 3D 모델을 이미지 평면에 투영 및 래스터화
  - Ray Casting: 카메라 중심으로부터 각 픽셀로 향하는 광선을 계산, 광선과 부딪히는 3D 모델 표면의 색상을 샘플링

### NeRF

- Neural Radiance Field

  - 위치와 방향이 알려진 이미지를 이용해 Novel view synthesis 수행
  - Goal: 신경망 F 가 장면의 색, 밀도 분포를 학습하도록 훈련
  - 볼륨 렌더링: 샘플 점들의 색상 값의 가중치 (T: 투과율, ∂: activated 밀도) 합
  - Technique 1: Hierarchical Sampling
    - 대부분의 공간은 비어 있기 때문에, 이미지를 그리는데 물체의 표면의 위치를 알아내는 것이 중요함.
    - 2개의 신경망을 학습: Coarse network, Fine network
    - Coarse network는 카메라 ray 상의 밀도 분포를 파악하는 데 사용 (확률 밀도 함수).
    - Fine network는 계산된 밀도 분포를 이용해 비균등 샘플링한 점의 색, 밀도 추정.
  - Technique 2: Positional Encoding
    - 인공신경망은 low frequency 입력에서 high frequency 출력을 학습하기 어려워 함.
    - positional encoding을 이용하여 입력을 high frequency로 맵핑
    - 신경망 F 구조

- Neural Radiance Field

  - Rendered Result

- 파생 연구들
  - Deblur-NeRF: 흐릿한 이미지로부터 NeRF 모델 학습
  - Ev-NeRF: 이벤트 카메라 데이터로부터 NeRF 모델 학습
  - iNeRF: 학습된 NeRF 모델을 이용해 카메라의 위치 및 방향 추정
  - Nerfies: 움직이는 동영상으로부터 NeRF 모델을 학습
  - HyP-NeRF: 학습된 NeRF 모델을 생성하는 메타 신경망을 학습

### 3DGS

- 3D Gaussian Splatting
  - NeRF 의 단점: 학습이 느리다 (24 시간 이상 소요), 퀄리티가 좋지 않다. 렌더링 속도가 느리다.
  - Point-based rendering: 3D 포인트의 위치, 색상 값을 학습함. 그러나 holes, aliasing, discontinuous 이슈가 있음.
  - 3DGS: 비등방성의 3D 가우시안 파라미터(위치, 방향, 크기, 색상, 투명도)를 학습
  - Projection: 가우시안들을 이미지 평면에 투영, 카메라에 가까운 가우시안부터 순차적으로 색상 값을 누적(forward rendering)
  - Adaptive density control: 3D 장면을 충분히 표현할 수 있도록 능동적으로 가우시안을 추가하거나 제거함
    - 추가: Under-reconstruction, over-reconstruction
    - 제거: 낮은 투명도 값을 갖는 가우시안 제거

## [[논문 리뷰] NeRF: Representing Scenes as Neural Radiance Fields for View Synthesis](https://mole-starseeker.tistory.com/114)

### 개요

NeRF는 3D 장면을 생성하고 렌더링하기 위한 딥러닝 기반의 컴퓨터 비전 기술이다. 여기서 NeRF의 풀네임은 Neural Radiance Fields 이다. 직역하면 '신경 복사장'이라는 이해할 수 없는 단어가 나온다. 그냥 특정 위치 (물체의 위치 정보와 물체를 바라보는 방향) 값을 입력하면 RGB 값을 출력하는 함수라고 생각하면 된다.

NeRF의 궁극적인 목표는 실제 세계의 3D 장면을 모델링하여, 기존에 촬영하지 않은 새로운 시점에서의 장면 (novel view)을 볼 수 있게 하는 것이다. 기존에는 3D Scanner 등을 사용하여 Point Cloud, Mesh, Voxel 등으로 표현되는 3D 객체를 직접 렌더링했다. 하지만 NeRF는 3D 객체 자체를 생성하는 대신, 객체를 다양한 시점에서 바라보는 장면을 생성하는 `Novel View Synthesis` 기술을 제안한다. 이를 통해 객체를 여러 각도에서 보다 현실감 있게 재현할 수 있다.

NeRF를 구성하는 각 기술에 대한 개요 설명을 하고, 그 다음에 각 기술에 대한 자세한 설명을 하자.

(a) NeRF의 입력은 물체의 위치 정보 (x,y,z)와 물체를 바라보는 방향 (θ, φ)가 함께 있는, 5차원 데이터이다. 이때 ø: 방위각 (좌우), φ: 고도각 (상하)를 나타낸다. 즉, Ray 상에서 샘플링된 점들의 정보 (x, y, z, θ, φ)가 입력으로 사용된다. 이때 Ray는 물체를 찍은 방향으로부터 물체를 향해 일직선으로 쏜 선을 의미한다.

(b) NeRF의 출력은 샘플링된 점들에 대한 RGB 값과 물체의 밀도 σ이다. 입력값으로부터 출력 값을 얻기 위한 모델은 간단한 MLP (다층 퍼셉트론) 네트워크이다. 여기서 말하는 밀도는, 그 값이 커지면 물체가 불투명해지고, 작아지면 물체가 투명해진다는 의미이다. 추가로, 최적화를 위해 `Positional Encoding`을 사용한다.

(c) + (d) NeRF는 출력값을 `볼륨 렌더링` 한다. 볼륨 렌더링이란 MLP에서 얻은 Ray 내 점들의 RGB와 밀도 값을 합쳐 하나의 픽셀로 변환하는 과정을 뜻한다. 볼륨 렌더링을 하는 이유는 바로, 물체의 픽셀 값에 영향을 미치는 것은 Ray 내의 모든 점들이기 때문이다. 여기서도 추가적인 최적화를 위해, `Hierarchical Volume Sampling`을 사용한다. 이후, 실제 픽셀 RGB 값과 볼륨 렌더링으로 예측한 픽셀 RGB 값 간의 MSE, 즉 `렌더링 손실 함수` 를 계산해서 모델을 최적화한다.

#### (a): 5D Input (Position + Direction)

중요한 개념이라 다시 설명드리자면, Ray는 물체를 찍은 방향으로부터 물체를 향해 일직선으로 쏜 선을 의미한다. Ray 내에 포함되는 점들 (그림의 검은색 점들)의 3D 위치 (position) 는 (x,y,z)로 나타내며, Ray 방향을 나타내는 2D 시점 방향 (viewing direction)은 (θ, φ)로 나타낸다.

실제 예시 코드에서는 100개의 이미지와 이에 해당하는 100개의 카메라 포즈 값이 입력된다.
이때 이미지는 (x,y,z), 위치를, 카메라 포즈 값은 (θ, φ), 방향을 결정한다. 카메라 포즈 값은 물체를 찍은 카메라의 위치로 이동시켜주는 '변환 행렬' (transform matrix) 의 값으로 표현하며, 특정 θ, φ에 대한 cos와 sin 값으로 구성된다.
