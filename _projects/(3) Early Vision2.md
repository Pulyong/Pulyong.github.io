---
name: Classification using Early Vision Method(SIFT,LBP)
tools: [python, opencv]
image: https://private-user-images.githubusercontent.com/76218918/289917930-c7612d53-97fd-442b-a876-aeb6826add8c.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTI2Mjc5MTksIm5iZiI6MTcxMjYyNzYxOSwicGF0aCI6Ii83NjIxODkxOC8yODk5MTc5MzAtYzc2MTJkNTMtOTdmZC00NDJiLWE4NzYtYWViNjgyNmFkZDhjLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDA0MDklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwNDA5VDAxNTMzOVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWM3NTQ0MmU5ZGRlNDJkNjZiYzI2ZDAyZTJmZjhmY2I5NTYxZjc5MGI2YmMwMWZmNDRmYzI0MGEzZjhkZmRhZWMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.jIBUHVGCpatNqP0cOID9UsSAGQTUordVxMaD4WDaGtY
description: Classification without DeepLearning Method.
---

# ❓Image Classification
<a href='https://github.com/google/recaptcha' target='_blank'>Recaptcha</a> dataset의 12개 class중 Mountain과 Other class를 제외한 10개의 class를 SIFT와 LBP를 이용해 Classification 하는 모델을 구현했습니다. 모델의 알고리즘은 다음과 같습니다.
1. Dense SIFT를 통해 Image의 모든 영역의 Shape Feature를 뽑는다.
2. 동일한 위치에서 LBP를 통해 Image의 Texture Feature를 뽑는다.
3. Training Image의 모든 SIFT feature vector를 K-means로 clustering한다. 이 때 비슷한 물체(의자, 소파 등등)는 같은 feature는 같은 cluster로 할당될 것이고 해당 cluster는 특정 물체를 설명하는 Cluster가 될 것이다.
4. Training Image의 모든 LBP feature vector 또한 K-means로 clustering 한다.
5. Image별로 K-means로 predict된 SIFT vector와 LBP vector의 Histogram을 만든다.
6. SIFT, LBP Histogram을 concat하여 최종 feature로 이용한다.
7. KNN을 이용하여 Image 별로 최종 feature를 학습한다.
8. Test Image에 대해서도 최종 feature를 얻어 낸 후 KNN을 이용하여 predict한다.

# SIFT
SIFT는 Scale Invariant Feature Transform의 약자로 이미지의 Scale, Rotation에 Robust한 Feature를 추출하는 알고리즘입니다.  
Scale-space extrema detection, Keypoint localication, Orientation assignment, Keypoint descriptor 의 총 4단계로 이루어 집니다. 여기서 Dense SIFT는 KeyPoint를 찾지 않고 Slidinig Window를 하며 Feature를 추출하므로 Scale-space extrema detection, Keypoint localication 단계는 건너 뛰겠습니다.

SIFT의 전체적인 알고리즘은 <a href='https://bskyvision.com/entry/SIFT-Scale-Invariant-Feature-Transform%EC%9D%98-%EC%9B%90%EB%A6%AC' target='_blank'>여기</a>를 참고했습니다.

### Orientation assignment
Orientation assignment는 Sliding Window안의 중심에 rotation invariance를 갖게 하기 위해 방향을 할당해 주는 단계입니다. 영역을 가우시안 블러링 한 다음 모든 픽셀들의 gradient 방향과 크기를 구합니다.

### Keypoint Descriptor
16x16 window를 설정하고 각 4x4영역 별로 8개 방향으로 구분하여 크기를 할당합니다. 4x4영역 8개 방향이 16x16 window안에 16영역으로 존재하므로 16x8 = 128 feature vector로 나타내어 집니다. 각 point 별로 128 feature vector가 나오기 때문에 이미지별로 nx128개의 Matrix가 나옵니다.

# LBP
LBP 는 Local Binary Patterns의 약자로 이미지의 Texture feature를 추출하는 알고리즘 입니다. 3x3 window를 갖는다고 하면 window의 중심 pixel값을 기준으로 주변 값을 하나하나 판단하여 중심값보다 작으면 0 크면 1로 나타냅니다. 그 다음으로 0과 1을 나열하여 2진수로 나타내고 그 값을 10진수로 표현할 수 있습니다.

SIFT의 전체적인 알고리즘은 <a href='https://bskyvision.com/entry/Local-binary-pattern-LBP%EC%9D%98-%EC%9B%90%EB%A6%AC-%EB%B0%8F-%ED%99%9C%EC%9A%A9' target='_blank'>여기</a>를 참고했습니다.  

기존 원시 LBP가 아닌 Rotation-invariant LBP를 사용하여 구현하였습니다.

# Bag of Visual Words
Bag of Visual Words는 뽑아낸 Feature를 clustering을 통해 특정 cluster로 할당하는 방법론입니다. 의자를 생각해보면 대부분 의자들은 비슷하게 생겼을 것이고 SIFT나 LBP로 뽑아낸 의자 feature는 하나의 cluster에 할당될 것입니다. 사람이라면 눈,코,입 등의 feature가 각각 cluster에 할당될 것이고 이 Image의 Cluster를 Histogram에 나타내 보면 얼굴만의 특별한 Histogram이 나타날 것입니다. 이렇게 특정 feature가 Cluster라는 bag에 담긴다고 하여 Bag of Visual Words라는 이름을 갖습니다.
![image](https://github.com/Pulyong/Early_Vision_Project/assets/76218918/c7612d53-97fd-442b-a876-aeb6826add8c)

# KNN
KNN은 k nearest neighbor의 약자로 근처 K개의 가까운 data point들을 이용하여 predict하는 방식입니다. 위에서 만든 Histogram은 사람이면 사람끼리 비슷한 영역에 존재하고, 나무면 나무끼리 비슷한 영역에 존재 할 것이고 어떤 새로운 사람 Image가 들어왔을 경우 주변 k개는 사람일 가능성이 높습니다. 이런식으로 Classification을 수행하고 주변 10개 data point를 이용하여 retrieval도 수행하였습니다.

![image.jpg1](https://github.com/Pulyong/Early_Vision_Project/assets/76218918/dfb25d25-a15a-47e0-a684-b83ce591d348)
![image.jpg2](https://github.com/Pulyong/Early_Vision_Project/assets/76218918/6bc0aadd-fb7d-4bde-ac04-88f843c4498a)


[Github](https://github.com/Pulyong/Early_Vision_Project/tree/main/Image_Classification)

**Summary**

- SIFT와 LBP를 이용한 Recaptcha Image Classification
- Bag of Visual Words 방법론을 이용하여 Classification을 진행
- 자세한 내용은 Github Readme 참고

**Role**

- 프로젝트를 혼자서 진행
- 방법론 탐색, 구현, 실험

**Result**

- SIFT, LBP를 이용한 Shape, Texture Feature에 대한 이해
- K-Means, KNN에 대한 이해
- Bag of Visual Words 방법론을 통한 Classification

**Period**

- 2023.06 ~ 2020.07