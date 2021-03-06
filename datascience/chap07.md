# chapter7. 데이터에서 의미 추출하기

## 7.1 윌리엄 커키어스키
- 특징추출 : 원시데이터를 조심스레 다루는 것과 관계있음
- 특징선택 : 모형과 알고리즘의 예측변수 혹은 변수가 되는 데이터의 부분집합이나 데이터의 함수를 구성하는 과정

#### 7.1.1 배경 : 데이터과학 경진대회

- 데이터과학 경진대회는 데이터과학 생태계의 일부로, 데이터과학 영역에 존재하는 문화적 힘의 하나
- 경진대회 개최는 데이터과학을 체계화하고 그것의 범위를 정하는데 도움을 줌
- 데이터과학 경진대회는 복잡한 요소들을 제거(적절한 문제 제기, 데이터 수집, 시각화, 커뮤니케이션등)
- 여성 데이터과학자들이 경쟁에 참여하지 않으므로 데이터과학 재능의 척도로 간주하기 전 고려해야 됨 

#### 7.1.2 배경 : 크라우드소싱

- 크라우드소싱 모형 : 분산적인 모형(위키피디아), 특이하고 어려운 문제들(캐글등)
- 유용성에 영향을 주는 이슈 : 신뢰할만한 평가척도 부재, 조직 요인들(저질의 문제와 빈약한 상금, 너무 크거나 작은 문제등)
- 역사적 맥락 
  * 1714년 영국 왈립 해군의 경도측정
  * 2002년 폭스 노래 경연대회인 '아메리칸 아이돌'
  * X-프라이즈재단 상금걸린 경진대회 개최
- 크라우드 소싱(crowdsourcing) : 도전과제가 제시되고, 참가자들의 최적의 해결책을 찾기 위해 상호독립적으로 경쟁
- 메커니컬 터크(Mechanical Turk) : 사람들에게 과업이 주어지는 온라인 크라우드소싱 서비스
 * 인간이 기계를 좁기 위해 상당히 반복적인 일을 처리하고, 그런 다음 그 기계가 인간을 돕기 위해 업무를 자동화 하는 것

## 7.2 캐글모형

- 캐글(Kaggle) : '우리는 데이터과학을 스포츠로 만든다'라는 구호를 가진 기업
- 캐글을 수수료를 받고 데이터 문제를 크라우드소싱으로 해결하기 원하는 기업을 위해 경진대회를 개최

#### 7.2.1 단독참가자

- 캐글 경진대회는 훈련데이터세트와 검증세트가 주어짐
- 참가자가 접근할 수 없는 제3의 유보 데이터세트가 있음
- 유보 데이터세트에 대한 참가자들의 현재 평가척도를 보여주고 참가자들 사이 '뛰어넘기'를 통해 경쟁심 자극
- 경진대회 기간이 너무 길어지면 복잡한 모형을 낳고, 이는 실제 현실에서 쓰이기 힘들었음(넷플릭스의 예)

#### 7.2.2 캐글의 고객

- 캐글이 채우는 공백 : 분석이 필요한 사람들과 분석 능력을 가진 사람들 사이의 불일치
- 캐글의 혁신 : 기업을 설득해 그들이 소유한 데이터를 공유하도록 유도하고, 수만명의 캐글 데이터과학자에게 크라우드소싱하여 커다란 데이터 문제가 그 기업들에 도움이 되도록 풀어준다는 점


## 7.3 사고 실험: 로봇 평가자의 윤리적 함축성은 무엇인가?

- 인간채점자들이 항상 공정한 것은 아니다
- 기계는 상황을 구조화하고, 이것은 창의성을 억제하는가?
- 테스트의 목적은 훌륭한 어세이를 쓰는 것인가 아니면 표준화된 시험을 잘 보는 것인가?
- 능력있는 참가자와 서투른 참가자의 차이
 * 알고리즘에 넣은 정보. 데이터에서 무엇을 추출해야 할지 결정해야 함
 * 랜덤 포리스트 같은 알고리즘은 말도 되지 않는 구상들을 얼마든지 던져 넣을 수 있고, 어떤 구상들이 작동하는지 알아냄

## 7.4 특징 선택

- 특징 선택(feature selection)은 모형에 넣기를 원하는 데이터 혹은 변환 데이터의 부분집합을 확인하는 작업
- 원시 데이터는 중복되거나 상관이 높은 변수가 많을 수 있음. 모형에 포함시키기 전 변수를 변환하거나 바꾸는 것이 필요할 수 있음
- 거대한 데이터세트가 항상 도움이 되는 것은 아님
- 예측 모형의 성과를 향상시키기 위해 특징 선택 과정을 개선하길 원함

#### 7.4.1 사례: 사용자 유지

- 체이싱 드래곤을 통한 신규 사용자들이 되돌아 올 것인지 예측하는 모형 구축. 사용자 유지 상황	
- 특징생성시 특징들간에 중복과 연관성이 있다는 것에 주의!
- 특징 생성 혹은 특징 추출시 상상력을 이용하는 것도 좋음
- 이자벨 구용의 논문 : 변수와 특징 선택 입문
 * 좋은 예측변수를 만드는 데 유용한 특징들의 부분집합을 선택하고 구성하는 데 초점

#### 7.4.2 필터

- 결과변수와의 상관계수 또는 통계량이나 척도에 기반한 순위에 따라 가능한 특징들을 정렬
- 중복에 관심을 두지 않음
- 특징을 상호 독립적인 것처럼 다룸으로써 가능한 상호작용들을 고려하지 못함

#### 7.4.3 래퍼

- 어떤 고정된 크기의 특징들의 부분집합을 찾음
- 특징을 선택하기 위해 사용하는 알고리즘 선택, 특징들의 집합을 '좋다'고 판단하기 위한 필터나 선택기준 결정
 * 알고리즘 선택 : 단계적 회귀(전진 선택, 후진 제거, 혼합형 접근)
 * 전진 선택 : 한번에 한개의 특징 선택, 선택 기준이 더 이상 향상되지 않고 나빠질 때 특징 추가 중단
 * 후진 제거 : 어떤 특징의 제거가 선택 기준을 나빠지게 할 때 특징 제거 중단
 * 혼합형 접근 : 최소 중복과 최대 적합의 징후는 포착
 * 선택은 다소 임의적이 될 수 밖에 없음
 * 무엇을 위해 왜 최적화하는 지 결정하는 것은 우리가 수행하는 일의 일부임
 * 내 곁에 도움을 주는 해당 분야 전문가가 있다면 그 전문가와 완전히 친해지기 전까지는 특징 선택의 기계학습 미로에 들어가지 마라!

#### 7.4.4 의사결정나무

- 큰 결정을 일련의 질문으로 분해
- 큰 장점은 해석가능성
- 데이터 맥락에서 보면 분류 알고리즘임
- 가장 유용한 정보를 주는 것을 우선적으로 취해야 함

#### 7.4.5 엔트로피

- 가장 '유용한 정보를 주는' 특징이 무엇인지 정량화하기 위해 X에 대해 엔트로피(무엇이 얼마나 혼합되어 있는지에 대한 척도)를 정의
- 우리가 a를 알 때 X에 대해 얼마나 많은 정보를 알게 되는가? 혹은 우리가 엔트로피를 얼마나 잃게 되는가?

#### 7.4.6 의사결정나무 알고리즘

- 종종 과적합을 방지하기 위해 사후에 '가지치기'를 하고, 이것은 특정한 깊이 아래를 잘라냄을 의미

#### 7.4.7 의사결정나무에서 연속변수 다루기
#### 7.4.8 랜덤 포리스트

- 일반적으로 가지치기를 원하지 않는다. 왜냐하면 랜덤 포리스트의 큰 장점은 그것이 특이한 잡음을 포함할 수 있다는 것이기 때문임

#### 7.4.9 사용자 유지: 해석가능성 대 예측력

- 모든 변수를 가지고 특징선택을 수행하기 원하겠지만, 사용자 속성에 따른 조건부로 내가 무엇인가 할 수 있는 변수들에 초점을 맞춰야 함

#### 참고URL

- http://ai.xprize.org/
- https://www.kaggle.com/c/facebook-recruiting-iii-keyword-extraction
- https://www.kaggle.com/c/facebook-recruiting-iii-keyword-extraction
- https://inclass.kaggle.com/c/movie/data
- http://jmlr.org/papers/volume3/guyon03a/guyon03a.pdf