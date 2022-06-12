# Face Detector JS

canvas 를 이용해 이미지의 얼굴 위치를 식별합니다.

- [mauricesvay/php-facedetection](https://github.com/mauricesvay/php-facedetection) 프로젝트를 기반으로 만들었습니다.
- 저분도 [js 로 된 것을 포팅](https://svay.com/blog/face-detection-in-pure-php-without-opencv/)한것인데, 안타깝게도 원본 사이트가 없어져서 저분의 php 소스를 다시 js 로 포팅했습니다.
- 라이센스가 GPL-2.0 인 이유는 [원본의 라이센스](https://github.com/mauricesvay/php-facedetection/issues/18)를 유지해야하기 때문입니다.
- 이미지 크기에 따라 인식률에 큰 차이가 생깁니다. 알고리즘의 원리를 이해하지 못해서 크기 외의 인식률 개선 작업을 하지 못했습니다. 테스트에 사용된 이미지들은 대부분 280px 일 때의 결과가 가장 좋기 때문에 기본값이 280 입니다.
- 자원소모가 심한편이기 때문에 썸네일 정렬 용도로 사용하려면, 실시간 적용 보다는 업로드시 검출하여 direction 값을 함께 저장하는것이 좋습니다.

```javascript
const face = XenoFaceDetector.Detect([img or canvas], width, height, [multiple bool], [resSize int]);
{x:int, y:int, w:int}

const faces = XenoFaceDetector.Detect([img or canvas], width, height, true);
[
	{x:int, y:int, w:int},
	{x:int, y:int, w:int},
	{x:int, y:int, w:int},
	{x:int, y:int, w:int},
	{x:int, y:int, w:int},
]
```
- 로딩이 끝난 img 태그나, 이미지가 들어있는 canvas 태그를 입력하면 얼굴의 위치와 크기를 반환합니다.
#### multiple 
- multiple 값을 true 로 주면 얼굴위치를 최대 10개까지 더 찾아서 배열로 반환합니다.
- true 대신 숫자를 입력할 수 있고, 2~50으로 제한됩니다.
- 여러 얼굴 인식률이 특히 안좋아서 이미지 크기를 바꿔도 모든 얼굴을 인식하지 못할 확률이 높습니다.
#### resSize
- 원본 이미지를 그대로 사용하지 않고 이미지 크기를 줄여서 사용합니다. 가로,세로 중 작은쪽 기준으로 줄이며, 기본값은 280 입니다.

```javascript
const direction = XenoFaceDetector.AlignDirection(width, height, x, y, w);
```
- 얼굴쪽으로 정렬을 하기 위한 방향을 반환합니다.
- 가로가 길면 left, center, right, 세로가 길면 top, middle, bottom 중 얼굴이 감지된 위치를 반환합니다.
- 약 30% 이상 치우쳤는지로 판단합니다.

```javascript
const class_name = XenoFaceDetector.OnLoadAlignDirection(img, prefix, [multiple bool]);
```
#### prefix
- img 태그에 적용할 클래스명으로 AlignDirection 의 값을 prefix 를 붙여서 반환합니다.
#### multiple
- 얼굴이 둘 이상 감지되면 정렬이 의미 없으므로 정렬하지 않게 뒤에 true 를 줄 수 있습니다.
- 큰 것 대비 60% 이상 작은것을 거른 후 비교합니다.

```javascript
XenoFaceDetector.OnLoadAlignDirectionAddClass(img, prefix, [multiple bool]);
```
- OnLoadAlignDirection 를 호출하여 img 태그에 클래스를 직접 붙입니다.

```javascript
faces = XenoFaceDetector.FilterSmallFaces(faces);
```
- 가장 큰 것 대비 60% 이상 작은것을 걸러냅니다.

### 예제 보러가기: [example](https://crucifyer.github.io/facedetector-js/)
### php project: [https://github.com/crucifyer/facedetector-php](https://github.com/crucifyer/facedetector-php)