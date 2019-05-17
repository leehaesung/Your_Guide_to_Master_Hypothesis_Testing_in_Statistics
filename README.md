# 통계에서 가설 테스트 마스터하기 가이드

* [출처](https://www.analyticsvidhya.com/blog/2015/09/hypothesis-testing-explained/?utm_source=blog&utm_medium=statistics-t-test-introduction-r-implementation)
* [저자](https://www.analyticsvidhya.com/blog/author/sunil-ray/)

### 소개 - 사고 방식의 차이
저는 MIS 전문가로서 경력을 쌓기 시작한 후 BI (비즈니스 인텔리전스), 비즈니스 분석, 통계 모델링 및 최근에는 기계 학습으로 나아갔습니다. 이러한 각각의 전환은 데이터를 보는 방법에 대한 생각을 바꾸기 위해 필요했습니다.

그러나이 모든 전환에서 한 가지 예가 돋보입니다. 이것은 내가 관리 대시 보드 및 보고서를 작성하는 BI 전문가로 일할 때였습니다. 근무중인 조직의 내부 구조 변화로 인해 우리 팀은 비즈니스 분석가 팀 (BA)에보고해야했습니다. 당시 비즈니스 분석이 무엇인지, 그리고 BI와 다른 점이 무엇인지에 대해 거의 인식하지 못했습니다.

따라서 일상 업무의 일환으로 아침에 관리 대시 보드를 준비하고 논평을 작성했습니다. 나는 지난 달의 판매량과 작년 같은 달의 판매량을 비교해 보았습니다. 그것은 다음과 같이 보입니다.

![이미지1](https://www.analyticsvidhya.com/wp-content/uploads/2015/09/Sales-report.png)

필자의 논평에서 필자는 판매가 작년과 지난 달보다 좋으며 영업 팀이 최근에 취한 새로운 계획에 박수를 보냈다. 나는 이것이 나의 새로운 매니저에게 보여줄 좋은 일이라고 생각하고 있었다. 조금은 내가 알았습니까, 무엇이 가게에 있었습니까!

