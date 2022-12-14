# 2022 국립국어원 인공지능 언어능력 평가 
## **prove 폴더 사용설명서 반드시 읽어볼 것**
 
## 진행기간 
- 2022.10.04. ~ 2022.11.09.

## 개요
- **국립국어원이 구축한 '속성기반 감성 분석' 말뭉치를 활용하여 언어에 나타난  
개체의 속성 정보와, 속성정보에 대한 감성을 동시에 분류한다.**
  - 속성 범주 탐지(Aspect Category Detection, ACD)  
    - 문장에서 분석의 대상이 되는 범주들을 추출하는 과정이다  
    - 4개의 '개체'와 7개의 '속성'으로 구성된다.    
    
    
    구분 | 항목|
    :-----:|:----:|
    개체 | 제품 전체, 본품, 패키지/구성품, 브랜드|
    속성 | 일반, 가격, 디자인, 품질, 편의성, 다양성, 인지도|

     ex) 제품 전체#인지도
  
  - 속성 감성 분류(Aspect Sentiment Classification, ASC)  
    - 속성 범주 탐지에서 추론한 '개체#속성' 범주에 대한 화자의 감성을 긍정, 부정, 중립 으로 분류한다.  
    - 예를들어 "아이폰은 이쁜데 비싸요"와 같은 문장의 경우, “제품 전체#디자인”은 긍정으로 분류할 수 있으나,  
    “제품 전체#가격”은 부정으로 분류할 수 있다.      
       
   
## 위치eㅔ너지 팀   

- 한민재 : 모델 구축, 핸들링
- 정가영 : 데이터 관리 및 기획
- 이성연 : 모델 핸들링, 데이터 정리
- 양병진 : 소프트보팅, 데이터 크롤링
- 김규인 : 데이터 증강, 하드보팅
___
## 폴더 설명

- data
  - 모델 학습에 사용된 데이터셋
  

- data_analysis
  - 데이터 전처리에 사용된 코드

- predict_voting
  - 모델 예측, 오토라벨링, 하드보팅, 소프트보팅에 사용된 코드

- prove
  - 최종 제출파일 검증 코드
  
- train_model
  - 모델 학습에 사용된 코드    

># 모델 검증시 *prove  폴더 사용 설명서*
>>1. 모델 다운로드
>>>- url : http://github.com/hanmoonje/Korean_ABSA_potential
>>>내의 prove 폴더에 'predict_final_potential.ipynb' 활용
>>>모델 돌릴 시 jupyter 파일 위치에 'tmp'라는 이름의 폴더가 없는 환경에서 구동

>>2. 필요한 설치 ( colab 환경에서는 필수 설치 )
>>>- !pip install transformers==4.24.0 (colab환경 시 필수 설치, 가상환경의 경우 터미널에 설치)
>>>- !pip install datasets==2.6.1 (colab환경 시 필수 설치, 가상환경의 경우 터미널에 설치)
>>>- !pip install gdown (colab환경 시 필수 설치, 가상환경의 경우 터미널에 설치)

>>3. 위 과정이 완료 시 모듈 import 항목부터 모두 실행
>>>- ! 실행 중간에 !gdown 환경에 의해 중단 시 중단된 위치에서 다시 작동해 끝까지 실행
>>>- ./tmp 위치에 완성된 'final_potential.jsonl'파일 생성
>>>- 해당 파일 제출 시 리더보드 성적과 일치


## 사용된 데이터  

- 국립국어원 제공 ['속성 기반 감성분석 말뭉치'](https://corpus.korean.go.kr/)
- 모두의 말뭉치 ['속성 기반 감성분석 말뭉치 2021'](https://corpus.korean.go.kr/)
- [네이버 쇼핑 크롤링 데이터](https://shopping.naver.com/home)

## 과정  

 1. 개발환경 : Python, PyTorch, Colab, AWS(Amazon Web Servise)
 
 2. 데이터 전처리
   - 전처리
     - 데이터프레임 변환  
       JSON 형식으로 구성되있는 데이터들을 데이터프레임으로 변환하는 과정을 거쳤다.  
       해당 과정에서 학습에 용이하기위해 여러 속성을 갖는 문장의 경우, 하나의 문장에 하나의 속성을 갖도록   
       전처리 과정을 거쳤다.  
       
     - 오토라벨링  
       네이버 쇼핑에서 크롤링한 14만개의 데이터의 속성분류를 수작업으로 할 수 없었기에 오토라벨링 모델을 만들었다.  
       1. 여러 버전의 오토라벨링 모델을 만들어 여러 예측값을 찾아낸다.  
       2. ms Excel의 필터 기능을 이용하여 하드 보팅을 진행하였다.  
       3. 마지막으로 직접 검수하는 과정을 거쳤다.
      
      
   - 증강  
     최대 1:300 의 데이터 불균형을 해소하기위해 적은 범주에 대해 증강과정을 수행하였다.  
     증강과정을 수행한 속성은 아래와 같으며, 최종적으로 최대 1:43 으로 데이터 불균형을 해소하였다.  
     
     증강 수행 속성|
     :-------------:|
     브랜드#품질, 브랜드#디자인, 브랜드#인지도, 브랜드#가격, 제품 전체#다양성, 패키지/구성품#다양성, 패키지/구성품#가격, 패키지/구성품#일반, 패키지/구성품#품질, 본품#인지도, 본품#다양성|  
     
     - 역번역  
       [Pororo](https://github.com/kakaobrain/pororo)  라이브러리를 사용하였다. 역번역을 구현하기위해 번역task를 사용하였으며, 원본 데이터를 영문으로 번역한 후,  
       다시 한글로 번역하는 과정을 거쳤다.  
       N개의 데이터를 생성하는데 성공하였다.
       
     - 유의어 변환  
       [Pororo](https://github.com/kakaobrain/pororo) 라이브러리를 사용하였다. 유의어 변환 task를 사용하였으며,   
       데이터셋 버전에 따라 N개의 데이터 혹은 2N개의 데이터를 생성하는데 성공하였다.
      
     - 문장 합치기  
       같은 속성을 갖는 문장들을 대상으로, ms Excel 필터 기능과 rand 함수를 사용하여 2개의 문장을 합치는 과정을 수행하였다.  
       원본, 역번역, 유의어 변환 과정을 거친 후 수행하였다.  
       4N개의 데이터를 생성하는데 성공하였다.
       

![CNN-Figure-02]( https://user-images.githubusercontent.com/114709607/201504392-a490f5b9-bab2-42c5-99c5-aaca791ed51b.png)  

___

   3. 데이터셋 조합
   
 데이터셋 | 데이터 갯수 | 
 :-------:|:-----------:|
 train + dev | 6334|         
 data + similar | 6735|       
 data + 108 | 6442 |         
 data + manual | 6623 |        
 data + all | 7826|          
 data + 10_17 | 7175|       
 data +108_1017_10_24| 7365|       
 final_data | 9641     |  

 4. 데이터셋 분석

  ![CNN-Figure-01](https://user-images.githubusercontent.com/114709607/201512983-cbcd6399-d378-4936-a48d-74a5eea11889.png)  
  
  - 위의 표는 데이터별 가장 좋은 f1 점수를 나타낸 것이다. 표를 보면 알 수 있듯이 증강된 데이터보다 외부데이터를 추가했을 때 f1 점수가 높고 데이터별로 라벨 수, 데이터 수, component의 수에 따라 영향이 있다는 것을 알 수 있음. 

    - 데이터별로 적합한 라벨이 있다.
      : data를 보면 학습데이터에서 2개의 레이블이 없음에도 불구하고 23labels보다는 25labels의 f1 점수가 더 좋은 것을 알 수 있다. 반면, data+108+10_17+10_24는 학습데이터에         2개의 레이블을 채웠음에도 불구하고 25labels보다는 23labels이 좀 더 유의미했다.

    - 레이블 간의 불균형을 해소해줘야 한다. 
      : data보다 data+α로 증강을 하거나 외부데이터를 추가하여 불균형을 해소했을 때 좀 더       유의미한 결과를 얻을 수 있다.

    - component는 여러 개를 뽑아내는 것이 조금 더 좋으나 비슷하다.
      : data+10_17과 data+108+10_17+10_24를 비교해보면 component가 n개인 data+10_17이 좀 더 좋지만 0.01차라서 component에 따른 영향은 없다고 볼 수 있다. 
    - 따라서, 레이블 간 불균형이 심할 때는 증강하는 방법보다는 외부데이터를 추가하는 것이 좀 더 효과적이며 외부데이터는 찾고자 하는 데이터의 성질과 비슷하여야 좋은 결 과를       얻을 수 있다는 것을 알 수 있다. 

 
 5. 모델 구축
   - BaseLine
     - BaseLine 모델을 기반으로 ELECTRA, BERT, RobertA를 검증하였다.  
       결과적으로 ELECTRA 모델의 성능이 제일 좋게 나왔으며, 한글 리뷰데이터를 학습시킨,
       [kykim/electra-kor-base](https://github.com/kiyoungkim1/LMkor)를 사용하였다.
       ![CNN-Figure-03]( https://user-images.githubusercontent.com/114709607/201506095-9f7056b0-fc6f-4a89-9e2a-f8a2a808a7e2.png)  
     - 속성범주 탐지와 속성감성 탐지 두개의 모델로 나눠져 있으며, 같은 문장이라면 속성범주가 달라도 속성감성이 같은 점을 이용해,
     각 모델의 최고값을 찾아 서로 concat 하는 방법을 사용하였다.
 
 6. 앙상블 기법
   - 소프트 보팅
     -  pt값을 이용해 소프트 보팅을 시도하였다. 
   - 하드보팅
     - 여러 모델의 결과값을 2개, 3개, 5개, 12개 조합으로 하드보팅을 시도하였다. 주로 3개 조합에서 좋은 결과를 얻었다.  


## 결과
- pre-trained 된 모델일지라도 어떤 데이터로 학습을 했는지에 따라 결과가 다르다는 것을 알 수 있다. 본 경진대회는 리뷰데이터로 속성기반 감성분석을 하는 것이 과제였으므로 다양한 리뷰데이터가 학습된 모델들을 앙상블 함으로써 좋은 결과를 얻을 수 있었다. 또한, 문장 당 개체#속성과 감성을 따로 학습하여 결과를 나타내기에 2개의 모델이 필요하고 앙상블 할 때는 entity_property는 polarity를 고정해놓고 앙상블을 하고 polarity는 entity_property를 정해놓고 앙상블을 하여 마지막에 각각 개별로 앙상블 된 것을 합치면서 성능을 올릴 수 있었다.
  
  

## 보완점
- 데이터 핸들링을 다양하게 했음에도 불구하고 단일 모델에서 61% 이상 오를 수 없었다. 단일 모델의 성능을 끌어올리기 위해 외부데이터 추가 시 모델의 학습에 적합하게 만들어주기 위한 다양한 방법을 모색함과 동시에 앙상블 최적의 조합을 찾기 위해서 앙상블을 하지 않았던 데이터들을 검토해봐야 할 것 같습니다.

## 참조
- Clark, Kevin, et al. "Electra: Pre-training text encoders as discriminators rather than generators." ICLR 2020.
https://arxiv.org/abs/2003.10555
