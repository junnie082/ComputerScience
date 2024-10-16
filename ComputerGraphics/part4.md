# Computer Graphics

# Part 4 - Pixel and Filter

#### 2024.10.08.(화)

## 화소 처리 (Pixel Point Processing)

### 화소 처리 (Pixel Point Processing)

전통적인 영상 처리는 크게 두 가지로 나누어진다.

- 화소 처리
- 공간 필터링

### 화소 처리란?

> > 화소처리는 `입력 영상의 화소값이 수학적인 함수를 거쳐서 새로운 값으로 변경된 후에 출력 영상의 동일한 위치에 저장`된다. 화소 처리에서 각 출력 화소값은 해당 입력 화소값에만 의존한다. 이것은 출력 화소의 값을 결정할 때, 이웃한 화소들의 값을 사용하는 `공간 필터링`과 비교된다.

### 화소처리의 대표적인 예

- 밝기 및 콘트라스트(contrast) 조정
- 색상 교정
- 색상 변환

### 화소를 하나씩 처리하는 방법

- 방법 1: 모든 화소에 값 더하기

```Python
img = cv2.imread('lena.png', 0) # grayscale
plt.imshow(img, cmap = 'gray')

img += 30
plt.imshow(img, cmap = 'gray')
```

> > 실행 결과가 약간 이상하다. 약간 밝게 되었으나 중간에 검은색이 보인다. 이유는 값을 더하면서 `255을 넘게 되어서 오버플로우가 일어난 것이다.` OpenCV에서는 `cv2.saturate_cast()`라는 함수를 제공한다. 이 함수는 255가 넘는 값은 그냥 255로 고정시킨다. `파이썬에서는 np.clip() 함수로 구현할 수 있다.`

```Python
img = cv2.imread('lena.png', 0)

new_img = np.clip(img+100., 0, 255).astype(np.uint8)
plt.imshow(new_img, cmap = 'gray')
```

- 방법 2: OpenCV 함수 사용하기, convertTo()

> 영상의 밝기를 증가시키는 것은 OpenCV에서 이미 함수로 지원하고 있다. 바로 `convertTo()` 함수이다. `성능에서도 가장 효율적인 방법은 OpenCV의 함수 convertTo() 함수를 사용하는 것이다.` 가능하다면 OpenCV가 제공하는 함수를 사용하는 것이 시간 면에서 가장 효율적이다. `파이썬에서는 cv2.convertScaleAbs()를 사용한다.`

### 밝기 및 콘트라스트 조정

> 가장 많이 사용되는 화소처리는 화소의 값에 상수를 곱하거나 더하는 연산이다. g(x,y) = alpha \* f(x,y) + beta
> alpha > 0 와 beta 는 종종 이득 및 바이어스라고 불린다. 여기서 alpha 가 1.0 보다 커지면 영상의 콘트라스트가 증가할 것이다. alpha가 1.0 보다 작으면 영상의 콘트라스트는 감소할 것이다. beta가 양수이면 영상은 밝아질 것이고 반대로 음수이면 영상은 어두워질 것이다. 화소 처리 연산 시에 출력 화소의 값은 항상 [0:255] 범위에 있어야 하기 때문에 255보다 큰 화소값은 255로 제한된다.

- converTo()함수 사용하기

콘트라스트 증가 alpha = 1.3, beta = 0

```Python
#(358, 634, 3)
img = cv2.imread('car_plate.jpg')
plt.imshow(img)

contrast_img = np.zeros((358, 634, 3), np.uint8)
cv2.convertScaleAbs(img, contrast_img, 1.3, 0)

plt.imshow(contrast_img)
```

### 기타 화소 처리

#### 선형 콘트라스트 확대

> 알고리즘
> for 0 <= input <= x1
> output = y1 / x1 _ input
> for x1 <= input <= x2
> output = ((y2 - y1) / (x2 - x1)) _ (input - x1) + y1
> for x2 <= input <= 255
> output = ((255 - y2) / (255 - x2)) \* (input - x2) + y2

> 이 코드는 상당한 시간이 걸릴 수 있다. 따라서 다음에 설명하는 LUT 방법을 사용하는 것이 좋다.

```Python
image = cv2.imread('lena.png', 0)
plt.imshow(image, cmap = 'gray')

x1, y1 = 70, 0
x2, y2 = 150, 255

for i in range(image.shape[0]):
      for j in range(image.shape[1]):
          val = image[i][j]

          if (0 <= val and val <= x1):
              image[i][j] = y1 / x1 * val
          elif (x1 < val and val <= x2):
              image[i][j] = ((y2 - y1) / (x2 - x1)) * (val - x1) + y1
          elif(x2 < val and val <= 255):
              image[i][j] = ((255 - y2) / (255 - x2)) * (val - x2) + y2

plt.imshow(image, cmap = 'gray')
```

```Python
image = cv2.imread('lena.png', 0)
plt.imshow(image, cmap = 'gray')

x1, y1 = 70, 0
x2, y2 = 150, 255

for i in range(image.shape[0]):
    for j in range(image.shape[1]):
        val = image[i][j]

        if (0 <= val and val <= x1):
            image[i][j] = y1 / x1 * val
        elif (x1 < val and val <= x2):
            image[i][j] = ((y2 - y1) / (x2 - x1)) * (val - x1) + y1
        elif (x2 < val and val <= 255):
            image[i][j] = ((255 - y2) / (255 - x2)) * (val - x2) + y2

plt.imshow(image, cmap = 'gray')
```

### 반전

> 반전은 영상의 어두운 부분과 밝은 부분을 반대로 하는 것이다.

```Python
img = cv2.imread('lena.png', 0)
plt.imshow(img, cmap = 'gray')

image = 255 - img
plt.imshow(image, cmap = 'gray')
```

### 이진화 (Thresholding)

> 이진화는 가장 간단한 영상 분할(image segmentation) 방법이다. 이진화는 분석하려는 영상에서 물체와 배경을 분리한다. 이진화는 기본적으로 물체와 배경 사이의 밝기 차이를 이용하여 영역을 구분한다. 미리 설정된 임계값과 각 화소값은 비교하여 임계값보다 낮은 화소는 검은색(0)으로, 임계값과 같거나 높은 화소는 흰색(255)으로 바꾼다.

- OpenCV 에서는 이진화를 위하여 `threshold()` 함수를 제공한다.

g(x,y) = {0 if f(x,y) <= T, 255 if f(x,y) > T}

```Python
img = cv2.imread('lena.png', 0)
plt.imshow(img, cmap = 'gray')

threshold_value = 127
ret, thresh_cv = cv2.threshold(img, threshold_value, 255, cv2.THRESH_BINARY)

print(ret) # threshold 기준값 반환
plt.imshow(thresh_cv, cmap = 'gray')
```

### LUT(Look-Up Table)

> 룩업 테이블도 화소 처리에서 상당히 많이 사용되는 방법이다. `입력 영상의 각 화소들을 룩업 테이블을 통과시켜서 출력 영상으로 바꾸게 된다`. LUT 변환에서는 동일한 값을 갖는 두 개의 화소는 출력에서도 동일한 값을 가지게 된다. 룩업 테이블에서 인덱스는 입력 화소값을 나타내고 인덱스에 의해 제공된 셀의 내용은 해당 출력값을 나타낸다.

#### 색상의 수를 감소시키는 작업 구현

> lout = (lin / 100) \* 100

```Python
img = cv2.imread('lena.png', 0)
plt.imshow(img, cmap = 'gray')
img2 = np.zeros((256, 256), dtype = np.uint8)

table = np.zeros(256, dtype = np.uint8)

for i in range(256):
    table[i] = (int)((i//100) * 100)

img2 = cv2.LUT(img, table)
plt.imshow(img2, cmap='gray')
```

### 감마 보정

> 감마 보정은 입력 화소값을 `비선형 변환`을 통하여 출력 화소값으로 바꾸는 처리로서 영상의 밝기를 보정하는 데 사용할 수 있다. `gamma < 1이면 원래의 어두운 영역은 더 밝아지고, 히스토그램은 오른쪽으로 이동된다. gamma > 1이면 반대가 된다.` `인간의 눈은 디지털 카메라의 센서와 차이가 있어서 색상이나 휘도가 다르게 인식된다.` 디지털 카메라에서 센서가 2배 더 많은 광자를 받는다면 센서의 출력도 2배가 된다. (즉 선형적인 관계가 있다.) `하지만 인간의 눈에서는 광자의 수가 2배가 되었다고 해서 출력이 2배가 되지 않는다. 즉, 비선형적인 관계가 된다.` 또한 인간의 눈은 밝은 톤보다 어두운 톤의 변화에 훨씬 더 민감하다.

```Python
import math

img = cv2.imread('lena.png')
img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
plt.imshow(img)
img2 = np.zeros((256, 256, 3), dtype = np.uint8)

table = np.zeros(256, dtype = np.uint8)
gamma = 0.5

# 룩업 테이블 설정
for i in range(256):
    table[i] = math.pow(i/255.0, gamma) * 255.

img2 = cv2.LUT(img, table)

plt.imshow(img2)
```

### 영상 합성

> 2개의 영상의 화소값을 결합하여 출력 영상에 저장할 수 있다. `g(x,y) = f1(x,y) + f2(x,y) 여기서 f1과 f2는 2개의 입력 영상을 나타낸다.` 위 식에는 + 연산을 하였지만 결합함수는 수학적이거나 논리적인 연산자, 즉 +, -, X, /, AND, OR, XOR 중의 하나가 될 수 있다.

#### 선형 영상 합성

> `g(x,y) = (1 - alpha) * f1(x,y) + alpha * f2(x,y)` 이때 사용되는 함수는 `addWeighted()` 이다.

- 매개변수
  1. src1: 첫번째 입력 영상
  2. alpha: 첫번째 영상의 가중치
  3. src2: 두 번째 입력 영상
  4. beta: 두 번째 영상의 가중치
  5. gamma: 화소의 합계에 더해지는 값
  6. dst: 출력 영상

```Python
src1 = cv2.imread('road_image.jpg') # (600, 800, 3)
src2 = cv2.imread('watermark_no_copy.png') # (1280, 1277, 3)

src1 = cv2.cvtColor(src1, cv2.COLOR_BGR2RGB)
src2 = cv2.cvtColor(src2, cv2.COLOR_BGR2RGB)

src2 = cv2.resize(src2, (800, 600))

alpha = 0.5
beta = 1 - alpha

dst = cv2.addWeighted(src1, alpha, src2, beta, 0.0)

plt.imshow(dst)
```

### 논리적인 영상 합성

> OpenCV에서는 2개의 영상을 가지고 비트별로 AND, OR, XOR과 같은 논리적인 연산을 적용할 수 있다. 이들 연산은 영상의 어떤 부분을 관심 영역(ROI)으로 정의하고 이 부분을 추출할 때 유용하다. 특히 정사각형 모양이 아닌 다른 모양을 관심 영역(ROI)을 정의할 때 필요하다.

- 매개변수
  1. src1: 첫 번째 입력 영상
  2. src2: 두 번째 입력 영상
  3. dst: 출력 영상
  4. mask: 영향을 받는 화소들을 지정하는 마스크 영상 (선택 사양)

```Python
src1 = cv2.imread('road_image.jpg') # (600, 800, 3)
src1 = cv2.cvtColor(src1, cv2.COLOR_BGR2RGB)

black_img = np.zeros((600, 800, 3), dtype = np.uint8)
black_img[200:400, 250:550] = 255
```

```Python
result = cv2.bitwise_and(src1, black_img)
plt.imshow(result)
```

> `여기서 주의해야할 사항은 입력영상과 마스크 영상은 크기와 유형이 같아야 한다는 점이다.` 비트별로 AND 연산을 하기 때문에 두 번째 영상 화소의 값이 0이면 결과 영상의 화소값도 무조건 0이 된다. 또 두 번째 영상 화소의 값이 255이면 결과 영상의 화소값은 첫 번째 영상과 같게 된다.
