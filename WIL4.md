1. 정의
-input: RGB Image
-output: label, bbox(bounding box) info(x,y,width,height)

2. Image Classification vs Object Detection
        single label    :     multi label
        label           :     label + bbox info
        low resolution  :       high resolution     ; resolution(해상도)

3. Location Information(single) - what?
    
    Image-> CNN Model -> Vector1    ->      Vector2 >>> Softmax Loss
    (R,G,B)                       FC Layer

    Location Information(single) - where?
    
    Image-> CNN Model -> Vector1    ->      Vector2 >>> L2 Loss
    (R,G,B)                       FC Layer

    Location Information(multiple) >>> Sliding Window

4. Sliding Window - Detecting Multiple Objects
    N = (W-(w-1))*(H-(h-1))    (W: 이미지 너비, w: 윈도우 너비, H: 이미지 높이, h: 윈도우 높이)
    Total possible boxes: (H(H+1)/2)*(W(W+1)/2)
                ex) 800*600 사이즈의 이미지 >>> 5800만번의 Image classification 필요; 실행불가

5. Regional Proposal ; 어떻게 하면 객체를 포함할 확률이 높은 이미지 영역을 찾을 수 있을까?
-Candidate Region 만들기
-Selective Search (CPU가 몇초동안 약 2000개의 영역을 제안)
-신경망으로 대체 (Region Roposal Network)

6. R-CNN
    Selective Search 알고리즘을 사용한 Object Detection 모델
    Region-Based CNN
    i) selective search로 RoI(Region of Interest) 뽑기 ; 사이즈 모두 다름
                            주의깊게 볼 영역들
    ii) region size process (224*224로 warping) ; (warping: 사이즈 통일)
    iii) Compute CNN features 
    iv) Linear SVM classify
              SVM: Support Vector Machine

7. IoU(Intersection over Union) ; bbox 비교방법
    IoU = (Area of Overlap)/(Area of Union)
-IoU가 1에 가깝다 = 같은 object를 가르키고 있을 확률이 높다
-threshold는 user가 정함
    ex) IoU > 0.5 : decent
        IoU > 0.7 : pretty good 
        IoU > 0.9 : almost perfect

8. Non-Max Suppression(NMS) ; 겹치는 bbox 지우기
    i) 가장 높은 score를 가진 박스를 선택
    ii) 나머지 score를 가진 박스들과 IoU를 비교하여 threshold (ex. 0.7) 이상의 IoU를 가진 박스들을 제거
    iii) 무한 i),ii) 반복
        >>> 문제: 애초에 사진안에 있는 객체들이 겹쳐 있는 상황인 경우 좋은 bbox를 제거하기도 함  

9. Mean Average Precision(mAP) - Object Detector를 평가하는 기준
각 category별 Average Precision의 평균
    i) test image를 detector에 넣고 통과시킨다
    ii) NMS를 가지고 overlapping된 bbox를 제거한다
    iii) 각 카테고리별 Average Precision을 계산한다
threshold의 설정에 따라 정밀도와 재현율 값이 변화한다
    confidence score;
        threshold 값이 높으면 bbox수가 적어짐(신중해짐) 
        threshold 값이 낮으면 bbox 수가 많아짐(bbox 난사)
    Precision           vs      Recall Curve
    tp/(tp+fp)          :       tp/(tp+fn)
    tp/(all detections) :       tp/(all ground truths)
    모델이 positive라고            실제 맞게 예측한 것 중에서 모델이
    예측한 것 중에서 실제    :       positive라고 예측한 비율
    True인 비율

10. Fast R-CNN
-CNN과 Resize의 순서를 바꾸어 속도 개선
-Classification으로 SVM에서 linear classification 사용
    R-CNN: region 뽑고 warping & ConvNet
    Fast R-CNN: ConvNet 돌리고 region을 projection하여 해당 feature map을 뽑아와서 ROI Pooling하여 
                class와 bbox 뽑아냄 (ConvNet에서 FC Layer 없음)

11. Faster R-CNN 
-Region Proposal Network(RPN)의 등장
    i) CNN을 통과시켜 feature map 뽑기
    ii) RPN으로 region proposal
    iii) ROI Pooling 거쳐 classification bbox regression
RPN
-앵커박스 안에 객체를 포함하고 있는지 아닌지 판단
-앵커박스(Anchor box)
  ->1*1 binary classification 수행 ; Classification Loss
-객체를 포함하고 있다면 구체적으로 중심을 받아 resize를 해주는 역할
