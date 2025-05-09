# 정보처리기사사 (필기)과목1~3 암기사항

## 1과목

### 소프트웨어 설계
> #### 소프트웨어 생명주기
= 개발 단계의 진행 방식(프로세스 모델)

+ **폭포수 모형**: 한 단계가 완전히 끝나야만 다음 단계로 넘어가는 개발 방법론
+ **프로토타입 모형**: 사용자가 요구사항을 정확히 파악하기 위해 실제 개발될 소프트웨어에 대한 견본품을 만들어 최종 결과물을 예측
+ **나선형 모형(점진적 모형)**: 계획→위험분석→개발→평가의 반복, 점진적으로 최종 소프트웨어를 개발하는 것
+ **애자일 모형**: 고객의 요구사항 변화에 유연하게 대응할 수 있도록 일정한 주기를 반복하면서 개발과정을 진행 <br>
스크럼, XP(eXtreme Programming), 칸반, Lean, 크리스탈, DSDM(Dynamic System Development Method) 등이 있다.

> #### XP(eXtreme Programming)기법
5가지 핵심 가치: 의사소통(Communication), 단순성(Simplicity), 용기(Courage), 존중(Respect), 피드백(Feedback)

#### XP 주요실천 방법
+ **Pair Programming(짝 프로그래밍)**: 다른 사람과 함께 프로그래밍을 수행, 개발에 대한 책임을 공동으로 나눠 가짐
+ **Collective Ownership(공동 코드 소유)**: 개발 코드에 대한 권한과 책임을 공동으로 소유
+ **Test-Driven Development(테스트 주도 개발)**
+ **Whole Team(전체 팀)**

> #### 요구사항 정의
크게 기능 요구사항, 비기능 요구사항으로 나뉨

> #### 요구사항 분석

+ **자료 흐음도(DFD)**: 요구사항 분석에서 자료의 흐름 및 변환 과정과 기능을 도형 중심으로 기술하는 방법 <br>
프로세스→원, 자료흐름→화살표, 자료저장소→평행선, 단말→사각형 <br>
입력 화살표가 있다고 반드시 출력화살표가 있어야 하는 것은 아니다.
+ **자료 사전**: 자료 흐름도에 있는 자료를 더 자세히 정의하고 기록한 것
    + 자료의 정의: =
    + 자료의 연결: +
    + 자료의 생략: ()
    + 자료의 선택(or): [|]
    + 자료의 반복: {}
    + 자료의 설명(주석): * *

> #### 요구사항 분석 CASE(자동화 도구)

+ **SADT(Structure Analysis and Design Technique)**: SoftTech 사에서 개발
+ **SREM(Software Requirements Engineering Methodology)**
    + RSL(Requirement Statement Language)
    + REVS(Requirement Engineering and Validation System)
+ **PSL(Problem Statement Language)/PSA(Problem Statement Analyzer)**
+ **TAGS(Technology for Automated Generation of System)**

#### HIPO(Hierarchy Input Process Output)
시스템의 분석 및 설계나 문서화할 때 사용되는 기법, 하향식 소프트웨어 개발을 위한 문서화 도구이다.

#### HIPO Chart
+ **가시적 도표**: 시스템의 전체적인 흐름
+ **총체적 도표(Overview Diagram)**: 프로그램을 구성하는 기능을 기술한 것으로 입력, 처리, 출력에 대한 전반적인 정보를 제공하는 도표
+ **세부적 도표(Detail Diagram)**: 총체적 도표의 상세 도표표

> #### UML(Unified Modeling Language)
구성 요소: 사물, 관계, 다이어그램 등 <br>
만약 UML에서 표현하는 기본 기능 외에 추가적인 기능을 표현하고 싶으면, 스테레오 타입(겹화살괄호: <<>>)을 사용한다.

#### 다이어그램
구조적(정적) 다이어그램과 행위(동적) 다이어그램으로 나뉘어짐 <br>
행위 다이어그램 중 상태 다이어그램에서 객체 전이의 요인이 되는 요소를 event라고 한다.

> #### 주요 UML 다이어그램

#### 유스케이스 다이어그램
개발될 시스템을 이용해 수행할 수 있는 기능을 사용자의 관점에서 표현한 것 <br>
유스케이스 사용 시 특별한 조건이 만족할 경우에만 수행하는 유스케이스를 확장이라고 한다.

#### 순차 다이어그램
시간의 흐름에 따라 상호 작용하는 과정을 액터, 객체, 메시지 등의 요소를 사용하여 그림으로 표현한 것 <br>
순차 다이어그램의 구성 요소 중 액터(Actor)는 시스템으로부터 서비스를 요청하는 외부요소이다.
