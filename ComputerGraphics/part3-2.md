# Computer Graphics

# part 3-2 - Camera and pixel

#### 2024.10.24.(목)

### ISP (image sensor pipeline)

- Receives sensor data, and optionally transforms it

  - untransformed raw data must also be available

- Computes helpful statistics
  - histograms, sharpness maps

The sequence of image processing operations applied by the camera's image signal processor (ISP) to convert a RAW image into a "conventional" image.

`analog front-end` -> (RAW image mosaiced, linear, 12-bit) -> `white balance` -> `CFA demosaicing` -> `denoising` -> `color transforms` -> `tone reproduction` -> `compression` -> final RGB image (non-linear 8-bit)

## [CMOS 이미지센서(CIS)](https://news.samsung.com/kr/%EC%B0%B0%EC%B9%B5-%EB%B9%9B%EC%9D%84-%EB%94%94%EC%A7%80%ED%84%B8-%EC%9D%B4%EB%AF%B8%EC%A7%80%EB%A1%9C-%EB%A7%8C%EB%93%9C%EB%8A%94-cmos-%EC%9D%B4%EB%AF%B8%EC%A7%80%EC%84%BC%EC%84%9Ccis)

렌즈를 통해 들어오는 빛을 전기적인 영상 신호로 바꿔 주는 `CMOS 이미지센서(CIS)`, 이미지 신호를 처리하는 이미지 신호 처리 장치(ISP), 데이터 처리속도를 빠르게 해주는 `D램`, 사진파일을 저장하는 `낸드플래시` 등 많은 반도체가 우리 카메라에서 중요한 역할을 한다.

### 카메라의 필름역할을 하는 '이미지 센서'

PIXEL stack:

- Microlens
- Color Filter
- Photodiode
- Readout Circuitry

디지털 카메라에는 여러 반도체 기술이 필요하지만, 가장 중요한 역할을 하는 부품이 바로 '이미지센서'이다. 이미지센서는 피사체 정보를 읽어 전기적인 영상신호로 변환해주는 장치이다. 즉 빛에너지를 전기적 에너지로 변환해 영상으로 만드는데 카메라의 필름과 같은 역할을 한다.

쉽게 말해 렌즈를 통해 들어온 빛을 전기적 디지털 신호로 변환해주는 역할로, 영상신호를 저장하고 전송해 디스플레이 장치로 촬영한 사진을 볼 수 있도록 만들어주는 반도체이다.
이러한 이미지센서가 사용되는 디지털카메라는 일반 필름카메라와 달리 필름, 인화 과정을 필요로 하지 않는다. 사진을 찍은 후 바로 디스플레이 화면에서 사진을 확인하거나 삭제할 수 있는 것이다.

이미지센서는 응용 방식과 제조 공정에 따라 CCD 이미지센서와 CMOS 이미지센서로 나눌 수 있다. CCD 이미지센서는 전자 형태의 신호를 직접 전송하는 방식으로 아날로그 제조 공정이 사용된다.

CMOS 이미지센서 대비 노이즈가 적다는 장점을 가지고 있다. 반면 CMOS 이미지센서는 신호를 전압 형태로 변환해 전송하는 방식으로 CMOS 제조 공정이 사용되어 가격경쟁력이 있다는 장점이 있다.

이러한 특징 때문에 과거 CCD 이미지센서는 디지털카메라에 사용되고, CMOS 이미지센서는 주로 휴대폰에 사용됐다.

하지만 휴대폰, 태블릿 PC 등 카메라 기능이 탑재된 모바일 기기의 시장이 확대되면서 핵심 부품으로 CMOS 이미지센서가 주목받기 시작했다. 특히 모바일 기기는 전력 소비를 줄이는 것이 중요한데 이는 배터리 사용 시간 연장과 직결되기 때문이다. 때문에 CCD 이미지센서 대비 저전력 공정으로 소비전력에서 강점을 가지는 CMOS 이미지센서가 핵심 부품으로 떠오르게 된 것이다.

또한, CMOS 이미지센서는 노이즈 등 성능이 매우 개선되고 동영상 지원 기능과 가격 측면에서도 지속적인 우위를 가지면서 최근에는 DSLR 카메라에도 많이 탑재되고 있다.

이외에도 CMOS 이미지센서는 CCD 이미지센서에 비해 많은 장점을 가지고 있는데, 일반 반도체 공정인 CMOS 공정을 사용하기 때문에 가격 경쟁력을 가지며 이미지센서와 주변 회로를 원칩화할 수 있어 소형화 및 관리가 용이하다.

### CMOS 이미지센서의 혁신, ISOCELL

일반적으로 이미지의 화질은 센서를 구성하는 각 픽셀(Pixel, 화소)에 모이는 빛의 양에 많은 영향을 받는다. 최근 CMOS 이미지센서의 칩 크기는 작아지고 픽셀 수는 늘어나 픽셀의 크기가 계속 작아지고 있다. 작은 픽셀일수록 충분한 빛을 흡수하기 어려워 CMOS 이미지센서 기술은 '수광율(이미지센서가 빛을 받아들이는 정도)'을 높이는 방향으로 발전해 왔다.

그래서 기존 후면조사형(BSI) 센서는 '수광부(빛이 받아들이는 부분)'를 센서의 가장 윗부분으로 옮겨 '수고아율'을 높여왔다. 하지만 픽셀의 크기가 계속 작아짐에 따라 최근 한계에 봉착했다.

삼성전자는 업계 최초로 기존 CMOS 이미지센서의 성능 한계를 극복한 '아이소셀(ISOCELL)' 개발에 성공했다.

### [광학계 - 롤링 셔터와 글로벌 셔터의 비교](https://codingsuru0525.tistory.com/32)

카메라에는 ccd와 cmos와 같은 디지털 이미지 센서가 들어가는데, 이러한 센서의 중요한 기능은 셔터 유형이다.

일반적으로 ccd 센서는 글로벌 셔터만, cmos 센서는 롤링셔터/글로벌셔터 둘 다 선택가능하지만 롤링셔터가 주를 이룬다.

(cmos 센서가 글로벌 셔터 방식을 구현하기 위해서는 각 픽셀마다 캐퍼시터 등이 추가되어야 해서 회로가 복잡해지고 비용 또한 증가)

이러한 셔터는 카메라의 이미지센서가 이미지를 스캔하는 방식을 말하며 유형에는 글로벌 셔터와 롤링 셔터 두 가지가 있다.

### 1. 롤링 셔터 (rolling shutter)

롤링셔터는 모든 열이 동시에 노출이 시작되는 글로벌 셔터와는 달리, 순차적으로 노출 및 readout (입사된 빛의 강도만큼 신호레벨 출력)을 진행한다.
즉, 첫번째 행의 reset(픽셀에 있는 전하는 비워냄)이 끝나면 그 다음 행의 reset을 시작한다.

cmos 센서는 reset이 순차적으로 진행되고 readout 이 끝나는 파이프라인과 같은 구조로 자세한 과정은 다음과 같다.

1. 센서의 가장 상단의 행에서부터 포토 다이오드의 축적이 완성되며 신호가 출력되기 전에 전하검출부를 리셋게이트로 리셋한다.
   (각 행의 축적 타이밍의 행의 출력 타이밍에 동기하고 있기 때문에 전하가 축적되는 시간에 동시성이 거의 없다)

2. 신호가 없는 상태인 리셋 레벨이 앰프의 게이트로부터 반응한 전압으로 출력된다.

3. 그 직후에 포토다이오드에서 리딩게이트를 통하여 전하검출부의 신호 전하를 읽어내어 저송을 마친 후 게이트를 닫으면
   전하 검출부의 전압이 포토다이오드에 입사된 빛의 강도만큼 변화하기 때문에 신호레벨이 전압으로 출력된다.

이러한 롤링셔터는 수광부 외에 별도의 회로가 없기 때문에 센서 구조가 단순하고 노이즈가 적다.

장점

- 소비전력이 낮고 저발열인 cmos 센서에 주로 사용되어 디지털 장비에 적용하기가 쉽다
- 제조 단가가 낮으므로 대형 센서 제작에 유리하다
- 낮은 노이즈
- 넓은 동적 범위 (실제 사물과 유사한 이미지)

단점

- 스트로브 조명, 움직이는 물체 등의 촬영에서 이미지 왜곡 발생
  (비선형스캔 방식의 부작용으로 젤로 현상, 워블링 현상, 얼라이어싱 현상, 부분 노출 불량 등이 나타난다)
- 센서 크기와 해상도 클수록 비동기 시간이 늘어나 이미지 왜곡이 더 심해진다
- 밝기 확보를 위한 최소 노출시간 김
- 프레임 rate 느림

### 1.1 글로벌 리셋 (global reset)

롤링셔터 센서에 대부분은 글로벌 리셋 기능이 들어있다.
움직이는 물체를 촬영할 때 영상이 왜곡되는 것을 방지하기 위해 글로벌 셔터처럼 찍을 수 있도록 롤링 셔터를 이용하여 왜곡없이 사용하는 방식이다.

글로벌 리셋은 롤링 셔터 센서의 추가 기능으로 왜곡없는 이미지를 촬영할 수 있지만, 센서의 빛을 받기 시작하는 시점은 동일한 것에 반해 노출이 끝나는 시점이 달라 상하 기준 밝기 편차가 생긴다.

따라서 글로벌 리셋을 적용하면 촬영 시점이 늦춰질수록 노출시간이 길어져 영상 하단으로 갈수록 밝기가 밝아지는 왜곡이 발생한다.

움직이는 물체를 촬상하기 위해 이러한 글로벌 리셋을 적용하는 경우 상하 밝기 편차를 없애는 방법은 다음과 같다.

- 암실에서 스트로브 조명을 사용한다. (순간적으로 일정량의 비 조사, 외부광 차단)
- 조리개를 짧게 순간적으로 열었다 닫는다.

### 2. 글로벌 셔터 (Global shutter)

글로벌 셔터는 ccd와 cmos 모두에 적용될 수 있는 셔터방식으로, 노출 시 모든 픽셀이 동시에 빛에 노출된다.
즉, 모든 픽셀열이 동시에 reset 되고 설정된 노출시간 동안 모든 픽셀은 빛을 받아 전하를 쌓는다.

그리고 모든 픽셀의 노출이 끝나면 첫번째 행에 있는 픽셀부터 readout을 시작한다.
그동안 다른 픽셀들은 전하를 쌓아 놓은 채 보유하고 있다.
첫번째 행의 모든 픽셀의 readout이 끝나면 다음 행에 위치한 픽셀이 readout을 시작한다. 이처럼 각행에 위치한 픽셀들이 순차적으로 readout을 한다.

글로벌 셔터는 축적된 전하를 쌓아놓을 수 있는 별도의 공간이 있어야 하는데 그로 인해 픽셀의 수광부는 줄어들게 된다.

또한 별도 커패시터가 필요하며 구조가 복잡해진다.

장점

- 밝기 확보를 위한 최소 노출시간 짧음
- 움직이는 물체 촬상에 좋음
- 프레임 rate 빠름

단점

- 가격이 비쌈
- cmos 센서에 적용 어려움

## [CFA & Bayer Pattern](https://wikidocs.net/231481)

### CFA (Color Filter Array)

CFA는 디지털 카메라의 이미지 센서 위에 배치되는, 다양한 색상 필터의 배열이다. 이 배열은 센서가 빛의 다른 색상을 감지하도록 도와, 컬러 이미지를 생성하는 데 필요한 색상 정보를 제공한다. CFA는 각 픽셀이 전체 가시광 스펙트럼의 빛을 감지하지 못하므로 필요하며, 대신 픽셀별로 빨간색, 녹색, 또는 파란색 빛만을 감지하게 한다.

### 주요 CFA 유형:

1. Bayer Pattern: 가장 일반적인 CFA 유형으로, Bryce Bayer가 발명했다. 이 패턴은 각각 두 개의 녹색 필터와 하나의 빨간색, 하나의 파란색 필터를 가진 2x2 배열을 사용한다. 녹색이 더 많은 비중이 차지하는 이유는 인간의 눈이 녹색에 더 민감하기 때문이다.

2. X-Trans Pattern: Fujifilm에 의해 개발된 이 패턴은 일반적인 Bayer 배열과는 다른, 더 복잡한 6x6 배열을 사용한다. 이는 더 자연스러운 컬러 재현을 제공하고 모아레 현상을 줄이기 위해 설계되었다.

3. CYGM Pattern: 이 패턴은 전통적인 RGB 대신 시안(Cyan), 황색(Yellow), 녹색(Green), 마젠타(Magenta) 필터를 사용한다. CYGM 배열은 특정 조명 조건에서 더 넓은 색상 범위를 제공할 수 있다.

4. RGBE Pattern: 이 배열은 표준 RGB 필터에 에메랄드 (Emerald) 필터를 추가하여, 더 넓은 색상 범위와 더 나은 색상 정확도를 제공한다.

### CFA의 작동 원리:

디지털 카메라의 센서는 원래 흑백 이미지만을 캡쳐할 수 있다. CFA를 사용함으로써, 각 픽셀은 특정 색상의 빛만을 감지하게 된다. 예를 들어, 특정 픽셀이 빨간색 필터 아래에 있으면 그 픽셀은 빨간색 빛만을 감지하게 된다. 이미지 처리 소프트웨어(디모자이킹)는 이렇게 얻은 원시 이미지 데이터를 사용하여 주변 픽셀의 색상 정보를 기반으로 각 픽셀의 실제 색상을 추정하고, 최종적으로 컬러 이미지를 생성한다.

### CFA의 중요성:

CFA는 디지털 이미지의 컬러 재현에 있어 핵심적인 역할을 한다. 다양한 CFA 디자인은 색상 정확도, 이미지 선명도, 그리고 노이즈 수준에 영향을 미친다. 따라서, 카메라 제조업체들은 자신의 제품에 가장 적합한 CFA 유형과 배열을 선택하여 최상의 이미지 품질을 달성하려 한다.

### Bayer Pattern

Bayer Pattern은 디지털 이미징에서 널리 사용되는 컬러 필터 배열(CFA)이다. 1976년에 Bryce Bayer가 발명했으며, 대부분의 컬러 디지털 카메라와 이미지 센서에서 사용된다. 이 패턴은 각 픽셀이 단일 색상의 빛만을 감지할 수 있는 이미지 센서에서 컬러 이미지를 생성하기 위해 설계되었다.

### Bayer Pattern의 구조:

- 패턴 배열: Bayer Pattern은 2x2 격자로 구성된다. 이 격자는 보통 두 개의 녹색(G), 한 개의 빨간색(R), 한 개의 파란색(B) 필터로 구성된다. 이 배열은 인간의 눈이 녹색에 더 민감하기 때문에 녹색 필터를 더 많이 사용한다.

- 배치: 가장 흔한 배치는 'RG/GB'이다. 여기서 첫 번째 행은 빨간색, 녹색 필터로 번갈아 가며 구성되고, 두 번째 행은 녹색, 파란색 필터로 번갈아 가며 구성된다.

### Bayer Pattern의 종류

1. 표준 Bayer Pattern (BG/GR 또는 RG/GB): 가장 일반적인 유형의 Bayer Pattern으로, 이 배열은 2x2 격자에서 두 개의 녹색, 하나의 빨간색, 하나의 파란색 필터로 구성된다. 표준 배열은 RG/GB 또는 BG/GR이 될 수 있다. 이는 인간의 눈이 녹색에 가장 민감하기 때문에 녹색 필터가 더 많이 사용된다.

2. GR/BG 또는 GB/RG 패턴: 이들은 표준 Bayer 배열의 변형이며, 녹색 필터와 빨간색 또는 파란색 필터의 위치가 다르다. 이러한 변형은 특정 센서 설계나 이미지 처리 요구 사항에 따라 선택될 수 있다.

### Bayer Pattern의 작동 방식:

- 컬러 감지: 각 픽셀은 R,G,B 중 하나의 색상만을 감지한다. 따라서 각 픽셀은 전체 컬러 정보를 가지고 있지 않다.

- 디모자이킹(Demosaicing): 이미지 센서에서 얻은 원시 데이터는 각 픽셀 주위의 다른 색상 필터에서 얻은 정보를 사용하여 전체 컬러 이미지로 변환된다. 이 과정에서 각 픽셀에 대해 누락된 색상 정보를 추정하여 전체 색상 이미지를 생성한다.

### Bayer Pattern의 장점 및 사용:

- 장점: 간단하고 비용 효율적인 방법으로 컬러 이미지를 생성할 수 있다. 또한, 인간의 시각 시스템과 유사한 방식으로 더 많은 녹색 정보를 캡쳐하여 더 자연스러운 색감을 제공한다.

- 사용: 대부분의 디지털 카메라 및 스마트폰 카메라에서 Bayer Pattern을 사용한다. 이는 고품질의 컬러 이미지를 생성하는 표준 방법으로 자리 잡았다.

Bayer Pattern을 이해하는 것은 디지털 이미지 처리, 특히 원시 이미지 데이터를 처리하고 컬러 이미지를 재구성하는 방법을 배우는 데 중요하다.

### RGB in Cameras - Bayer Pattern

- 25% pixels see Red
- 25% pixels see Blue
- 50% pixels see Green

### Spectral Power Distribution (SPD)

- Most types of light "contain" more than one wavelengths.

- We can describe light based on the distribution of power over different wavelengths.

### [Spectral Power Distribution, SPD](https://blog.naver.com/framkang/220410939899)

IESNA Definition: A pictorial representation of the radiant power emitted by a light source at each wavelength or band of wavelengths in the visible region of the electromagnetic spectrum (360 to 770 nanometers).

Lamp manufacturers publish SPD curves of specific light sources. The spectral make-up of a light source affects its ability to render colors "naturally", as seen in the following examples:

### [카메라 촬영 기법 - 화이트 밸런스 (White balance)](https://kalchi09.tistory.com/entry/%EC%B9%B4%EB%A9%94%EB%9D%BC-%EC%B4%AC%EC%98%81-%EA%B8%B0%EB%B2%95-%ED%99%94%EC%9D%B4%ED%8A%B8-%EB%B0%B8%EB%9F%B0%EC%8A%A4white-balance)

흰색 A4 종이를 한장 들고 붉은 조명이 있는 정육점에 가면, 맑은날 밖에서 보았을 때와 비교하였을 때 다소 붉게 보일 것이다. 마찬가지로 청색 인공조명이 있는 곳에서 흰색 A4용지를 보면 다소 푸르게 보일 것이고, 빨간 노을 아래에서는 대부분의 사물이 평소보다 붉게 보인다. 결국 외부 광원(조명)의 형태에 의해 흰색 종이의 색이 흰색으로 보이지 않고 달라져 보이게 되며, 사진 촬영을 하였을 때에는 이것이 그대로 반영되어 촬영된다.

> 이런 이유로 카메라에는 외부 광원에 의해 본래의 흰색이 흰색처럼 보이지 않을 때 이를 보정해 줄 수 있도록 하였다. 이것이 화이트 밸런스(white balance)이다.

화이트 밸런스를 조절하는 메뉴에 가면 백색광, 그늘, 태양 등과 같은 모드가 있고 옆에 (4000K)와 같은 형태로 온도가 표시되어 있다. 또한 AWB (auto white balance) 기능과 색온도라는 기능도 있다. (카메라 메뉴에는 커스텀 화이트 밸런스라는 기능 또한 따로 있다.)

여기서 온도는 색온도 (color temperature)이다.
색온도라는 것은 흑체의 온도에 따라 최대 에너지가 방출되는 빛의 색이 다르다는 것에 기인한다. 플랑크 곡선 등등 알아야 하는 것이 많다만, 여기서 설명하고자 하는 것은 이것이 아니기 때문에 이에 대한 것은 결론만 논하고자 한다.

> 결론은 온도가 낮을수록 붉은색을 띄고, 온도가 높을수록 푸른색을 띈다.

별(star)도 마찬가지이다. 온도가 낮은 별 (4000K)은 붉은색, 태양정도 온도의 별 (6000K)은 주황색계열, 10,000K의 표면온도가 나타나는 별은 백색, 이보다 온도가 높으면 청색을 띈다.

이렇게 흑체의 온도에 따라 색이 달라지는 것을 이용하여, 빛의 색을 온도로 대응시킬 수 있다. (천문학에서 이와 유사하게 색지수라는 것을 사용한다.)

그런데 인공조명, 예를 들어 청색 램프의 경우 램프의 온도가 1만K가 넘어가기 때문에 청색을 띄는 건 아니다. 청색 램프는 흑체가 아니다. 다시 말해 스스로 빛을 내는 것이 아니라 전기를 통해 에너지를 공급받고 빛을 내는 도구일 뿐이다. 때문에 흑체에 적용할 수 있는 색온도를 여기에 그대로 적용할 수 없다. 그래서 `상관색온도(Correlated Color Temperature, CCT)`라는 개념이 도입된다.

흑체에서의 색온도를 조명에 대응시키기 위해 색온도의 근사값(또는 보정값)인 상관색온도를 사용하게 된다.

조명의 색이 붉은색일수록 상관색온도는 낮아지고, 조명의 색이 푸른색일수록 상관색 온돈느 높은 값을 가진다.

---

푸른 색 조명에서 촬영된 사진은 원래 사물이 가진 색보다 푸른색을 띄게 되어, 원래의 색이 왜곡된다. 따라서 카메라의 바디는 여기에 인위적으로 붉은 색을 좀 넣어주어서 원래 색과 거의 같도록 색 보정을 해 주는 것이 화이트 밸런스의 기능이다. 마찬가지로 붉은색 조명에서 촬영된 사진은 원래보다 사물이 붉게 보이고, 여기에 푸른색을 더하여 원래 색과 비슷하게 만들어진다.

정리하자면, 카메라의 화이트 밸런스기능은 현재 어떤 조명 아래에서 촬영하고 있는지를 카메라에게 알려주는 것이다.
화이트밸런스 메뉴에서 색온도를 선택하여

1. 색온도의 온도를 높이면, 현재 푸른색이 강한 조명에서 촬영하고 있으니 사진에 붉은기를 더하라는 소리이고,
2. 색온도의 온도를 낮추면, 현재 붉은색이 강한 조명에서 촬영하고 있으니 사진에 푸른기를 더하라는 소리가 된다.

위 사진은 약간의 노란색이 나타나는 LED 조명 아래서 촬영하였다. 앞서 이야기한 것처럼, 2500에서 푸른색이 가장 나타나고, 숫자가 증가할수록 붉은색이 더해지는 것을 확인할 수 있다.

화이트밸런스 본연의 기능을 잘 생각해보면, 흰색 인형이 가장 흰색처럼 표현된 사진은 3500으로 놓고 촬영한 사진이다. 이는 앞서 이야기 하였듯, 약간의 노란색 LED 조명에서 촬영하였다고 하였는데, 조명의 색온도가 3500K라는 소리이기도 하다.

정리 하자면...

1. 화이트밸런스는 카메라에게 현재 촬영하고 있는 조명(자연광이든 인공광이든)의 색을 알려주는 것이다.
2. 색온도가 낮아지면 사진은 푸르게 찍히고, 색온도가 높아지면 사진은 붉게 찍힌다.

### White balancing

Human visual system has chromatic adaptation:

- We can perceive white (and other colors) correctly under different light sources.
- Cameras cannot do that (there is no "camera perception").

White balancing: The process of removing color casts so that colors that we would perceive as white are rendered as white in final image.

### [Demosaicing(디모자이킹)](https://melona94.tistory.com/20)

Demosaicing은 color filter array(CFA) interpolation이라고도 한다. 대부분의 이미지 센서는 각 픽셀에서 R,G,B 중 한 가지 색만을 감지한다. 하지만 우리가 보는 실제 영상은 각 화소마다 RGB 값을 모두 갖고 있다. 이를 가능케 하는 기법이 디모자이킹 기법이다.

### Artifacts

일반적으로 디모자이킹 이후에는 edge 영역에서 artifacts가 발생한다. 발생하는 대표적인 artifacts 종류는 다음과 같다.

- Zipper effect

- False color

- Aliasing

- Blurring

> 보간 알고리즘

Non-adaptive interpolation

1. Nearest Neighbor Interpolation: 단순히 가까이에 있는 화소를 출력하는 방식. 가장 간단한 방식이기 때문에 매우 빠르지만, 영상 왜곡이 발생할 가능성이 매우 높다. 보통 Up-scaling에 많이 사용한다.

2. Bilinear Interpolation: 새롭게 만들어지는 R,G,B 성분은 주변의 가장 가까운 화소들에 가중치를 곱한 값으로 할당된다. 가중치는 선형적으로 결정되며 각각의 가중치는 존재하는 화소로부터 거리에 정비례한다. 계산 과정이 단순하며 고속으로 처리가 가능하며 Nearest Neighbor 보간보단 나은 영상을 얻을 수 있다. 하지만 Blurring이 발생한다.

3. Edge-directed Interpolation:
   G value를 보간할 때, Horizontal / Vertical 방향으로 밝기 변화를 구해서 변화 값이 작은 쪽으로 보간하는 방식. Non-adaptive interpolation보단 Edge 영역에 효과적이나 여전히 color artifacts는 발생한다. 활용하는 커널의 사이즈가 클수록 좀 더 좋은 영상을 얻을 수 있는 대신 연산량이 증가한다. R,B value는 R/G, B/G에 대한 값을 보간된 G value에 곱하여 보간한다.

### CFA demosaicing

Produce full RGB image from mosaiced sensor output.

Interpolate from neighbors:

- Bilinear interpolation (needs 4 neighbors)
- Bicubic interpolation (needs more neighbors, may overblur)
- Edge-aware interpolation (more on this later).

### Noise in images

Can be very pronounced in low-light images.

### Denoising Strategy 1: Gaussian or Averaging Filter

Gaussian filtering

Averages out pixel variation but blurs edges/sharp features

### [[OpenCV] 블러링 기법과 가우시안 필터](https://blog.naver.com/sees111/222366804864)

### 블러링 기법

블러링(blurring)이란 마치 초점이 맞지 않은 사진처럼 영상을 부드럽게 만드는 필터링 기법이다. 스무딩(smoothing)이라고도 한다.
영상에서 인접한 픽셀 간의 픽셀 값 변화가 크지 않은 경우 부드러운 느낌을 받을 수 있다.

### 평균값 필터

블러링 필터 중 단순하고 구현하기 쉬운 `평균값 필터(mean filter)`가 있다.
평균값 필터란 입력 영상에서 특정 픽셀과 주변 픽셀들의 산술 평균을 결과 영상 픽셀 값에 설정하는 필터이다.
평균값 필터에 의해 생성되는 결과 영상은 픽셀 값의 변화가 줄어들고, 날카로운 에지가 무뎌지며 노이즈의 영향이 크게 사라지는 효과가 있다. 그러나 평균값 필터를 너무 과도하게 사용하면 사물의 경계가 흐릿해지며 구분이 어려워진다.

평균값 필터 마스크는 마스크의 크기가 커지면 커질수록 더욱 부드러운 느낌의 결과 영상을 생성할 수 있다.
대신 연산량이 크게 증가할 수도 있다.

3 x 3 크기의 평균값 필터 마스크는 9개의 원소 개수로, 각각의 행렬은 모든 원소가 1/9로 설정된 행렬이다.

5 x 5 크기의 평균값 필터 마스크는 25개의 원소 개수로, 각각의 행렬은 모든 원소가 1/25로 구성된 행렬이다.

### 가우시안 필터

가우시안 필터(Gaussian filter)란 가우시안 분포(Gaussian distribution) 함수를 근사하여 생성한 필터 마스크를 사용하는 필터링 기법이다.
가우시안 분포는 평균을 중심으로 좌우 대칭의 종 모양을 갖는 확률 분포를 말한다. 정규 분포(normal distribution)이라고도 한다.

가우시안 분포는 평균과 표준 편차에 따라 분포 모양이 결정된다. 다만 영상의 가우시안 필터에서는 주로 평균이 0인 가우시안 분포 함수를 사용한다.

가우시안 분포를 따르는 2차원 필터 마스크 행렬을 생성하려면 2차원 가우시안 분포 함수를 근사해야 한다.
2차원 가우시안 분포 함수는 x,y 2개의 변수를 사용하고, 분포의 모양을 결정하는 평균과 표준 편차도 x,y 축 방향에 따라 따로 설정한다.

가우시안 필터 마스크 행렬은 중앙부에서 비교적 큰 값을 가지고, 사이드로 갈수록 행렬 원소 값이 0에 가까운 작은 값을 가진다.
이런 필터 마스크를 이용하여 마스크 연산을 수행한다는 것은 필터링 대상 픽셀 근처에서 가중치를 크게 주고, 필터링 대상 픽셀과 멀리 떨어져 있는 주변부에는 가중치를 조금만 주어서 가중 평균을 구하는 것과 같다.
즉, 가우시안 필터 마스크가 가중 평균을 구하기 위한 가중치 행렬 역할을 하는 것이다.

OpenCV에서 가우시안 필터링을 수행하기 위해 GaussianBlur() 함수를 제공한다.

### GAUSSIAN FILTER

### 정의

잡음을 제거하여 이미지를 부드럽게 해주고 픽셀들 마다 가중치를 두어 중앙에 위치한 픽셀이 가중치가 높고 중앙 픽셀에서 멀어질수록 가중치를 약하게 두는 필터이다.

### 목적

- Guassian Filter는 이미지의 잡음을 감소시키거나 부드러운 효과를 주는 데에 활용된다.

- 주변 픽셀들과의 가중 평균을 계산하여 픽셀 값을 할당하는 smoothing 효과가 있다.

- 필터의 인덱스 별로 가중치가 다르다. 필터의 중앙으로 갈수록 가중치를 높이는 방식으로 필터의 가장자리로 갈수록 가중치를 감쇄시켜 주변의 잡음의 영향을 줄이는 필터이다.

- noise 감소: 이미지의 잡음을 감소시키는 데에 효과적이다. 잡음이 고주파 성분으로 간주되고, 고주파 성분을 부드럽게 흐리게 만들어 잡음을 제거하는 효과를 줄 수 있다.

평균 필터와 가우시안 필터를 비교해보면 차이가 있음을 확인할 수 있다.
! 그 이유는 필터의 가중치 차이이다. 가우시안 필터는 필터의 중앙으로 갈수록 가중치를 높게 두지만, 평균 필터는 모든 인덱스가 다 같은 값을 가지므로 가중치를 두는 가우시안 필터가 미세하지만 비교적 평균 필터보다 선명함을 가질 수 있다.

### [Smoothing](https://hanstar4.tistory.com/24)

이미지에 흐림효과를 주어 번져보이게 하는 함수이다.
주로 이미지 안에서 노이즈를 줄이거나 경계선을 흐리게 할 때 사용한다.

Smoothing filter에 따라 결과가 달라진다. 주로 많이 사용하는 4가지 filter에 대해 소개하겠다.

1. Averaging Filter
   Averaging 방법은 주변 픽셀값들의 평균을 취하는 형태이다.
   아래와 같은 filter를 곱해 새로운 픽셀 값을 도출한다.

|1/9|1/9|1/9|
|1/9|1/9|1/9|
|1/9|1/9|1/9|

2. Gaussian Filter
   Gaussian filter는 가운데 부분의 값을 좀 더 큰 값으로 할당하여 중심 값을 보존하면서 주변의 값에 대한 평균을 취하는 형태이다.
   아래와 같이 Gaussian 분포를 가지는 filter를 곱해 새로운 픽셀 값을 도출한다.

|1/16|1/8|1/16|
|1/8|1/4|1/8|
|1/16|1/8|1/16|

3. Median Filter
   Median 방법은 filter 안에 있는 픽셀 값들을 정렬 한 뒤 중간 값을 새로운 픽셀 값으로 도출한다.
   이미지 내에 존재하는 값을 이용하여 결과를 얻는 대표적인 비선형 filter이다.
   특히, salt-and-pepper noise를 가지고 있는 경우 다른 필터링 보다 우수한 결과를 보인다.

4. Bilateral Filter

앞선 세가지 방법은 이미지 전체에 동일한 필터를 적용시켜 흐리게 만들기 때문에, 물체의 경계선까지 흐려지게 된다.
반면에, Bilateral filter는 Gaussian filter의 단점을 보완하고자 중심 픽셀에서의 거리에 따른 가중치뿐 아니라 픽셀 값 차이도 가중치로 둔 필터이다.

중심 픽셀과 비슷한 값을 가지고 있는 픽셀 값에만 가중치를 주어 계산하고 값 차이가 큰 픽셀들은 계산에서 제거된다.

예를 들어 경계부근의 왼쪽 픽셀은 값 차이가 큰 경계 너머 오른쪽 픽셀 값의 영향을 거의 받지 않는다. 그렇기 때문에 경계가 그대로 보존된 채 나머지 영역만 흐려지게 된다.

### [OpenCV의 다양한 필터](https://overface.tistory.com/601)

### 평균 값 필터(Mean filter)

영상의 특정 좌표 값을 주변 픽셀 값들의 산술 평균으로 설정
픽셀들 간의 그레이스케일 값 변화가 줄어들어 날카로운 에지가 무뎌지고, 영상에 있는 잡음의 영향이 사라지는 효과

### 가우시안 필터

평균값 필터에 의한 블러링의 단점.
필터링 대상 위치에서 가까이 있는 픽셀과 멀리 있는 픽셀이 모두 같은 가중치를 사용하여 평균을 계산 멀리 있는 픽셀의 영향을 많이 받을 수 있다.

### 언샤프 마스크(Unsharp mask) 필터링

날카롭지 않은(unsharp) 영상, 즉, 부드러워진 영상을 이용하여 날카로운 영상을 생성

### 영상의 잡음(Noise)

영상의 픽셀값에 추가되는 원치 않는 형태의 신호

f(x,y) = s(x,y) + n(x,y)

잡음의 종의 종류: 가우시안 잡음(Gaussian noise), 소금&후추 잡음(Salt&Pepper)

### 잡음 제거(1): 미디언 필터

주변 픽셀들의 값들을 정렬하여 그 중앙값(median)으로 픽셀 값을 대체 소금-후추 잡음 제거에 효과적이다.

### 잡음 제거(2): 가우시안 필터

가우시안 잡음 제거에는 가우시안 필터가 효과적이다.

### 잡음 제거(3): 양방향 필터

- 양방향 필터(Bilateral filter)
  `엣지 보존 잡음 제거 필터(edge-preserving noise removal filter)`의 하나로 평균 값 필터 또는 가우시안 필터는 에지 부근에서도 픽셀 값을 평탄하게 만드는 단점이 있음. 기준 픽셀과 이웃 픽셀과의 거리, 그리고 픽셀 값의 차이를 함께 고려하여 블러링 정도를 조절한다.

(일반적인) 가우시안 필터링: 영상 전체에서 blurring을 적용한다.

양방향 필터: 엣지가 아닌 부분에서만 blurring을 적용한다.

### [Non local means 알고리즘, MATLAB 구현 코드 포함](https://bellzero.tistory.com/25)

Non local means algorithm은 노이즈 제거에 강력한 성능을 보여주는 denoising 알고리즘이다.

일반적으로, gaussian smoothing과 같은 노이즈 제거 알고리즘을 많이 사용하는데 이러한 smoothing 방식은 local한 데이터들, 즉 해당 픽셀 주변의 정보들만을 이용한다는 데에 그 한계가 있다. 그렇기 때문에 edge 등이 소실되기 마련이고, 텍스쳐가 뭉개지는 등의 단점이 있다. 이를 통해 노이즈가 제거되기도 하지만, 영상의 디테일 또한 소실되는 문제점이 있다.

NL means 알고리즘은 다음과 같은 아이디어에서 시작된다.

> > 해당 픽셀의 gaussian kernel에 해당하는 local 영역의 픽셀값 대신, 영상 내에서 해당 픽셀 주변 영역과 비슷한 패턴을 갖는 영역들을 찾아내 찾아낸 영역들에 대해서만 평균을 취하면 텍스쳐 또한 살리면서 노이즈 제거가 가능하지 않을까?

### [[매트랩] 이미지 non local means filter 구현하기](https://blog.naver.com/won19600/221776570748)

non local means filter, 줄여서 nlm filter를 탈피한 필터 방식이다.

mean filter은 자신의 픽셀값을 주변 픽셀 기준으로 구하는 방식을 말한다. 대표적으로는 가우시안 필터가 있다.

하지만 nlm 필터는 자신 주변의 픽셀만 비교하지 않는다.

자신과 비슷하게 생긴 마스크를 찾아 탐색한 후, 비슷하게 생긴 마스크가 있다면 그것을 모두 더해서 나누기 n 한다.

가우시안 노이즈는 (같은 이미지일 경우에) 많이 더하고 n빵할수록 노이즈가 줄어든다는 특성을 이용한 것이다.

### [[Computer Vision] 블록 매칭 및 3D 필터링 (Block-Matching and 3D filtering, BM3D)](https://goatlab.tistory.com/entry/%EB%B8%94%EB%A1%9D-%EB%A7%A4%EC%B9%AD-%EB%B0%8F-3D-%ED%95%84%ED%84%B0%EB%A7%81-Block-matching-and-3D-filtering-BM3D)

### 블록 매칭 및 3D 필터링 (Block-Matching and 3D filtering, BM3D)

블록 일치 및 3D 필터링(BM3D)은 주로 이미지의 noise 감소에 사용되는 3D 블록 일치 알고리즘이다.
non-local means methodology의 확장 중 하나이다. BM3D에는 hard-thresholding 및 Wiener filter 단계와 둘 다 그룹화 (grouping), 협업 필터링 (collaborative filtering) 및 집계 (aggregation) 부분을 포함한다. 이 알고리즘은 변환 사이트의 증강 표현에 따라 다르다.

1. Grouping

이미지 조각은 유사성을 기반으로 함께 grouping되지만 표준 k-means 클러스터링 및 클러스터 분석 방법과 달리 이미지 조각이 반드시 분리되지 않는다. 이 블록 일치 알고리즘은 계산이 덜 요구되며 나중에 aggregation 단계에서 유용하다. 그러나 조각의 크기는 동일하다. reference fragment 과의 비유사도가 지정된 임계값 아래로 떨어지면 단편이 grouping 된다. 이 grouping 기술을 블록 일치라고 하며 일반적으로 디지털 비디오의 서로 다른 프레임에 걸쳐 유사한 그룹을 grouping 하는 데 사용된다. 반면에 BM3D는 단일 프레임 내에서 매크로블록을 grouping 할 수 있다. 그런 다음 그룹의 모든 이미지 조각이 쌓여서 3D 실린더와 같은 모양을 형성한다.

2. Collaborative filtering

filtering은 모두 조각 그룹에서 수행된다. d+1 차원 선형 변환이 적용된 다음 Wiener filtering과 같은 변환 영역 축소가 뒤따른 다음 선형 변환을 반전하여 모든 (필터링된) 조각을 재생한다.

3. Aggregation

이미지는 다시 2차원 형태로 변환된다. 모든 겹치는 이미지 조각은 noise에 대해 filtering 되지만 고유한 신호는 유지하도록 가중치 평균을 낸다.

## Tone reproduction

- Also known as gamma correction.

- Without tone reproduction, images look very dark.

We have already seen that sensor response is linear.

Human-eye response (measures brightness) is also linear.

However, human-eye perception (perceived brightness) is non-linear:

- More sensitive to dark tones.
- Approximately a Gamma function.

Displays have a response opposite to that of human perception.

- Because of mismatch in displays and human eye perception, images look very dark.

- Pre-emptively cancel-out the display response curve.
- Add inverse display transform here.
- This transform is the tone reproduction or gamma correction.

### Photodiode response function

For silicon photodiodes, usually linear but:

- non-linear when potential well is saturated (over-exposure)

- non-linear near zero (due to noise)

### [ND 필터 총정리 (Neutral Density Filter) +++](https://m.blog.naver.com/dadamum/222651625736)

> ND 필터란?

ND 필터는 렌즈에서 들어오는 빛의 양을 줄이기 위해 사용하는 회색 필터다. 눈부심을 억제하는 선글라스 같은 이치이다.

ND란, Neutral Density (뉴트럴 덴시티)의 약어로, 직역하면 중립적인 농도, 중간 농도라고 하는 의미이다.

발색에 영향을 주지 않고, 광량을 줄여주는 것이 ND 필터이다.

> ND 필터의 효과와 선택 방법

ND 필터는, 사진의 발색에 영향을 주지 않고, 광량만을 줄이는 효과가 있다. 광량을 1/4로 하는 ND 4, 1/8로 하는 ND 8 등, 감광의 정도에 따라 숫자가 커진다. ND 필터는 효과가 높인 것일수록 색이 짙다. 어느 정도의 효과를 필요로 하는지 목적에 따라 골라야 한다.
즉 번호가 클수록 진해져, 셔터 속도를 더 늦출 수 있다.

ND4, ND8과 같은 숫자는 광량을 몇 분의 1로 줄이는지를 나타낸다. (예: ND4는 광량 1/4).
또 ND 필터의 패키지에는 "3 조리개분 감광"과 같은 글이 기재되어 있는데, 이것은 ND 필터의 감광량이 어느 정도인가를 나타내 주고 있다.
1단 조임의 감광은 광량이 반 (50%)이 된다고 하는 것이며 2단 조임은 반에 반 (25%), 3단 조임은 반에 반에 반 (12.5%)이 된다.

> ND 필터 활용방법

밝은 장소에서 개방으로 찍는다. 밝은 환경 속에서 조리개를 개방으로 해 촬영하려고 하면, 셔터 버튼이 눌리지 않거나, 너무 밝게 찍히거나 하는 일이 생긴다. 카메라의 최고 셔터 스피드에서도 찍을 수 없을 만큼 피사체가 너무 밝다는 것이다. 이러한 때에 ND 필터로 빛의 양을 줄여 찍을 수 있다. 여름의 맑은 날씨나 화창한 날의 설경 등에 활용할 수 있다.

스틸사진의 장시간 노출이나 동영상에서는 셔터 스피드를 1/60s에 고정해 찍고 있기 때문에, 카메라 라이프에는 필수라고 해도 좋을 정도의 아이템이다.

> ND 필터의 용도

ND 필터는 카메라에 들어오는 빛을 억제하는 역할을 하는 필터다.
폭포의 흐름(물의 흐름)을 밝은 환경에서 ND 필터를 사용하여 폭포의 흐름이나 물의 흐름을 매끄럽게 보여주는 방법이다. 한낮의 밝은 시간대에 ND 필터를 사용함으로서 셔터 스피드를 느리게 촬영하는 것이 가능하다.

예를 들면, 셔터 스피드를 낮의 밝은 시간대에 1초로 찍으려고 생각해서, 조리개 F치를 올리고, ISO 감도를 100 정도의 저감도로 하더라도, 빛의 양이 너무 많아 하얗게 날아가 버리게 된다. 그럴 때에, ND 필터를 사용하고, 들어오는 빛의 양을 조정해 주는 것이다.

그리고 길거리에서 사람을 사라지게 하고 싶을 때에도 사용한다.

[[개념 정리] HDR: High Dynamic Range](https://xoft.tistory.com/22)

우리가 보고 있는 디지털 영상들은 얼마나 현실 세상의 밝기를 표현하고 있을까?
현실 세계의 장면(Scene)은 카메라 센서로 획득하여 이미지/영상으로 저장된 후, 디스플레이로 볼 수 있다. 본 글에서는 현실에서 보는 것과 비슷한 밝기를 표현하는 기술인 HDR (High Dynamic Range)에 대해 디스플레이 영역에서의 HDR과 이미지 영역에서의 HDR을 구분해서 다뤄보고자 한다.

### Dynamic Range

`Dynamic Range`란 가장 밝고 가장 어두운 밝기 간의 ratio(비율)이며, 의역하면 명암비로 볼 수 있다.
밝기는 1cd/m^2 또는 1nit로 표현하며, 이는 1m^2당 촛불 한 개의 밝기 (candela=cd)를 의미한다. 100nit는 1m^2당 촛불 100개의 밝기로 보면 된다. 가장 어두운 경우 0이며, 밝을 수록 값이 커진다.

명암비는 영어로 Contrast Ratio 단어가 따로 있다. 디스플레이 분야에서 주로 사용되는 용어이며, Dynamic Range는 Contrast Ratio와 동일 의미이다. 가장 어둡고 가장 밝은 구간을 얼마나 세밀하게 표현하는가를 나타낸다.
가장 왼쪽 밝기가 0.1 nit 이고, 가장 오른쪽 밝기가 10nit 이면, Dynamic Range는 100 이다.

### HDR: High Dynamic Range

단어 자체로는 현실의 명암비(Dynamic Range)를 디지털로 가능한 가장 가깝게 표현한 고명암비(High Dynamic Range)를 의미한다. 다른 의미로 현실의 밝기를 높은 명암비로 구현하는 기술을 의미하기도 한다.

### 디스플레이 분야에서의 HDR

사람은 10^-6 ~ 10^8 nits를 인지할 수 있는 반면, 일반 디스플레이는 0.1~100nits를 표시할 수 있다. 이 수치는 방송 표준들이 제정되던 시기에 밝기에 대한 표준값이며, SDR(Standard Dynamic Range)라고 부른다.

SDR은 0.1~100nit를 표현할 수 있기에, 명암비(Dynamic Range)는 1000이다. 반면 HDR은 이를 더 큰 명암비인 100,000 이상을 낸다. SDR, HDR, HDR10, HDR10+, Dolby Vision 이라는 용어가 많이 언급된다.

`SDR`은 표준 명암비를 의미한다.
`HDR`은 고명암비를 의미하며,
`HDR`은 밝은 부분은 더 밝게, 어두운 부분은 더 어둡게 보여주는 기술을 의미하기도 한다.

`HDR10`은 색상, 밝기, 명암 정보가 담긴 메타데이터를 가진 영상 규격(또는 기술)을 의미한다. 한 개 영상에 고정된 메타데이터를 가진다. 최소 0.05~1,000nits(명암비 200,000 이상) 또는 0.0005~540nits(명암비 1,080,000 이상)을 표현하면 HDR10 인증을 받을 수 있다. RGB 각 채널의 색심도(colour depth)가 10bit 이상이 되어야 한다.

`HDR10+`는 한 개 영상에 프레임마다 다른 메타데이터를 적용한 규격(또는 기술)을 의미한다.

[[OpenCV] 이미지 노출 융합(exposure fusion)이란?](https://gmnam.tistory.com/162)

### [OpenCV] 이미지 노출 융합 (exposure fusion) 이란?

"세 장의 노출 시간을 다르게 찍은 사진이 있다고 하자, 첫 번째 사진은 밝은 부분은 잘 나오지만 어두운 부분이 너무 어두워 물체를 분간할 수 없고, 반면 다른 사진은 어두운 부분은 잘 나왔지만 밝은 부분이 너무 밝아 하얗게만 표현되었다. 이 사진들을 이용해 모든 영역이 잘 나오도록 사진을 만들고 싶은데 어떻게 하겠냐"

그 당시에는 이 알고리즘에 대해서 몰랐기에 각 이미지의 같은 지점의 픽셀 값을 interpolation 해서 characteristic curver를 얻어 적절하게 노출된 사진을 얻을 수 있겠다고 온갖 브레인 스토밍으로 대답했었다. 인터뷰 후에 제대로 알고 싶어 찾아보니 노출 융합이었고 카메라에서 굉장히 널리 쓰이는 방법이었다. 소 잃고 외양간 고치는 아픈 심정으로 노출 융합에 대해서 간략하게 정리해 보았다.

`노출 융합(exposure fusion)`은 노출시간이 다른 여러장의 사진을 합쳐서 빛의 동적 범위(dynamic range, DR)가 높은 사진을 얻어내는 방법이다. 요즘 대부분의 스마트폰에서 이 방법을 이용해 최종 사진을 만든다. 우리는 사진을 찍지만 스마트폰은 사진을 '계산'해 내는 것이다. 그래서 이것을 computational photography 라고 한다. 이렇게 만들어 낸 사진은 빛의 밝기 차이가 큰 영역에서 사물의 디테일을 잘 캡쳐할 수 있는 장점이 있다. 즉 역광에서도 배경과 사물 모두 잘 찍을 수 있다.

동적 범위(DR)는 사진상 빛의 가장 밝은 부분과 어두운 부분의 비율을 뜻한다. 이 범위에 따라 사진의 어두운 부분과 밝은 부분이 표현되는 정도가 결정된다. 즉, 동적 범위가 넓을수록 어두운 부분에서 밝은 부분으로 변화하는 정도가 더 세밀하게 표현하여 좋은 사진을 만든다.

왼쪽은 낮은 동적 범위의 사진이고, 오른쪽은 높은 동적 범위의 사진이다. 왼쪽의 사진에서 하늘은 빛이 밝아 하얀색으로만 표현된 반면, 오른쪽 사진에선 구름이 보일 정도로 세밀하게 표현되었다. 이는 동적 범위가 넓어 빛의 밝기가 변하는 단계가 많기 때문이다.

오른쪽과 사진과 같이 동적범위가 높게 찍힌 것을 High Dynamic Range(HDR) 이미지라고 한다. 반면 왼쪽은 Low DR (LDR)이다. LDR은 대개 8비트 이미지로 픽셀의 밝기가 0-255 사이로 표현되는 반면, HDR은 최소 16비트 범위의 확장된 픽셀 값을 가진다.

위의 사진에서 보여지듯이 HDR은 LDR에 비해 세부사항을 잘 잡아낸다. 이 HDR 이미지를 얻기 위해 대부분의 카메라는 노출시간이 다르게 찍은 여러 장의 사진을 합친다.

예를 들어 아래와 같이 4개의 다른 노출 시간으로 찍은 사진이 있다고 하자. 방안에서 창문을 향해 사진을 찍을 때 주로 얻게 되는 사진이다. 노출시간이 짧을수록 창문 밖의 풍경은 잘 담아낼 수 있지만 방안은 빛이 적어 어둡게 나온다. 반면 긴 노출 시간으로 찍으면 방안은 잘 나오지만 빛의 양이 많은 창문은 픽셀 값이 포화되어 하얀색으로만 나온다.

노출시간이 적은 것을 underexposed, 긴 것을 overexposed 되었다고 한다. 적절한 노출시간을 준다고 해도 밝은 부분과 어두운 부분을 동시에 세밀하게 나타난 이미지를 얻는 것은 쉽지 않다. 따라서 이렇게 노출시간을 달리하며 여러 장의 시간을 찍어 이것들을 융합해 이미지의 모든 부분이 세밀하게 표현한 사진을 얻는 것이다.

### Eyes & Camera

- 각막: 안구를 보호. 광선의 초기 초점 형성.
- 홍채: 들어오는 빛의 양 조절.
- 수정체 (Flexible lens): 상을 망막에 맺게 하는 블록 렌즈 역할.
- 모양체 (Ciliary body): 수정체 주위의 근육 조직이며, 수정체의 두께를 수정하는 역할. 모양체가 퇴화되면 근시와 원시가 발생할 수 있음.
- 망막 (Retina): 영상을 감지하는 기관이며, 망막의 상이 맺히면 사물을 볼 수 있음. 원추세포(Cones)와 간상세포(Rods)라는 시세포가 분포.
- 원추세포 (Cone cell): 세 종류의 시색소가 색에 따라 서로 다르게 반응.
- 간상세포 (Rod cell): 빛의 밝기에 민감하지만 색을 잘 구분 못함.
- 중심와 (Fovea): 0 도, 원추세포 (Cones) 가 몰려있고, 간상세포 (Rods) 가 없음.
- 맹점 (Blind spot): 원추세포(Cones)와 간상세포(Rods)가 모두 없음.

### Color

- RGB (red, green, blue) model for color monitors and a broad class of color video cameras.

- CMY (cyan, magenta, yellow) and CMYK (cyan, magenta, yellow, black) models for color printing

- HSI (hue, saturation, intensity) model, which corresponds closely with the way humans describe and interpret color.

- Red, green, and blue images is an 8-bit image. Under these conditions, each RGB color pixel [that is, a triplet of values (R,G,B)] has a depth of 24 bits (3 images planes times the number of bits per plane).

- The term full-color image is used often to denote a 24-bit RGB color image.

- The total number of possible colors in a 24-bit RGB image is (2) = 16, 777, 216.

[(17) 색공간과 색공간 변환 HSV, HSI, HSL](https://m.blog.naver.com/jdancor/222881600118)

- HSV/HSL/HSI

YCbCr과 다르게 컬러를 조금더 직관적으로 표현하는 색공간이다.
YCBCR은 Cb와 Cr로 색을 선택하면서 색이라는 표현 사람 눈에 직관적으로 보기 어려웠다면 HSV/HSL/HSI는 아래 이미지에서 보는 것과 같이 조금 더 보기 편할 거다.

- H (Hue): 색을 표현 0~360의 값을 가지며 0-red, 60-yellow, 120-Green, 240-blue 와 같이 표현이 된다.
  각도로 원으로된 공간에서 색상을 선택하게 된다.

- S (Saturation): 색상의 정도다. 0-흰색으로 색이 없음, 255-색이 있음

- V (Value) / L (Lumninance) / I (Intensity)
  : 색상의 밝기이다. 0-black, 255-white
  밝기를 나타내는 값은 아래의 수식으로 평균 또는 R,G,B의 max를

I = avg(R,G,B) = 1/3(R+G+B)

V = max(R,G,B) = M

L = mid(R,G,B) = 1/2(M+m)

밝기를 나타내는 게 3가지나 있는데 차이에 대해서 이미지를 보며 비교해 보겠다.

### [histogram stretching & equalization](https://velog.io/@from_numpy/%EC%98%81%EC%83%81-%EC%B2%98%EB%A6%AC-4%EC%A3%BC%EC%B0%A8-histogram-stretching-equalization)

### Histogram Analysis (히스토그램 분석)

히스토그램을 이용하면 주어진 영상의 픽셀 밝기 분포를 조사하여 밝기 및 명암비를 조절할 수 있다.

### 히스토그램이란?

히스토그램은 간단히 말해서 도수분포표를 그래프로 나타낸 것이다. 영상 처리 측면에서 보면 영상의 픽셀 값 분포를 그래프 형태로 표현한 것이다. 모든 픽셀의 밝기 값들을 구한 후 밝기 값 마다의 픽셀 개수를 세어서 그래프를 구성한다.

히스토그램에서 가로축을 히스토그램의 빈(bin)이라고 하며 그레이스케일 영상의 경우 256개의 빈을 가진 히스토그램을 구하는 것이 일반적이지만 경우에 따라서 빈 개수를 다르게 할 수도 있다.

### 히스토그램 스트레칭 (normalized histogram)

'normalized'란 영어 뜻에서도 알 수 있듯이 히스토그램 스트레칭은 일종의 `"정규화"`와 같다. 앞서 우리는 명암이 낮은 경우 픽셀의 분포가 한 쪽으로 몰려 있는 것을 히스토그램을 통해 확인할 수 있었다.

그러한 치우친 분포를 "히스토그램 스트레칭"을 통해 넓게, 고르게 펴주는 작업이 필요하다. 만약 명암이 낮은 경우, 해당 작업을 거치면 명암비가 증가하며 얼굴 인식에 있어서 좀 더 명확한 인식이 가능케 될 것이다.

히스토그램 스트레칭은 어떤 방식으로 이루어질까? 사실 깊게 파고들자면 복잡하다. 일단 알아가는 단계이므로 간단히 이해해보기로 하였다.

우리가 `normalize()` 메서드를 이용해 진행할 스트레칭 작업에서 첫 번째 매개변수는 "원본" 이미지이어야 한다. 즉, 원본 이미지를 기존에 지정한 `img` 변수를 그대로 설정한 뒤, `saturate_contrast()` 함수를 통해 명암을 조절(변경)한 이미지를 `var_img` 로 둔다. 그 뒤 진행되는 코드에서 `img`를 `var_img`로 변경시키는 것 또한 잊지 말자.

이러한 변수명 변경 후, 히스토그램을 그리는

`hist = cv2.calcHist([dst], [0], None, [256], [0, 256])`

다음의 코드에 첫 번째 매개변수에 (기존엔 `variable_img`였을 것이다. ) 정규화 작업을 통해 만들어 준 결과물인 `dst`를 주입시켜준다.

우리는 스트레칭 작업을 하기 전, 명암을 낮춘 상태로 설정함으로써 픽셀의 분포가 한 곳으로 모여있는 경우를 확인할 수 있었다. 하지만 스트레칭 작업 후 명암의 분포를 고르게 만들 수 있게 되었고, 영상 이미지의 명암 또한 커진 것을 확연히 볼 수 있게 되었다.

하지만 위와 같은 경우도 완벽한 문제 해결이라 보긴 어렵다.
우리가 명암을 높여줌으로써 분포를 나름 고르게 할 순 있었지만 아직도 여전히 낮은 픽셀에 분포 값들이 모여 있는 것을 확인할 수 있다.

조금 더 쉽게 말하자면, 히스토그램 스트레칭을 통해 분포도는 넓혀주었지만 특정 그레이 스케일 값에 픽셀 분포가 뭉쳐있는 문제를 해결할 수 없는 것이다.

우리는 이와 같은 문제를 아래서 보게 될 `"히스토그램 평활화(histogram equalization)"`을 통해 해결할 수 있다.

### 히스토그램 평활화 (Histogram Equalization)

히스토그램 평활화를 하는 방법은 여러가지가 있지만, 이번 포스팅에서는 위으 스트레칭 방법과 마찬가지로 cv2에서 제공하는 메서드를 사용할 것이다.

해당 메서드는 `equalizeHist()`이다. 인자로는 원본 이미지를 받을 것이므로 우리는 `img`를 주입해준다.

```python
dst = cv2.equalizeHist(img)

hist = cv2.calcHist([dst], [0], None, [256], [0,256])
plt.subplot(3, 1, 2), plt.imshow(dst)
plt.subplot(3, 1, 3), plt.plot(hist, color='r')
plt.xlim([0, 256])
plt.show()
```

히스토그램 평활화는 뭉쳐져 있는 픽셀 부분은 펼쳐주고, 빈도가 적은 부분은 오히려 뭉쳐지게끔 해준다.
해당 코드는 "Gray_Scale"에 해당하는 코드이다.
