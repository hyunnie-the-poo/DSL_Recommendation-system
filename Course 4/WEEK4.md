### Course4. Matrix Factorization and Advanced techniques



###### 행렬분해와 차원축소  (Matrix Factorization & Dimension Reduction)

* 차원축소의 필요성
  * 너무 많은 user / product  => million x million 행렬 생성 => OVERFIT  (highly detailed)
  * problems of synonymy
  * computational complexity
* 행렬분해 
  * example : $A = U\sum{V^T}$
    * r x r diagonal matrix인 행렬의 요소들은 해당 dimension의 중요도를나타냄. 
    * 중요도 순으로 내림차순으로 정리한 후 top k개의 dimension을 사용하게 되면 원하는 개수의 taste dimension으로 행렬을 분해할 할 수 있음.
    * 적절한 k의 개수는 경험적으로 결정해야함. 
  * taste dimension
    * 특징1. mathematically orthogonal / optimal 
    * 특징2. not particularly user-understandable
  * challenges
    * 결측치 처리
    * computational complexity
    * 낮은 설명력 



###### 방식 1. SVD ; Single Value Decomposition (특이값 분해)

* definition

   $R = P\sum{Q^T}$

  ![image-20210807155141618](C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210807155141618.png)

  * R : user - item rating matrix 
  * P : user-feature affinity matrix (features에 대한 유저들의 선호도)
  * $\sum$ : 각 feature에 대한 weight matrix (각 feature들의 중요도)
  * Q : item-feature relevance matrix (features와 아이템들의 관련성) 
  *  P와 Q는 orthogonal matrix 
  * feature weight matrix는 diagonal matrix

* latent features (잠재적인 특성)

  * SVD는 선호도를 latent features를 기준으로 설명함. 
  * latent features는 rating data를 통해 추출된 기준. 
  * 예측력에 대해서 최적화 되어있는 기준이며, 설명력이 약해서 설명하기 어려움. 
  * 분해하게 되면 user preference vector와 item relevance vector를 같은 공간 (feature space) 에서 표현할 수 있음. 

* challenge

  * computational cost
  * missing value 채우기
    * SVD는 matrix가 complete하다고 가정하기 때문에 sparse 한 rating matrix를 채워줘야함. 
    * 방식
      * mean값으로 채우기
      * matrix를 normalize한다음 0으로 채우기 
      * 무시하기  (by *gradient descent techniques*)
  * 신규 유저의 정보가 업데이트 되었을 때 
    * 처음부터 다시 계산하는 것은 너무 비효율적. 
    * folding in 방식 사용 (??)

  

###### Gradient Descent Techniques

* 결측치를 무시하고 처리하는 방식 

* 기본 아이디어 : decomposition과 이를 통한 prediction을 guess => 실제 user rating과의 error계산 => error를 최소화 하는 방향으로 이동 

* simplifying error

  * SSE를 고려 (SSE를 최소화하는 값은 RMSE도 최소화함 )

* simplified SVD

  * $R = B + PQ^T$
  * $S(u,i) = b_{ui} + \sum_{f} {p_{ui}q_{if}}$ 

* funk SVD : feature 하나씩 train

* 알고리즘 

  1. initialize values to train (item/user feature vector)

     => non zero, random

  2. 해당 item/user feature vector로 rating 예측
  3. 실제값과 비교하여 SSE계산
  4. SSE를 활용해 update rule을 따라 값을 업데이트
     *  $\Delta q_{if} = \lambda(\epsilon_{ui}p_{ui}-\gamma q_{if})$
     *  $\Delta p_{uf} = \lambda(\epsilon_{ui}q_{if}-\gamma p_{uf})$
     * $\lambda $ : learning rate
     * $\gamma$ : regularization factor
  5. 더이상 변하지 않을 때까지 (수렴할때까지) 반복 

* gradient descent 방식을 사용하면 R,Q가 더이상 orthogonal 하지 않게 됨 => SVD (X)
  * 그렇지만 작은 learning rate를 사용하면 orthogonal할 수 있고, folding in 방식이 가능할 수 있음 ?  (on folding- in with gradient descent)



###### https://towardsdatascience.com/funk-svd-hands-on-experience-on-starbucks-data-set-f3e0946da014#9e22



###### 방식 2. Probability Matrix Factorization

* 기본 아이디어 : 데이터가 랜덤한 방식으로 생성된다고 가정하고 파라미터를 조정

* Popularity model  

  * non personalized) P(i) =  # of buys of i / # total buys   (i를 살 확률)
  * personlized) P(i|u)   => 구할 수 없음  (특정 유저가 i를 살 확률)
  * probabilistic LSA  : $P(i|u) = \sum_z {P(i|z)P(z|u)} $ 
    * z  : particular random topic (latent)
    * 특정 유저가 특정 랜덤 토픽 z를 고를 확률 x 해당 토픽에서 아이템 i가 뽑힐 확률
    * SVD와 같은 결과가 나옴
      * $R = PQ^T$
      * P,Q 는 orthogonal 하지 않음 (분포를 표현하고 있기 때문
    * 단점 : 느림

* PMF :

  * model rating이 정규분포를 따른다고 가정한 상태에서 진행

    * $r_{ui}$ ~ $N(p_uq_i^T, \sigma ^2 )$

  * PLSI보다 빠름 

    



Quiz

1. (T/F) 행렬 분해를 하게 되면 중요한 taste dimension을 선정하여 추천을 하게 되기 때문에 더이상 Collaborative Filtering이 아닌 Content-Based가 된다. 

   => F (content based에서 활용되는 item의 특징들은 matrix에서 오는 것이 아니라, 수집할 수 있는 metadata이다. 행렬분해를 해서 만들어지는 taste dimension은 그런 데이터가 아니라 community of users & their rating 에서 오기 때문에 Content-based라고 할 수 없고, CF이다. )

2. SVD를 통해 추출되는 latent feature는 [       ]에 대해서 최적화 되어있으며, [        ]이 약하다. 

   보기 )  예측력, 설명력

   => 예측력, 설명력 

3. 결측치를 무시하고 dimension reduction 을 하기 위한 방식은 ? 

   => gradient descent technique

