# 통계에서 가설 테스트 마스터하기 가이드

* [출처](https://www.analyticsvidhya.com/blog/2015/09/hypothesis-testing-explained/?utm_source=blog&utm_medium=statistics-t-test-introduction-r-implementation)
* [저자](https://www.analyticsvidhya.com/blog/author/sunil-ray/)
* 구글번역

### 소개 - 사고 방식의 차이
저는 MIS 전문가로서 경력을 쌓기 시작한 후 BI (비즈니스 인텔리전스), 비즈니스 분석, 통계 모델링 및 최근에는 기계 학습으로 나아갔습니다. 이러한 각각의 전환은 데이터를 보는 방법에 대한 생각을 바꾸기 위해 필요했습니다.

그러나이 모든 전환에서 한 가지 예가 돋보입니다. 이것은 내가 관리 대시 보드 및 보고서를 작성하는 BI 전문가로 일할 때였습니다. 근무중인 조직의 내부 구조 변화로 인해 우리 팀은 비즈니스 분석가 팀 (BA)에보고해야했습니다. 당시 비즈니스 분석이 무엇인지, 그리고 BI와 다른 점이 무엇인지에 대해 거의 인식하지 못했습니다.

따라서 일상 업무의 일환으로 아침에 관리 대시 보드를 준비하고 논평을 작성했습니다. 나는 지난 달의 판매량과 작년 같은 달의 판매량을 비교해 보았습니다. 그것은 다음과 같이 보입니다.

![이미지1](https://www.analyticsvidhya.com/wp-content/uploads/2015/09/Sales-report.png)

필자의 논평에서 필자는 판매가 작년과 지난 달보다 좋으며 영업 팀이 최근에 취한 새로운 계획에 박수를 보냈다. 나는 이것이 나의 새로운 매니저에게 보여줄 좋은 일이라고 생각하고 있었다. 조금은 내가 알았습니까, 무엇이 가게에 있었습니까!

### 새 매니저에게 영업 팀에 박수를 보내는 보고서를 보았을 때, 그는 왜이 융기가 데이터의 무작위적인 변화가 아니라고 생각 하느냐고 물었습니다.
나는이 시점에서 통계 배경이 거의 없었기 때문에 그의 입장을 고맙게 생각할 수 없었다.  나는 우리가 2 개의 다른 언어를 말하고 있다고 생각했다. 저의 이전 관리자가이 보고서를 뛰어 넘었을 것이고 고위 경영진에게 메모를 남겼을 것입니다! 그리고 내 새 매니저가 제게 해설을 들으라고했습니다.

오늘 기사에서, 데이터의 노이즈와 신호를 구별하기위한 가설 테스트와 통계적 유의성을 설명 할 것입니다. 정확히 새 매니저가 원했던 것입니다!

추신 : 이 기사는 길다란 기사처럼 보일지 모르지만, 따라하면 가장 유용한 기사 중 하나입니다.

### 사례 연구 :
ABC 학교의 8 학년 학생들의 수학에서 평균 점수가 85 점이라고 가정 해 봅시다. 반면에 30 명의 학생을 무작위로 선택하고 평균 점수를 계산하면 평균은 95 점입니다.이 실험을 통해 결론을 맺을 수있는 것은 무엇입니까? 간단 해. 결론은 다음과 같습니다.

* 이 30 명의 학생들은 ABC 학교의 8 학년 학생들과 다르므로 평균 점수가 더 좋습니다. 즉,이 무작위로 선택된 30 명의 학생 샘플의 행동은 모집단과 다릅니다 (모든 ABC 학교의 8 학년 학생) 또는 이들은 두 개의 다른 인구입니다.

* 전혀 차이가 없습니다. 그 결과는 무작위 기회로 인한 것입니다. 즉 평균 85 점을 얻었습니다. 평균 점수가 85 점 미만이거나 85 점 이상인 학생들이 85 점 이상일 수있었습니다.

어떤 설명이 올바른지 어떻게 결정해야합니까? 이를 결정하는 데 도움이되는 다양한 방법이 있습니다. 다음은 몇 가지 옵션입니다.

* 1. 표본 크기 늘리기
* 2. 다른 샘플 테스트
* 3. 확률 확률 계산

처음 두 가지 방법은 더 많은 시간과 예산이 필요합니다. 따라서 시간이나 예산이 제약 사항 일 때 바람직하지 않습니다.


따라서 그러한 경우 편리한 방법은 해당 샘플에 대한 무작위 확률을 계산하는 것입니다. 즉, 샘플의 평균 점수가 95 일 확률은 얼마입니까? 위에서 주어진 주어진 두 가설에 대한 결론을 이끌어내는 데 도움이 될 것입니다.

![이미지2](https://www.analyticsvidhya.com/wp-content/uploads/2015/09/guide2.jpg)

이제 문제는 " 무작위 확률을 어떻게 계산해야합니까? ".

이에 대한 답을 얻기 위해 먼저 통계에 대한 기본적인 이해를 검토해야합니다.

 ### 통계의 기초
 1 . Z 값 / 표 / p 값 : Z 값은 표준 편차의 측정 값입니다. 즉, 평균값에서 표준 편차가 관측 된 값과 얼마나 차이가 났는지를 나타냅니다. 예를 들어, z 값 = +1.8의 값은 관측 된 값이 평균값에서 +1.8 표준 편차만큼 해석 될 수 있습니다. P 값은 확률입니다. 이 두 통계 용어는 표준 정규 분포와 연관되어 있습니다. [Z-테이블](https://www.stat.ufl.edu/~athienit/Tables/Ztable.pdf)의 각 z 값과 연관된 p 값을 볼 수 있습니다 . 아래는 z 값을 계산하는 공식입니다.
 
 ![이미지3](https://s3-ap-south-1.amazonaws.com/av-blog-media/wp-content/uploads/2015/09/24071007/Z-score-form.png)
 
 여기에서 X는 곡선의 점이고, μ는 모집단의 평균이고 σ는 모집단의 표준 편차입니다.
 
 ![이미지4](https://www.analyticsvidhya.com/wp-content/uploads/2015/09/SND.png)
 
앞에서 설명한 것처럼이 방법은 정규 분포 (위의 그림) 만 사용하며 다른 분포는 사용하지 않습니다. 인구 분포가 정상이 아니라면, 우리는 Central Limit Theorem에 의지 할 것입니다.

2. 중심 극한 정리 (Central Limit Theorem) :  이것은 통계에서 중요한 정리이다. 정의에 빠지지 않고 예제를 사용하여 설명하겠습니다. 아래의 사례를 살펴 보겠습니다. 여기에는 총점이 10 개인 1000 명의 학생들이 있습니다. 다음은이 모집단의 주요 핵심 지표입니다.

그리고 마크의 빈도 분포는 다음과 같습니다
![이미지5](https://www.analyticsvidhya.com/wp-content/uploads/2015/09/FD.png)

아마도 그렇지 않습니다. 이 점수는 모든 학생들에게 무작위로 배분되었습니다.

자,이 인구에서 40 명의 학생 샘플을 가져가 봅시다. 따라서이 인구에서 몇 개의 샘플을 채취 할 수 있습니까? 우리는 25 개 샘플 (1000/40 = 25)을 취할 수 있습니다. 당신은 모든 샘플이 인구 평균 (48.4)과 동일한 평균 점수를 가질 것이라고 말할 수 있습니까? 이상적으로는, 바람직하지만 실제로 모든 샘플은 동일한 평균을 가질 것 같지 않습니다.

여기 우리는 40 명의 학생 1000 표본을 추출했습니다 (무작위로 추출한 표본은 Excel에서 생성되었습니다). 수천 개의 샘플과 다른 통계 메트릭의 이러한 샘플 평균의 빈도 분포를 살펴 보겠습니다. 이 분포는 위에서 살펴본 것과 비슷합니까? 예,이 테이블도 정상적으로 배포됩니다. 더 나은 이해를 위해이 파일을 [여기](https://discuss.analyticsvidhya.com/t/data-set-for-central-limit-theorem/4469/1)에서 다운로드 할 수 있으며이 연습을하는 동안 아래 명시된 결과가 나타납니다.

![이미지6](https://www.analyticsvidhya.com/wp-content/uploads/2015/09/Test_Check_CLT.png)

1. 표본 평균의 평균 (1000 표본 평균)은 모집단 평균에 매우 가깝다

2. 표본 분포의 표준 편차는 모집단 표준 편차를 표본 크기 N의 제곱근으로 나눈 값에서 구할 수 있으며 평균의 표준 오차라고도합니다.

![이미지7](https://www.analyticsvidhya.com/wp-content/uploads/2015/09/central6.png)

3. 표본 평균의 분포는 실제 인구의 분포와 상관없이 정상이다. 이것을 센트럴 리미티드 (Central Limit) 정리라고합니다. 이것은 매우 강력 할 수 있습니다. 초기 ABC 학교 학생의 예에서 표본 평균과 모집단 평균을 비교했습니다. 정확하게, 우리는 표본 평균의 분포를 조사하고 모집단 평균과 표본 평균 사이의 거리를 알아 냈습니다. 이 경우 인구 분포에 대한 걱정없이 정규 분포를 사용할 수 있습니다.

위의 결과를 바탕으로 표준 편차와 평균을 계산하고 z 점수와 p 값을 계산할 수 있습니다. 여기서 무작위 확률은 ABC 학교의 예에서 논의 된 결론 중 하나를 받아들이는 데 도움이 될 것입니다 (위에서 언급 한). 그러나 CLT 정리를 만족하려면 표본 크기가 충분해야합니다 (> = 30).

자, 무작위 확률을 계산했다고합시다. 40 %가 나오면 첫 번째 결론이나 다른 결론을 얻어야하나요? 여기서 " 유의 수준 "은 우리가 결정하는 데 도움이 될 것입니다.

### 유의 수준이란 무엇입니까?
우리는 표본 확률이 평균 95 %가 40 %라는 가정을 취했습니다. 즉, 이는 행동 차이로 인한 것이 아니고 무작위로 인해 발생했을 가능성이 더 높다고 말할 수 있습니다.

확률이 7 % 였다면, 그것이 임의성 때문이 아니라는 것을 추측하는 것은 당연한 생각이었을 것입니다. 확률이 비교적 낮다는 것은 높은 확률은 무작위성의 수용으로 이어지고 낮은 확률은 행동의 차이로 이어지기 때문에 약간의 행동 차이가있을 수 있습니다.

자, 우리는 어떻게 높은 확률과 낮은 확률을 결정합니까?

솔직히 말해서, 그것은 본질적으로 매우 주관적입니다. 90 %가 높은 확률로 간주되고 다른 시나리오에서는 99 % 일 수있는 비즈니스 시나리오가있을 수 있습니다. 일반적으로 모든 도메인에서 5 %를 잘라냅니다. 이 5 % 를 α 수준 (α로 표시됨) 이라고도하는 **중요도** 라고 합니다. 즉, 무작위 확률이 5 % 미만이면 두 개의 다른 모집단의 행동에 차이가 있다고 결론 내릴 수 있습니다. (1- 유의 수준)은 또한 **신뢰 수준**으로 알려져 있습니다. 즉, 나는 그것이 임의성에 의해 유도되지 않는다고 95 % 확신한다고 말할 수 있습니다.

![이미지8](https://www.analyticsvidhya.com/wp-content/uploads/2015/09/images.png)

지금까지는 표본 평균이 인구와 다른지 아니면 무작위 적 기회인지에 따라 가설을 검증하는 도구를 살펴 보았습니다. 이제 가설 테스트를 수행하는 단계를 살펴보고 예제를 사용하여 테스트 해 보겠습니다.

### 가설 테스트를 수행하는 단계는 무엇입니까?
* **가설 설정 (NULL 및 대체)** :   ABC 학교 사례에서 우리는 실제로 가설을 테스트했습니다. 우리가 테스트하는 가설은 표본과 모집단 평균의 차이가 무작위 적 기회 때문이었습니다. 그것은 **"NULL 가설"** 로 불린다. 즉 표본과 모집단간에 차이가 없다. 귀무 가설의 기호는 'H0'입니다. 귀무 가설을 테스트하는 유일한 이유는 그것이 잘못되었다고 생각하기 때문입니다. 우리는 **대체가설** 에서 귀무 가설에 대한 잘못된 생각을 기술한다 .ABC School의 예에서, 대체 가설은 표본과 집단의 행동에 중요한 차이가 있다는 것입니다. 대안 가설의 상징은 'H1'이다. 법정에서 피고인은 무죄로 간주되므로 (이는 귀무 가설), 피고인이 무죄가 아니라는 증거를 보여주는 시험을 검사에게 맡기는 것이 부담입니다. 마찬가지로 귀무 가설이 사실이라고 가정하고 연구자가 귀무 가설이 사실 일 가능성이 거의 없음을 증명하는 연구를 수행하도록합니다.

* **결정 기준 설정 : 결정 기준**  을 설정하기 위해 테스트의 중요도를 명시합니다. 그것은 5 %, 1 % 또는 0.5 %가 될 수 있습니다. 유의 수준에 따라 Null 또는 Alternate 가설을 수락 할 것인지 결정합니다. 유의 수준이 1 % 인 경우 Null 가설을 허용하는 확률은 0.03 일 수 있지만 중요도가 5 % 인 경우 Null 가설을 거부합니다. 비즈니스 요구 사항을 기반으로합니다.

* **임의 확률 확률 계산 :** 확률 확률 / 테스트 통계는 확률을 결정하는 데 도움이됩니다. 확률이 높을수록 Null 가설을 수용 할 수있는 충분한 가능성과 충분한 증거가 있습니다.

* **의사 결정 :** 여기서 p 값과 미리 정의 된 유의 수준을 비교하고 유의 수준보다 작 으면 우리는 그것을 받아들이는 Null 가설을 거부합니다. 귀무 가설을 유지하거나 거부하기로 결정하는 동안 우리는 전체 인구가 아닌 표본을 관찰하기 때문에 잘못 될 수 있습니다. 귀무 가설에 대한 결정의 진실성과 허위에 관한 4 가지 결정 대안이 있습니다.
1.귀무 가설을 보유하기로 한 결정은 정확할 수 있습니다. 
2. 귀무 가설을 유지하겠다는 결정은 틀릴 수 있으며, **유형 II 오류로 알려져** 있습니다. 
3. 귀무 가설을 기각하는 결정은 정확할 수 있습니다. 
4. 귀무 가설을 기각하기로 한 결정은 정확하지 않을 수 있습니다. **유형 I 오류**.
 
 ### 예
 비만 환자의 혈당 수치는 평균이 15이며 표준 편차는 15입니다. 연구원은 원시 옥수수가 높은 식단은 혈당 수준에 긍정적 인 영향을 미칠 것이라고 생각합니다. 원료 옥수수 전분을 먹은 36 명 환자의 평균 포도당 수준은 108입니다. 원시 옥수수 전분이 효과가 있는지 없는지에 대한 가설을 테스트하십시오.
 
**해결책 :** -  위의 단계를 수행하여이 가설을 테스트합니다.

1 단계 : 가설을 진술하십시오. 모집단 평균은 100입니다.

H0 : μ = 100 
H1 : μ> 100

2 단계 : 유의 수준을 설정합니다. 문제에서는 주어지지 않았으므로 5 % (0.05)로 가정합시다.

3 단계 : z 점수와 z 표를 사용하여 무작위 확률을 계산합니다.

![이미지9](https://s3-ap-south-1.amazonaws.com/av-blog-media/wp-content/uploads/2015/09/24071007/Z-score-form.png)

이 데이터 세트의 경우 : z = (108-100) / (15 / √36) = 3.20

확률을 볼 때 Z- 테이블을보고 3.20과 관련된 p- 값은 0.9993입니다. 즉, 108 미만의 확률은 0.9993이고 108 이상은 (1-0.9993) = 0.0007입니다.

4 단계 : 0.05 미만이므로 Null 가설을 거부합니다. 즉 옥수수 전분 효과가 있습니다.

참고 : 유의 수준 설정은 임계 값이라는 z 값을 사용하여 수행 할 수도 있습니다. 5 % 확률의 Z 값을 알아 내면 1.65 (모든 방향에서 양수 또는 음수)입니다. 이제 계산 된 z 값과 임계 값을 비교하여 결정을 내릴 수 있습니다.

### 방향성/비 방향성 가설 검정 
이전 예제에서 우리의 Null 가설은 평균이 100이고 다른 가설이 표본 평균이 100보다 큰 차이가 없음을 의미합니다. 그러나 표본 평균이 100이 아니므로 대체 가설을 설정할 수도 있습니다. 우리는 다른 가설과 함께 가야 할 Null 가설을 거부한다.
* 표본 평균이 100보다 큼
* 표본 평균은 100과 같지 않습니다. 즉 차이가 있습니다.

여기서 질문은 "어떤 다른 가설이 더 적합합니까?"입니다. 대체 가설이 적합한 지 결정하는 데 도움이되는 특정 요령이 있습니다.
* 100 이하의 표본 평균을 테스트하는 데 관심이 없다면 더 큰 값만 테스트하고 싶을 것입니다.
* 당신은 강한 원료 옥수수의 영향이 더 크다고 믿습니다.

위의 두 가지 경우에는 One tail test를 사용합니다. 하나의 꼬리 시험에서, 우리의 대체 가설은 관찰 된 평균보다 크거나 작기 때문에 그것은 방향성 가설 검정으로 알려져있다. 반면에, 당신은 우리가 이동 한 후 시험의 영향이 크거나 낮은 지 여부 모르는 경우 두 꼬리 시험 이라고도 비 방향성 가설 테스트 .

연구 조직 중 하나가 새로운 교수법을 생각해 내고 있다고 가정 해 봅시다. 그들은이 방법의 영향을 테스트하려고합니다. 그러나 그들은 긍정적 또는 부정적 영향을 미친다는 것을 알지 못합니다. 그런 경우 두 꼬리 테스트를해야합니다.

하나의 꼬리 테스트에서 표본 평균이 양수이거나 음의 극단적 인 경우 Null 가설을 거부합니다. 그러나 두 꼬리 테스트의 경우 우리는 임의의 방향 (양수 또는 음수)으로 Null 가설을 거부 할 수 있습니다.

![이미지10](https://www.analyticsvidhya.com/wp-content/uploads/2015/09/Tailed_Test-850x590.jpg)

위의 이미지를보십시오. 양방향 테스트는 한쪽 방향으로 통계적 유의성을 테스트하고 다른 쪽 방향으로는 알파의 절반을 테스트하도록 알파의 절반을 할당합니다. 이것은 .025가 테스트 통계 분포의 각 꼬리에 있음을 의미합니다. 정규 분포가 대칭이므로 양쪽에서 0.025을 말하는 이유는 무엇입니까? 이제 우리는 두 꼬리 테스트에서 Null 가설에 대한 거절 기준이 0.025이고 이것이 0.05보다 낮다는 결론을 내린다. 즉, 두 꼬리 테스트는 Null 가설을 거부하는 더 엄격한 기준을 갖는다.

### 예
Templer and Tomeo (2002)는 1994 년과 1997 년 사이에 시험을 보는 학생들을 대상으로 Graduate Record Examination (GRE) General Test의 정량적 부분에 대한 인구 평균 점수가 558 ± 139 (μ ± σ)라고보고했습니다. 100 명의 참가자 (n = 100) 의 표본을 선택한다고 가정 합니다. 585 ( M = 585)와 같은 표본 평균을 기록합니다 . 유의 수준 0.05 (α = .05)에서 귀무 가설 (μ = 558)을 유지할지 여부를 p 값 t0로 계산하십시오.

### 해결책:

1 단계 : 가설을 진술하십시오. 인구 평균은 558입니다.

H0 : μ = 558 
H1 : μ ≠ 558 (두 꼬리 시험)

2 단계 : 유의 수준을 설정합니다. 질문에서 언급 한 바와 같이, 5 % (0.05)로 나타납니다. 방향이없는 양측 테스트에서는 알파 값을 반으로 나누어 면적의 동일한 비율을 위아래 꼬리에 배치합니다. 따라서 양 측면의 유의 수준은 다음과 같이 계산됩니다. α / 2 = 0.025. 이와 관련된 z 점수 (1-0.025 = 0.975)는 1.96입니다. 이것은 양측 테스트이므로 -1.96보다 작거나 1.96보다 큰 z- 점수 (관찰 됨)는 Null 가설을 거부하는 증거입니다.

3 단계 : 임의 확률 확률 또는 z 점수 계산

![이미지11](https://s3-ap-south-1.amazonaws.com/av-blog-media/wp-content/uploads/2015/09/24071007/Z-score-form.png)

이 데이터 세트의 경우 : z = (585-558) / (139 / √100) = 1.94

Z- 테이블을보고 확률을 볼 수 있고 1.94와 관련된 p 값은 0.9738입니다. 즉, 585보다 작은 값을 가질 확률은 0.9738이고 585보다 크거나 같을 확률은 (1-0.9738) = 0.03입니다.

Step-4 : 여기서 결정을 내리기 위해 얻어진 z 값을 임계 값 (+/- 1.96)과 비교합니다. 획득 된 값이 임계 값을 초과하면 귀무 가설을 기각합니다. 여기서 얻은 값 (Z obt = 1.94)은 임계 값보다 작습니다. 그것은 거절 지역에 속하지 않습니다. 결정은 귀무 가설을 유지하는 것입니다.

![이미지12](https://www.analyticsvidhya.com/wp-content/uploads/2015/09/error4.png)

### 엔드 노트
이 기사에서는 예측 모델링 중에 가설 테스트를 수행하는 완전한 프로세스를 살펴 보았습니다. 처음에 우리는 가설의 유형과 가설 유형 및 정보에 입각 한 결정을 내리는 가설을 검증하는 방법을 살펴 보았다. 또한 Z- 값, Z- 테이블, P- 값, 센트럴 한도 정리와 같은 가설 테스트의 중요한 개념을 살펴 보았습니다.

소개에서 언급했듯이, 이것은 내가 처음이 글을 읽었을 때 저에게있어 가장 어려운 마인드 변화 중 하나였습니다. 그러나 그것은 또한 가장 유익하고 중요한 변화 중 하나였습니다. 나는이 변화가 예측 모델러처럼 생각하기 시작했다고 쉽게 말할 수 있습니다.

다음 기사에서는 가설 테스트와 같은 what-if 시나리오를 살펴볼 것입니다.

샘플 크기가 30보다 작 으면 (CLT를 만족하지 않음)
표본 및 모집단보다는 두 표본을 비교하십시오.
우리가 인구 표준 편차를 모르는 경우
빅 데이터 시대의 p- 값과 Z- 점수
이 기사가 도움이 되었습니까? 아래 의견란에 의견이나 생각을 말씀해주십시오.
