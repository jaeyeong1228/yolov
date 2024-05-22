# yolov

우선 커스텀 제작 전 설치해야 할 것들이 있다.

(설치 과정과 방법은 [(5) YOLOV5 설치 및 기본 사용법 - YouTube](https://www.youtube.com/watch?v=-gCB9Iotjs8&t=278s) 이 영상을 기반으로 함)

1. yolov5 다운

[GitHub - ultralytics/yolov5: YOLOv5 🚀 in PyTorch > ONNX > CoreML > TFLite](https://github.com/ultralytics/yolov5)

나는 C:\Data 경로에 다운 받아서 압축을 풀었다.

설치 경로: C:\Data\yolov5-master

1. python 다운로드

파이썬은 3.12.3 버전을 다운 받았다.

1. vscode 다운로드

vscode은 최신 버전으로 다운 받았다.

여기까지가 설치 과정이고 pytorch는 다운받아야 하는지 아닌지를 확실히 모르겠다.

왜냐하면 pytorch 없이도 커스텀한 프로그램이 완성되었기 때문이다.

# 1) yolov5 설치 및 vscode 실행

우선 설치가 다 끝났으면 vscode를 실행시키자.

yolov5가 설치되었다면 

이렇게 파일이 생성될 것이다.

terminal 창을 켜서 명령어: pip install -r .\requirement.txt 입력하여 설치한다.

참고로 명령어는 powershell 말고 cmd로 실행시켜 입력하자.

# 2) Labeling 딴 사진들 넣기

라벨링 딴 사진들을 yolov5 폴더에 넣어야한다.

프로그래밍을 사용해서 정리해도 되지만 난 수작업을 진행하였다.

train과 valid 폴더를 생성해준다.

train 폴더와 valid 폴더에 images와 labels 폴더를 생성한다.

classes.txt 는 train 폴더와 valid 폴더에 저장해둔다.

images 폴더에는 이미지만, labels 폴더에는 label 딸 때 생성된 txt 파일을 넣는다.

train 폴더와 valid 폴더에 각각의 사진의 비율을 나눠야 하는데, 

우선 train과 valid 는 8:2 비율로 100장 중 각각 80장과 20장으로 나누었다.

(나중에 더 자세히 다룰 예정)

추가내용!!) 현재 train과 valid 밖에 안만들었지만 test 도 추가해서 해야함

[[Yolov5] Custom Dataset으로 Yolov5 돌려보기 : Train](https://minmiin.tistory.com/15)

여기 참고해서 만들기!!!!

## 3) yaml 파일 생성

data 폴더 안에 data.yaml 파일을 생성해주면 된다. (vscode에서 진행)

data.yaml 파일에 사진처럼 입력하자.

train 폴더 경로와 valid 폴더 경로를 지정해준 것이다.

nc는 클래스 개수를 말하는 것이고 names 는 클래스 이름을 의미한다.

# 4) 명령어를 통해 학습

1) 터미널에서 명령어를 칠 때 yolov5 폴더로 들어가서 명령어를 입력해야 한다.

```python
cd yolov5 또는    /   cd + 자기가 지정한 욜로 파일 이름이나 경로
```

2) 명령어

명령어는 GPU 사용 유무에 따라 두 가지 명령어로 나뉜다.

i) GPU 사용x, CPU 사용

```python
python train.py --img 640 --batch 16 --epochs 30 --data data/data.yaml --cfg models/yolov5s.yaml --weights weights/yolov5s.pt
```

내 노트북은 그래픽도 낮고, GTX 그래픽카드가 없으므로 위 방식을 사용

```
  --img    :   이미지 크기

  --batch  :   배치크기

  --epochs : epoch크기

  --data  : 실행할 yaml파일의 위치 및 실행할 yaml파일명

  --cfg    : 아래에서 정한 모델 크기 (yolov5/models 폴더에 yaml파일로 저장되어 있음)

  --weights : 미리 학습된 모델로 학습할 경우 (yolov5s.pt 등의 형식으로 다운로드 가능)

  --name : 학습된 모델의 이름

 ( 파라미터의 더 자세한 설명은 python train.py -h 하면 나온다)

```

ii) GPU 사용 O

```python
python train.py --img 640 --batch 16 --epochs 30 --data data/data.yaml --cfg models/yolov5s.yaml --weights weights/yolov5s.pt  --device 1,2
```

뒷부분에 —device 사용할 번호

(만약 RuntimeError: Unable to find a valid cuDNN algorithm to run convolution~~

에러가 나면 GPU가 감당을 못하는것이니 —device 0, 1, 2, 3 다 돌리던가 아니면 어쩔 수 없다.

코드 내용 중

--cfg models/yolov5s.yaml

이 부분이 있는데 뒤에 yolov5s에서 맨 뒤 영어 숫자만 바뀌면 모델 명이 바뀌게 된다.

이것이 주어진 모델이다.

기본적으로 모델이 이렇게 존재하는데 크기가 클수록(우측으로 갈수록)  정확도가 높아지지만, GPU 및 시간을 많이 차지한다.

그래서 나는 yolov5s로 모델을 만들었다.

# 5) 돌린 후 결과

나는 아래의 명령어를 입력해서 커스텀 프로그램을 만들었다.

```python
python train.py --img 640 --batch 10 --epochs 10 --data data/data.yaml --cfg models/yolov5s.yaml --weights weights/yolov5s.pt
```

이미지 크기는 640, 배치 10, 에포크 10

10 epochs 를 처리하는데 대략 10분정도 소요되었다.

결과물은 runs\train\exp 파일에 저장된다.

결과물에 대한 정보는 다른 페이지에서 좀 더 상세히 다뤄 볼 예정이다.

# 6) 학습된 모델 사용하기

```python
python detect.py --weights runs/train/exp/weights/best.pt --source ./data/images/testvideo.mp4 --conf 0.5
```

- -source : 불러오는 이미지 경로 ( 나는 /data/images/ 에 영상을 저장했다.)

다른 곳에 저장하니 잘 안돼서 저기에 저장했다.

- -weight : 가장 좋은 best.pt의 경로
- -conf : 정확도 설정 (0~1 까지 가능, 숫자가 1에 가까울수록 깐깐하게 평가함)

우선 100장으로만 학습을 시켰기 때문에 정확도는 0.5로 설정하였다.


그럼 이렇게 동영상 프레임 수에 맞춰 detect 과정이 진행된다.

우리는 30frame 으로 찍은 12초 영상이므로 369개가 나왔다.

영상은 run\detect\exp9 에 저장되었다.


영상을 보면 아직까진 구별을 잘 못한다.

더 많은 사진 데이터들을 이용해 학습시키면 이 부분은 개선될 것으로 예상된다.

**2024.05.21)**

100장, 500장, 1000장으로 학습을 완료한 상태이다.

1000장에 대해서만 설명하고 모델을 다뤄보겠다.

우선 내가 사용한 명령어를 살펴보자

```python
python train.py --img 640 --batch 24 --epochs 10 --data data/data.yaml --cfg models/yolov5s.yaml --weights weights/yolov5s.pt
```

입력 이미지 크기는 640, batch는 24, epochs는 10, 그리고 모델은 yolov5s 로 제작하였다.

판단이 잘 된다.

하지만 webcam으로 실시간 영상을 판단하였을 땐 아직까지 정확한 판단을 하지 못한다.

따라서 사진의 다양성과 epochs 수, 그리고 모델을 변경하여 좀 더 성능이 좋은 모델을 만들 예정이다.

Train, Valid, Test에 사용된 사진의 비율은 7:2:1 로 설정하였다.

Roboflow 공식 사이트에서 처음 학습자들에게 추천하는 비율을 이용하였다.
