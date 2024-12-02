# Computer Graphics

# Part 5-2 - Transformations

#### 2024.10.25.(금)

## Image Transformation

### [Image Warping](https://velog.io/@claude_ssim/%EA%B3%84%EC%82%B0%EC%82%AC%EC%A7%84%ED%95%99-Image-Warping)

Image processing에는 주로 다루는 2개의 image operation이 있다. 그 중 하나는 filtering이고, 나머지 하나는 warping이다.

Filtering은 pixel의 값을 바꾸는 것이지만, warping은 pixel의 위치를 바꾸는 것이다. 우리는 수학적으로 위와 같이 filtering과 warping을 표현할 수 있다. Filtering은 수학적으로 image function의 범위를 바꾸지만, warping은 수학적으로 image function의 domain을 바꿔준다.

Image warping의 pseudocode는 다음과 같다.

```python
Result = WarpImage(Image, Warp)
    for y from 0 to H-1
        for x from 0 to W-1
            x2, y2 = Warp(x,y)
            Result[y][x] = Image[y2][x2]
        end
    end
```

여기서 우리는 WarpImage라는 function을 부르고, 2개의 parameter를 input으로 가지게 된다. 첫번째 parameter는 source image이고, 두 번째 parameter는 얼마나 pixel의 위치가 바뀌는지 정해주는 warping function이다. 이 function은 반복문 내에서 모든 pixel 에 대해서 동작하게 된다. 각각의 pixel에서 이에 대응되는 source pixel의 위치를 계산하고, function은 source pixel의 위치로부터 pixel
