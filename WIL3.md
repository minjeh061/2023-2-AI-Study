1. 인공지능(AI): 인간의 지능을 인공적으로 구현
            >>>인간의 지능:지각능력, 인지능력, 기억과 이해 및 사고능력
                     >>>지각능력: 오감(시각,촉각,후각,미각,청각)
                                >>>시각: 뇌에서 처리되는 대부분의 정보
    Visual World > Sensing Device > Interpreting Device > Interpretation
    Interpretation -> Computer Graphics (Rendering)
    Computer Vision -> Interpretation (Inverse Rendering)
     >>>Computer Vision: intelligence를 얻기 위해 visual perception을 먼저 만드는 것 

2. Image Classification
- Classifier
    (i) 부서지기 쉬움
    (ii) 확장성이 높지 않다 
    Input -> Classifier -> Output
     x                      f(x)
- Semantic Gap

3. KNN (K-Nearest Neighbor)
훈련단계: 아무것도 하지 않고, 모든 훈련 데이터를 기억한다
예측단계: 새로운 이미지와 훈련 단계 데이터 비교 -> 가장 유사한 이미지를 라벨로 정한다
    L1(Manhattan) distance: |I1-I2|
    L2(Euclidean) distance: sqrt((I1-I2)^^2)

KNN 특징
    (i) 시간이 너무 오래 걸린다
    (ii) 이미지 간의 지각적 유사성 거리 != 픽셀 간의 거리
    (iii) 차원의 저주
    >>> 우리는 이미지를 보지만 컴퓨터는 거대한 픽셀 숫자 그리드를 본다 : Semantic gap

4. Linear Classification
- 매우 간단한 알고리즘, 신경 네트워크 구축
- 딥러닝에서 가장 기본적인 구성요소
Image Captioning: 컨볼류선 신경망 + 언어를 아는 신경망
    Input   Output
    이미지    설명문장
Parametric Approach
f(x,W) = Wx+b
  x:이미지 W:가중치 b:편향(bias)
Linear Classification 특징
    (i) 신경망의 가장 기본적 구성요소
    (ii) 가중치 행렬과 이미지 행렬의 계산으로 템플릿과 이미지 픽셀 사이의 유사성에 대한 score를 매김
    (iii) 선형분류기를 하나만 이용하면 템플릿이 하나로만 요약되는 단점이 있음 -> 다양한 블록들을 쌓을 필요

5. Loss Function; A Loss Function tells how godd our current classifier is 
Low Loss = good classifier
High Loss = bad classifier
Loss Function = objective Function = Cost Function

6. Multiclass SVM Loss (SVM: Support Vector Machine)
Li = sum(max(0,sj-syi+delta))
f(x,W) = Wx

7. Regularization; Beyond Training Error
>>> 정규화, 그리고 일반화 성능을 올리는...
L(W) = sum(Li(f(xi,W),yi))/N + lamda*R(w)
        >>>Data Loss            >>>Regularization
L2 Regularization : 매끄럽게, 모든 요소의 전체적인 영향
L1 Regularization : 분류기가 복잡할 때, 더 단순한 식으로
