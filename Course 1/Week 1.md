## Introduction to Recommender Systems : Non-personalized and content-based

https://www.coursera.org/learn/recommender-systems-introduction/home/



### Week 1

###### history

* 비평의 등장 
* 정보 검색 & 필터링
  * 정적 콘텐츠 기반 => 정보 검색 
    * 콘텐츠 인덱싱에 시간 투자 
    * ex . 도서관 : 저자,제목,출판연도 등으로 인덱싱 
    * TFIDF 접근 방식 
  * but 지금은 정보들이 너무 자주 바뀜 (news, email...) 필요한것만 보고 싶음 
  * 정보 필터링
    * 동적인 콘텐츠에서 정적인 정보를 필요로 할 경우임. 
    *  프로필 등으로 사용자 요구 모델링을 위해 투자 

* Collaborative Filtering
  * 정보가 너무 많아서 단순히 관련 정보를 원하는 게 아니라 "좋은" 정보를 원할때
  * Tapestry , Active CF
  * 비슷한 고객을 분석
* 자동화된 CF 
  * Group lens project  
    * 뉴스를 읽은 독자들이 1~5점으로 점수를 매기게함. 
    * 독자들은 자신과 비슷한 취향을 가진 다른 독자들과 matching
    * 이를 통해 특정 콘텐츠에 대한 선호도가 예측됨
    * 이렇게 예측된 선호도와 사용자의 행동은 잘 맞아떨어짐. 
* 추천시스템의 상업화 



######  Classic Collaborative Filtering 

*영화 추천 예시로 추천시스템 알고리즘 맛보기  (classic user-based collaborative filtering)*

* 알고리즘 
  * rating database 구축   
  * 해당 데이터베이스를 이용해 user간의 상관관계 도출
  * 사용자가 추천을 요구하면 상관관계를 이용해 해당 사용자의 neighbor을 찾음
  * 사용자의 neighbor가 좋게 평가한 영화 추천 

* 예시

  ![image-20210709184251329](C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210709184251329.png)
  * 목표 : 내가 Blimp와 Rocky XV 영화를 좋아할지 예측
    1. 나와 다른 user간 상관관계 찾기 - 이미 본 영화의 평점을 봤을때, 나와 Nathan은 영화를 비슷하게 평가, JK는 나와 반대로 평가. 
    2. Pat은 나와 비슷한 평가를 한 user지만 Blimp와 Rocky XV를 보지 않은 사용자라서 예측에 쓸모가 없음
    3.  blimp에 대한 다른 user의 평가를 봤을때, 나와 비슷한 nathan이 좋게 평가했고 나와 반대인 JK가 나쁘게 평가함. 따라서 나는 Blimp를 좋아할 것!
    4. Rock의 경우 나와 비슷하게 평가한 ben이 나쁘게 평가를 했기 때문에 별로 안좋아 할듯. 

  

  ###### 용어 정리 

  * **rating** : 선호도 표현 
    * explicit rating : 사용자가 직접 입력 (ex. 별점)
    * implicit rating  : 사용자의 활동에서 추출  (ex. 특정 페이지에 머무른 시간)
  * **prediction** : 추정된 선호도 (user A가 item A를 얼마나 좋아할까)
  * **Recommendation** : 사용자를 위해 추려진 item
  * **Content** : item의 속성 (ex. 영화 - 배우/synoptsis/제목 ... )
  * **Collaborative** : content의 속성들을 활용하기 보다는 다른 user에 집중. 

  

  ###### 접근 방식

  * 인기도, 집단 선호도 활용 (단순히 인기많은것 추천, 특정 그룹이 좋아하는것 추천)
  * Product Association : 제품 X를 좋아하는 사용자는 Y를 좋아한다는 가정하에 추천
  * Content-based : 사용자가 좋아하는 속성을 분석하여 해당 속성을 가지고 있는 제품을 추천
  * Collaborative : 다른 사용자의 경험을 통해 추천 

  

#### Preference & Rating

"preference" 를 나타내는 지표?  (구분기준 1. explicit/implicit  구분기준2. rating 시점)

* Explicit : 고객들에게 얼마나 좋은지 물어봤을때의 대답 (what users say)

  * Rating (ex. 별점)  
    * 평가 시점에 따라 분류 가능 (consumption/memory/expectation)
    * 문제점 : 사람들마다 편차가 있음. 사람마다 특정점수에 대한 의미가 다름. 
  * Review
  * Vote  (ex. 좋아요 / 싫어요)

* Implicit  : 고객의 행동을 수치화한 지표 (what users do)

  * Reading time (얼마나 오랫동안 기사를 읽는지 / 얼마나 오래 비디오를 보는지 등 )
  * Click
  * Purchase
  * Follow

  

#### Predictions and Recommendations

Prediction : 사용자가 특정 상품을 얼마나 좋아할지에 대한 지표  (how much?)

* 해당 지표를 기준으로 해서 item들을 비교할 수 있지만 사용자에 따라 해당 system 자체가 틀렸다고 생각하게 할 수 있음 

Recommendation : top 10, top 5.. 를 그냥 제안 (how much X)



#### recommender 분류

**기준**

* **domain** : 무엇을 추천할 것인가? 

   (ex. information / product / sequence ?    new item / re-recommend ?)

* **purpose**

* **context** : 쇼핑/ 음악듣기/친구들과 놀기/

* **whose opinion** : 전문가 추천/다수의 추천/ 나랑 비슷한 사람의 추천

  ex. 와인추천사이트 - 와인 전문가가 추천해 줌 (나랑 비슷하게 와인에 대해 아무것도 모르는 사람의 추천보다는 전문가가 추천해주는 것이 나음)  

* **personalization level**

   * Generic / Non-personalized : 모두가 같은 추천 
   * Demographic : 인구학적 정보에 따라 추천 
   * Ephemeral : 현재 활동 정보에 따라 추천 
   * Persistent : 개인의 정보에 따라 다르게 추천

* **privacy & trustworthiness** 

* **interface**

  * prediction/recommendations/filtering   
  * organic / explicit presentation
  * input - explicit / implicit

* **recommendation algorithms**

  * *non-personalized* : 베스트 셀러 / 가장 인기있는 /요즘 핫한 것들 추천

    ex. summary stats를 이용해서 visitor들에게 가장 인기있는 호텔 추천

  * *content-based* : 제품의 특성을 바탕으로  추천 

    * information filtering  / knowledge- based
    * user 별로 attributes에 대한 선호도 벡터가 만들어지고 update됨 이를 바탕으로 다른 제품에 대해서 그 제품이 가진 attributes를 기반으로 선호도를 예측 

  * *collaborative-filtering* : items, users를 각 축으로 하는 sparse matrix가 만들어지고 이를 바탕으로 빈 부분의 값을 예측

    * user-user : user간의 유사도를 계산하고 나와 비슷한 애가 좋아한 item추천 
    * item-item : item 간의 유사도를 계산하고 내가 예전에 좋아했던 item과 비슷한 item 추천 
    * dimensionality reduction  : matrix가 sparse 하기 때문에 차원을 축소해줘야할 필요성이 있음. 

  * *others*

    * critique
    * interview based



##### basic models

* users
* items
* ratings
* (community)



**Quiz**

1. item간의 유사도를 계산하여 내가 예전에 좋아했던 item과 비슷한 item을 추천하는 방식은 content-based 방식이다. 

   -> X item-item collaborative filtering 방식임이건 

2. item-item collaborative filtering 방식에서는 다른 user의 rating data는 필요하지 않다 

   -> item-item 유사도를 계산하는데 필요함 (taxonomy of recommender II, 18:00 전후 )



 ### Week 2

#### Summary Statistics



##### Examples 

*count, average, distribution 등의 지표 등은 적절하게 사용되면 효율적으로 서비스에 이용될 수 있음. 여러 방식으로 리뷰를 보여주는 것이 좋으나 여전히 한계는 있음.* 

* Story of Zagat  Guide
  * 식당별로  평균점수를 보여줌 
  * 계산 방식
    * rating =  {0,1,2,3}
    * score = round(MEAN(ratings)*10)
  * 이러한 평균을 사용하는 방식은 오해를 불러일으킬 수 있음 
  * normalization을 해서 조정 가능 (나의 4점과 다른사람의 4점이 다른 의미이기 때문. 표본이 적을 때 특히 문제가 됨. )
* 여행사
  * 각각의 문항 (매우 좋음, 좋음, 보통,, ,,) 에 답변한 사람의 비중을 보여줌 (분포)
* Amazon
  * 종합적인 평균 점수와 각 문항별 비중을 모두 보여줌. 
* Reddit
  * user's vote를 기준으로 상위 노출 



##### predict

Goal : 고객이 해당 제품을 사거나 읽거나 보는 것을 결정하는 것을 도와줌 

* 병균적인 rating
* net upvotes (좋아요 수)
* 별 4개 이상인 비율
* 모든 분포 



##### recommend (ranking item)

*꼭 prediction 에서 추산한 score을 기반으로 할 필요는 없음 (표본의 수가 너무 적거나, 해당 item이 너무 오래된것과 같은 상황을 고려해야함)*

###### 고려 사항

* confidence

* risk tolerance

* domain & business consideration (ex . age, system goals)

  * ex. time : 오래된것들 제거 

  * hacker news : $\frac{(net upvotes)^\alpha}{(age)^\gamma}P$

    * P : penalty score 
    * 오래된 news의 영향력을 적게 반영
    * 작동방식 : https://medium.com/hacking-and-gonzo/how-hacker-news-ranking-algorithm-works-1d9b0cf2c08d

  * reddit : $log max(1,|U-D|))+\frac{sign|U-D|t}{45000}$

    * 작동방식 : https://medium.com/hacking-and-gonzo/how-reddit-ranking-algorithms-work-ef111e33d0d9

    

###### statistics

* Damped mean

  $\frac{SUM(user rates) + k\mu}{n+k} $

  * k : strength of evidence required (to overcome global mean)

* Confidence intervals



##### Demographic and Related approaches

*단순히 인기있는 제품을 추천하는 것에서 좀더 개인화 가능 (나이, 서열, 인정, 위치 등의 인구학적 정보 이용)*. 그렇지만 그들의 인구학적인 기준으로 한 그룹과 동떨어진 사용자에게 적합한 추천을 하기 어려움.. 

* 사용자들을 분류하는데 적합한 기준을 찾아서 해당 변수와 데이터간의 상관성을 찾음 
  * scatter plot그리기, correlation 찾기 
* 해당 변수를 기준으로 나눈 group별로 요약 통계량 내기
* multiple regression model 고려 (선형회귀 / 로지스틱 회귀)



##### Product Association Recommenders

"이 제품을 구매한 사람이 같이 구매한 제품" 추천

* Ephemeral, contextual personalization
* 계산 방식
  * manual cross-sell table (끼워팔기 ,,?)
  * data mining association (자동화/기계화된 끼워팔기)
    * 베이지안 방식 사용 
    * X를 산사람 중에 Y도 같이 산 사람의 비율은 얼마나될까 ?  (X&Y)/X
    * <img src="C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210713040357904.png" alt="image-20210713040357904" style="zoom: 67%;" />
* 예시 
  * 장바구니 방식 (맥주와 기저귀)
  * 뉴스 : 어떤 링크로 오냐에 따라서 어떤걸 볼지가 달라짐 (association links)





Assignmnt #1. Non-personalized and stereotype-based recommeders

https://www.coursera.org/learn/recommender-systems-introduction/supplement/k3j3h/assignment-1-instructions-non-personalized-and-stereotype-based-recommenders

* Non-personalized Summary statistics

  * movie rating data (csv)
  *  output 계산해서 제출 
    * mean rating 
    * number of ratings
    * % positive ratings (4점 이상)
    * product association 
    * lift formula
    * correlation with a selected movie 

* Demographics

  * gender for the users data (sheet 2)

    





ㅅㅂ 뒷부분 날라감,,,, (TF IDF 부분)

