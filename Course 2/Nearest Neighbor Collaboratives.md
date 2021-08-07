## WEEK 2 

###  Nearest Neighbor Collaboratives. 



##### Course Introduction

Collaborative Filtering

* 유저와 아이템의 속성을 무시하고, 유저-아이템의 상호작용에 초점을 둠 
* 특별한 아이템을 추천할 수 있음 



Key-Concept

* Nearest Neighbor Collaborative Filtering : 나랑 비슷한 사람들이 좋아하는 것을 찾는 개념 
* User-User CF 
  * Neigborhoods and Tuning Parameters
  * Alternatives to Historic Agreement (신뢰, 관계 바탕)
* Item-Item CF
* Advanced topic : 많은 데이터가 없을 때, 그룹에 대한 추천 등의 상황에 대한 대응 



##### User-User Collaborative Filtering

* Score prediction 

  *유저 u의 아이템 i에 대한 예측 점수는 해당 유저의 평균 rating에 neighborhoods의 개별 평균 rating 대비 아이템 i에 대한 점수이 편차를 가중평균을 더한 값으로 계산됨*

  ![image-20210722190703305](C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210722190703305.png)

  * 가중치 ($w_{uv}$) :  유저간의 유사도 (피어슨 상관계수). 유저 u와의 유사도가 높을 수록 해당 유저의 rating이 많이 반영됨
  * neighborhood (V) : 모든 유저의 rating data를 이용하는 것이 아니라 해당 유저와 관련이 있는 유저들의 rating데이터만 이용함. 이 때 다음과 같은 기준들에 대한 조건 설정하여 결정
    * top k 설정 (유사도가 높은 유저들만 포함)
    * min similarity (유사도가 특정 수준 이상일때만 포함)
    * neg similarity 이용여부 (상관관계가 반대인 유저의 점수 포함할지 안할지 결정
  * Normalization : 단순히 해당 아이템에 대한 다른 유저들의 점수를 가중평균하지 않는 이유는 사람마다 score에 대한 bias로 인해 예측점수가 왜곡되지 않게 하기 위해서임.  (나의 4점이 다른사람의 2점과 같을 수 있음)

* Implemetation issues

  *유저간의 유사도를 계산하고 item, user별로 score을 예측하는 과정은 과도한 계산비용을 불러일으킴*

  * 해결 방식
    * 모든 사람의 rating data를 이용하지 않고 추려서 계산
    * user간의 유사도를 미리 계산해서 저장해놓고 사용 

* Assumption /Limitation

  * 한사람의 취향은 변화하지 않는다는 것을 가정 (하지만 어제의 취향과 오늘의 취향이 다를 수 있는 법..)
  * 하나의 domain 내에서만 적용 가능 (같은 음악취향을 가지고 있다고 같은 음식취향을 가지는 것은 아님)

* Configuration points 

  *처음에는 1) top N (~30) neighborhood 2) 가중평균된 score 3) user-mean or z-score normalization 4) normalized ratings에 대한 코사인 유사도로 알고리즘을 구성하는게 무난함.*

  1) Neighborhoods결정  (보통 25~30)

  * 모든 유저  : 데이터가 적을 때 
  * 특정 유사도 이상의 유저
  * 랜덤 유저 : 데이터가 너무 많을때 
  * Top N 유저
  * 특정 그룹의 유저 

  2) Neighborhoods의 item score 계산

  * 평균 / 가중평균 / 선형회귀 

  3) Normalizing data

  * z-score / user-mean 등 

  4) 유사도 계산

  * 피어슨 상관계수 : 둘다 평가한 아이템에 대해서만 계산. 두 유저가 각각 평가한 아이템은 많지만 공통의 아이템이 적을 경우 유사도가 무조건 낮게 나오는 문제점이 발생함. 
  * 코사인 유사도 : 코사인 유사도는 피어슨 상관계수와는 달리 정규화가 되어있는 지표가 아니라 rating 정규화 이후 사용하는 것이 좋음 



*Interviews*

* Influence Limiting and Attack Resistance (Paul Resnick)  
  *  가계정 파서 평점남기는 애들 대응방법 
* Trust-based Recommendation (Jen Golbeck)
  * 유사도를 가중치로 사용하는 대신에 사람들간의 신뢰(Trust)를 가중치로 사용하는 방법
  * 페이스북 같은 SNS등의 지표를 통해서 "신뢰" 추정
* Impact of Bad Ratings (Dan Cosley)
  * rating을 할 때 다른 사람들의 rating에 영향을 받는 것과 같은 현상에 대한 인터뷰



*assignments*

https://www.coursera.org/learn/collaborative-filtering/supplement/DVkH1/assignment-instructions-user-user-cf

* Part1 : 정규화하지 않고 CF 실행하기
* Part2 : 정규화하고 CF 실행하기 



##### Item-Item Collaborative Filtering

* Introduction 

  *다음과 같은  User-User CF의 한계를 개선하고자 등장. item은 그렇게 자주 변화하지 않아서 item-item 유사도는 꽤 안정적임.* 

  *  Issues of Sparsity : 대부분의 유저-아이템 매트릭스는 sparse하고, 이에 user-user CF를 활용할 때 추천이 이루어지지 않는 경우가 발생 . 
  * Computational performance : user profile은 자주 바뀌는 특성을 가지고 있고, 이에 따라 user-user similarity를 다시 계산해야하는 상황이 빈번함. 이에 따라 계산 비용이 많이 발생. 

* STEP
  * 아이템간의 유사도 계산 

    * rating 벡터간의 피어슨 상관관계 : multi-level rating에만 사용 가능 

    * 코사인 유사도 : multi-level , unary rating (0또는 1 ; 구매/비구매 , 클릭/안클릭) 모든 경우에 사용 가능 

      * unit vector로 normalize해줌 

      ![image-20210723000534359](C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210723000534359.png)

    * 조건부 확률 : unary rating 의 경우에 사용 가능

    cf. build model : item의 경우 어느정도 안정적이기 때문에 미리 계산해 놓을 수 있음. 이 때 몇몇 조건에 해당하는 아이템간의 유사도의 경우 계산을 안함으로서 계산시간을 더 줄일 수 있음

    cf. Item-Item Top-N : 아이템간의 유사도는 top-N을 바로 계산하는데 사용될 수 있음. 내가 산 아이템들과 가장 유사한 20개의 아이템들을 추천할 수 있음 (unary data에 유용)

  * user-item rating 예측 

    * multi-level data : item-neighbors의 가중합 방식 

      

      ![image-20210723000550707](C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210723000550707.png)

      -  neighbor 결정 : 유저 u가 평가를 한 아이템 중 아이템 i와 가장 비슷한 k개의 아이템  (보통 20개가 적당 )

    * unary data  : neighbor similarities의 합 

    * 선형회귀 방식 
  
* 장점  

  * 정확도가 꽤 높음
  * 계산이 효율적임(적어도 #of user >  # of item 일 때는)
    * 보통 item의 개수는 user의 수에 비해 적기 때문에 유사도 계산량이 적어지며, 유사도를 미리 계산해 놓을 수 있음 
  * 활용도가 높음 

* 단점 

  * item-item relationship 이 안정적이라는 것을 가정하기 때문에 몇몇 특별한 케이스 (년 초에만 사는 달력, 유행할때만 팔리는 책 등 )에는 적용하기 어려움
  * Lower Serendipity  : 뻔한 추천 밖에 못해줌. 

* Extensions & Hybrids

  * User trust : 아이템 유사도 계산 전 유저들을 trust에 따라 가중치를 줌. 

    * 더 신뢰가 가는 user가 추천에 더 큰 영향을 줌 

  * Papers and Page-Rank  :  논문의 HITS수에 따라 논문 유저의 중요도를 결정해 이를 가중치로둠 

  * Item-Item CBF

  * Deriving Weights

    

*Interview with Brad Miller*



*Assignment*

* 유사도 계산하기 

  

##### Advanced  Collaborative Filtering topics

* Cold Start Problem
* Threat Models
* Explanations





Quiz 

1. (T/F) 유사도 계산 방식인 피어슨 상관계수는 이미 정규화가 된 공식이라 따로 정규화를 해줄 필요가 없다.
2. (T/F) User-User collaborative filtering은 하나의 domain 내에서만 적용 가능하다. 
3.  유저들의 개별 아이템 구매 여부에 대한 데이터를 기반으로 item-item collaborative filtering을 진행한다고 할 때, 사용할 수 있는 유사도 계산 방식 두가지는 ? 



1. T (피어슨 상관계수는 정규화를 해줄 필요가 없고, 코사인 유사도는 해주어야함. )
2. T (같은 음악취향을 가지고 있다고 같은 음식취향을 가지는 것은 아님)
3. 코사인 유사도, 조건부 확률

