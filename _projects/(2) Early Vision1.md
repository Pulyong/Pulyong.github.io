---
name: Hand,Car Video Detection using Early Vision Method
tools: [python, opencv]
image: https://github.com/Pulyong/Early_Vision_Project/assets/76218918/c8275525-a12e-45c2-a66d-c772c49034fe
description: Hand, Car Video Detection without DeepLearning Method.
---

# Video Detection
Object Detection은 특정 물체에 Bounding Box를 생성하는 Task로 Deep Learning의 핵심 Task 입니다. R-CNN,YOLO와 같은 좋은 성능의 DL 모델이 있지만, 본 구현에서는 DL 방법론을 사용하지 않고 Object Detection을 수행합니다. ML 방법론 또한 Background을 제거할 때만 사용하고 색상 채널의 변경, Morphology, Connected Component를 이용하여 Bounding Box를 생성합니다. 

# Hand Detection
Hand Detection은 말 그대로 사람의 손을 Detect하는 Bounding Box를 찾는 Task입니다. Hand Detection은 사람의 피부색의 unique함을 이용하여 색상 채널의 변경으로 간단하게 구현할 수 있었습니다.

알고리즘은 다음과 같습니다.

1. Video를 frames로 잘라 이미지로 변경한다.
2. rgb채널의 이미지를 ycr 채널로 변경한다. ycr 채널은 사람 피부를 찾기 좋은 채널이다.
3. ycr 채널에 threshold를 이용하여 피부색이 존재하는 영역만 가져온다.
4. 동그란 형태의 Ellipse kernel을 사용하여 morphology Close연산을 수행한다.
5. connected component 알고리즘을 이용하여 일정 크기 이상의 Area를 갖으면 Bounding Box를 그린다.

![output (online-video-cutter com)](https://github.com/Pulyong/Early_Vision_Project/assets/76218918/797bbae9-f3fb-4bc3-94f3-e230217026a1)

# Car Detection
Car Detection은 도로위의 차를 Detection 하는 Bounding Box를 찾는 Task입니다. 도로에는 그림자 같은 움직이지만 차가 아닌 물체들이 존재하기 때문에 조금 더 어려운 Task 였습니다.

알고리즘은 다음과 같습니다.

1. Video를 frames로 잘라 이미지로 변경한다.
2. Mixture of Gaussian을 이용한 MOG2 BackgroundSubtractor를 이용하여 움직이지 않는 Background를 제거한다.
3. 동그란 형태의 Ellipse kernel을 사용하여 morphology Close연산을 두번, Open연산을 두번 수행한다.
4. connected component 알고리즘을 이용하여 일정 크기 이상의 Area를 갖으면 Bounding Box를 그린다.

![output_car2 (online-video-cutter com)](https://github.com/Pulyong/Early_Vision_Project/assets/76218918/c8275525-a12e-45c2-a66d-c772c49034fe)



[Github](https://github.com/Pulyong/Early_Vision_Project/tree/main/Video_Detection)

**Summary**

- rgb to ycr 채널 변경, morphology 연산, Connected Component 알고리즘을 통한 Hand Detection
- background 제거, morphology 연산, Connected Component 알고리즘을 통한 Car Detection
- 자세한 사항은 Github 참고

**Role**

- 혼자 프로젝트 진행
- 방법론 탐색, 구현, 실험

**Result**

- 여러가지 색상 채널에 대한 이해
- Connected Component 알고리즘에 대한 이해
- 여러가지 Morphology kernel과 Morphology 연산에 대한 이해

**Period**

- 2023.06 ~ 2023.07