# RCAN : https://arxiv.org/abs/1807.02758 

#### <I studied while referring to various sites, but it is not enough. Thanks anytime for feedback.>
### <heejo5@naver.com>

Image Super-Resolution Using Very Deep Residual Channel Attention Networks
--------------------------------------------------------------------------

* Deep Network를 구축하기 위해 Residual Block을 쌓는 것만으로 더 나은 개선을 얻을 수 없음
* Long skip connection, Short skip connection을 이용하여 다수의 Residual Groups 쌓음

Contribution
------------
  * 높은 정확도의 SR을 위한 Residual Channel Attention을 제안
  * 깊은 네트워크를 학습 할 수 있는 Residual in Residual을 제안
  * 각 채널들 사이의 상호 의존성을 고려하여 각 채널의 유익한 정도에 따른 채널을 재정의하는 Channel Attention을 제안

RCAN Architecture
-----------------
![image](https://user-images.githubusercontent.com/61686244/94658558-e97b6a80-033d-11eb-8ae7-ba3ee35b059e.png)

* Shallow Feature Extraction Net
    * Residual in Residual의 단계로 들어가며 추가적으로 upscale전 long skip connection에도 이용


* Residual in Residual(RIR) deep feature extraction
    * residual in residual, RIR 단계를 보게 되면 G개의 Residual Groups으로 구성 돼 있고 이 Residual Groups은 또 다시 B개의 residual channel attention block과 short skip connection으로 구성돼 있음


* Upscale module
    * upscale단계로 long skip connection과 함께 들어가게되는 RCAN에서 사용한 upscale 방법은 ESPCN에서 사용된 sub pixel convolution으로 원하는 scale factor r 이 있으면 r^2만큼 채널을 만들고 후에 Height와 weight에 r 을 곱한 하나의 채널에 매핑을 하는 방법을 사용

* Reconstruction
    * 한 개의 Convolution을 통해 최종 복원
    
Residual in Residual(RIR)
-------------------------
* 되면 G개의 Residual Groups 와 long skip connection, 그리고 그 안에 B개의 residual channel attention block과 short skip connection 이 존재하는 것을 확인 할 수 있음
* 다수의 skip connection과 residual groups으로 큰 receptive field를 가지는 깊은 네트워크를 만듦
* Long skip connection과 Short skip connection은 저 주파수 정보를 학습과정에서 우회하는데 사용을 하여 정보의 흐름을 용이하게 만듦
* residual groups만 쌓는 것으로는 더 좋은 퍼포먼스를 달성하는데 실패하기에 long skip connection을 추가적으로 넣어 학습과정을 안정화 시킴

Channel Attention
-----------------
![image](https://user-images.githubusercontent.com/61686244/94658338-a0c3b180-033d-11eb-91f6-ebfbb9b0284d.png)
![image](https://user-images.githubusercontent.com/61686244/94658381-ae793700-033d-11eb-9448-b9d176b0817c.png)

* Attention : 사용 가능한 자원들 중 가장 유익한 자원 요소로 편향 시키는 방법
* LR 공간에는 풍부한 저 주파수와 가치있는 고 주파수의 성분들을 가지고 있음
* 기존의 Conv layer의 각 채널들은 local receptive field 에서 수행을 하게 돼서 결과적으로 Conv후에 output은 local 영역 밖의 정보에 접근을 할 없음
* 채널 별 글로벌 공간 정보를 얻기 위해 입력으로 들어온 채널을 global average pooling을 적용하여 single value로 만듦
* 비선형적으로 상호 작용을 할 수 있고 여러 채널의 기능을 강조 할 수 있는 gating mechanism을 sigmoid로 적용
* global average pooling과  gating mechanism sigmoid를 적용한 Channel attention을 residual block에 적용한 RCAB를 이용

Investigation of RIR and CA
---------------------------
* Train Set : DIV2K / Test Set : Set14, B100, Urban100, Manga109 
![image](https://user-images.githubusercontent.com/61686244/94658476-cf418c80-033d-11eb-8f36-3b35dfc5ec11.png)


BI degradation model
--------------------
![image](https://user-images.githubusercontent.com/61686244/94657626-aa98e500-033c-11eb-90b4-582f04839829.png)

![image](https://user-images.githubusercontent.com/61686244/94657657-b7b5d400-033c-11eb-865b-27d91d4669f2.png)

BD degradation model
--------------------
![image](https://user-images.githubusercontent.com/61686244/94657705-ce5c2b00-033c-11eb-9e5b-1625ef2c2da6.png)
![image](https://user-images.githubusercontent.com/61686244/94657722-d5833900-033c-11eb-8205-399c1a6645a3.png)

Object Recognition Performance
------------------------------
![image](https://user-images.githubusercontent.com/61686244/94657847-05cad780-033d-11eb-8ef1-f6e6e5af39cc.png)

Conclusions
-----------
* RIR, LSC, SSC을 이용하여 깊은 네트워크를 만들어 큰 Receptive field를 얻음
* RIR은 많은 skip connection을 통해 저 주파수 성분을 우회시켜 main network가 고 주파수 정보를 학습하는데 초점을 맞춤
* 각 채널들 사이의 상호 의존성을 고려하여 각 채널의 유익한 정도에 따른 채널을 재정의하는 Channel Attention을 제안
* RCAN은 SR, BI, BD degradation model에서도 좋은 성능을 발휘함
