---
name: Image Processing using Mean,Median,HighBoost Filter
tools: [Mean Filter, Median Filter, HighBoost Filter]
image: https://private-user-images.githubusercontent.com/76218918/289892989-637333cd-51a8-4dd5-8683-4caa842f5e9e.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTI2Mjc5NzksIm5iZiI6MTcxMjYyNzY3OSwicGF0aCI6Ii83NjIxODkxOC8yODk4OTI5ODktNjM3MzMzY2QtNTFhOC00ZGQ1LTg2ODMtNGNhYTg0MmY1ZTllLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDA0MDklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwNDA5VDAxNTQzOVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTMzYjY3OWQzODMwYWExMTZlMDBmMDk4NjYzYmJlNTEyNmMyYzMwYWMzNTZkZTliMDYwNjFjY2NlZWI1MzRlYzEmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.GPgO-Yh1c8q4Mraf4dpVpVXFs6Gw5QrCx8XhTzsOquc
description: Noise remove & Image enhancement using Filter
---

# 💻Filter Implementation
CNN은 Filter 혹은 Kernel의 parameter를 업데이트 하며 Sliding Window를 수행합니다. 같은 개념으로 고정된 Weight를 가진 filter를 이용해서 Nosie를 제거할 수도 있고, Image를 Enhancing할 수도 있으며 경계를 찾아낼 수도 있습니다.  
여러 Filter 중 Mean, Median, Gaussian Filter를 이용하여 Salt & Pepper, Impulse, Gaussian Noise를 제거하는 Filter와 이미지의 high frequency 부분을 강조하는 High-Boost Filter를 구현했습니다.

### Original Image
![image](https://github.com/Pulyong/Early_Vision_Project/assets/76218918/637333cd-51a8-4dd5-8683-4caa842f5e9e)

# ✅Mean Filter
Mean Filter는 Filter의 weight 합이 1이 되는 filter입니다. 만약 3*3 크기의 filter가 존재한다면 각 weight는 1/9의 값을 갖고 이미지의 3x3 영역으로 Sliding Window를 수행합니다. 결국 Image의 3x3 영역에 각 pixel값에 1/9가 곱해지고 모두 더해지므로 mean을 계산하는 것과 같습니다. mean filter는 Gaussian Noise와 Impulse Noise에는 어느정도 잘 작동하나 Salt & Pepper와 같이 noise가 0과 255로 튀는 경우에는 잘 작동하지 않습니다.

### Gaussian Noise -> Mean Filter
![image](https://github.com/Pulyong/Early_Vision_Project/assets/76218918/061f3251-744c-4153-a4e7-9ab9b20b6b9d) 
![image](https://github.com/Pulyong/Early_Vision_Project/assets/76218918/0afd6d48-6dfb-464a-88b7-38f792a65281)


# ✅Gaussian Filter
Gaussian Filter는 Filter의 중앙, 예를들어 3x3 filter의 경우 (1,1),으로 부터 정규분포에 의해 값이 부여되는 Filter입니다. 그렇기 때문에 (1,1)이 가장 큰 값을 갖고 주변 영역은 점차 낮은 값을 갖습니다. 분산(Sigma)이 하이퍼파라미터로 존재합니다. Gaussian Noise에 가장 잘 작동하며 다른 Noise에도 어느정도 잘 작동하기 때문에 많이 사용됩니다.

### Gaussian Noise -> Gaussian Filter
![image](https://github.com/Pulyong/Early_Vision_Project/assets/76218918/061f3251-744c-4153-a4e7-9ab9b20b6b9d)
![image](https://github.com/Pulyong/Early_Vision_Project/assets/76218918/c76ecf48-090a-4eb5-bd85-596fb83dd16a)


# ✅Median Filter
Median Filter는 3x3 영역이 Sliding Window하면서 Image의 9개의 픽셀 값중 5번 째 값 즉, median 값을 취하는 filter입니다. 위의 filter들과 다르게 비선형적인 특징을 갖습니다. 중앙값을 취하기 때문에 Salt & Pepper Noise에서 잘 잘동합니다.

### Salt & Pepper Noise -> Median Filter
![image](https://github.com/Pulyong/Early_Vision_Project/assets/76218918/e4d814e0-e403-4f22-bc89-9937a6d0bd6f)
![image](https://github.com/Pulyong/Early_Vision_Project/assets/76218918/b2a26bdd-05b6-43ce-87a8-d81cb204520b)


# ✅High-Boost Filter
High-Boost Filter는 Sharpening에 사용되는 Filter입니다. Sharpening은 주변 pixel과의 차이를 극대화시켜 경계 부분의 명암 비를 증가 시키는 작업으로 high frequency 영역을 강조하는 역할을 합니다. Original Image에 high frequency를 더해주는 방법과 Original Image에 low frequency를 빼주는 방법이 존재합니다.

### Original Image
![image](https://github.com/Pulyong/Early_Vision_Project/assets/76218918/f50ca687-a572-4ff4-b8ea-97897418c98a)

### Add High frequency
![image](https://github.com/Pulyong/Early_Vision_Project/assets/76218918/0651d2b5-082e-480d-91da-e6ee17368f45)

### Sub Low frequency
![image](https://github.com/Pulyong/Early_Vision_Project/assets/76218918/78652a81-f1f6-40c3-af35-40f897996662)


[Github](https://github.com/Pulyong/Early_Vision_Project/tree/main/Filter)

**Summary**

- Mean, Median, Gaussian Filter를 이용한 Impulse, Gaussian, Salt & Pepper Noise 제거
- High-Boost Filter를 이용한 Sharpening
- 자세한 내용은 Github 참고

**Role**

- 혼자 프로젝트 진행
- 수식으로 표현된 Filter 구현

**Result**

- 자연에 존재하는 여러가지 Noise에 대한 이해
- Noise 별로 알맞는 Filter와 그 이유에 대한 이해

**Period**

- 2023.05 ~ 2023.06