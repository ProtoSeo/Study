# 3. **프로젝트 관리**

## **프로젝트 관리**
프로젝트를 계획하고 수행하는데 필요한 모든 작업 관리
- 필요한 작업의 결정
- 프로젝트에 적합한 인력 확보
- 직무 정의
- 일정 계획
- 작업 준비
- 비용 예측
- 지시, 감독
- 기술적 리더
- 다른 사람이 내린 결정을 검토하고 승인
- 팀원 사기 진작과 지원
- 모니터링, 통제
- 다른 프로젝트 관리자와 협력
- 보고
- 프로세스 개선

## **프로젝트 관리 활동**
- 제안서 작성
- 프로젝트 계획 및 스케쥴링
- 프로젝트 비용 계획(개발 노력 추정)
- 인력 선발 및 평가
- 보고서 작성 및 프로젠테이션

## **프로젝트 계획**
프로젝트 계획 수립
- 시간이 많이 걸리는 프로젝트 관리 활동
- 초기 개념에서 시스템 제공에 이르기까지 지속되는 활동
- 새로운 정보를 이용할 수 있을 떄마다 계획을 정기적으로 수정해야한다
- 주요 소프트웨어 프로젝트 계획을 지원하기 위한 다양한 종류의 계획이 있다


|Plan|Description|
|---|---|
Quality Plan|Describes the quality procedures and standards that will be used in a project|
Validation Plan|Describes the approach, resources and schedule used for system validation|
Configuration management plan|Describes the configuration management procedures and structures to be used|
Maintenance plan|Predicts the maintenance requirements of the system, maintenance costs and effor required|
Staff development plan|Describes how the skills and experience of the project team members will be developed|

## **프로젝트 개발 노력 추정**

프로젝트 개발노력 추정시 고려사항

구분|항목|내용
|---|---|---|
||문제의 복잡도||난이도, 유형
|프로젝트 요소|시스템의 크기|입/출력 양식의 수
||시스템 신뢰도|정확성, 견고성, 완전성, 일관성
|
||인적 자원|관리자, 개발자, 지원체계
|자원 요소|H/W 자원|개발 장비, 운영 장비
||S/W 자원|개발지원 도구
|
생산성 요소|개발자 능력|경험, 전문지식 확보 정도|
||개발 방법론|최신 기법, 관리 방법론

## **기능 점수(Function Point) 모델**
- 논리적 설계를 기초로 사용자에게 제공되는 소프트웨어의 기능을 정량화하여 소프트웨어 규모 산정
- 데이터 관리 위주의 소프트웨어에 적합
- 조직에 관계없이 애플리케이션 복잡도 계산의 일관성 제공
- 프로그래밍 언어와 무관하게 객관적인 복잡도 산정이 가능

> 기능 점수 산정 절차
> <img src="Activityprocess1.png" alt="Activityprocess" >

1. 기능점수 모델:UTF(Unadjusted Function Point) 산정
- 다섯 가지 소프트웨어 구성 요소
    * 외부 입력(external input)
        + 시스템 외부에서 들어오는 데이터 및 제어 정보를 처리하는 기본 프로세스
    * 외부 출력(external output)
        + 시스템 밖으로 데이터나 제어 정보를 보내는 기본 프로세스
    * 외부 질의(external inquiry)
        + 데이터나 제어 정보의 조회를 통해 사용자에게 정보를 제공하는 프로세스
    * 내부 논리 파일(internal logical file)
        + 시스템 영역 안에서 유지되고 사용자가 식별 가능한 논리적으로 연관된 자료 및 제어 정보 그룹
    * 외부 인터페이스 파일(external interface file)
        + 시스템에서 참조되지만 다른 응용 시스템에서 유지되며 사용자가 식별 가능한 논리적으로 연관된 자료 및 정보 그룹

<br>

- 소프트웨서 구성 요소별 기능 점수 가중치
<table>
    <thead>
        <tr>
            <th rowspan="2">SW 요소</th>
            <th colspan="3">복잡도</th>
        </tr>
        <tr>
            <th>단순</th>
            <th>평균</th>
            <th>복잡</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>입력(트랜잭션)</td>
            <td>3</td>
            <td>4</td>
            <td>6</td>
        </tr>
        <tr>
            <td>출력(화면 및 출력양식)</td>
            <td>4</td>
            <td>5</td>
            <td>7</td>
        </tr>
        <tr>
            <td>질의</td>
            <td>3</td>
            <td>4</td>
            <td>6</td>
        </tr>
        <tr>
            <td>논리 파일</td>
            <td>7</td>
            <td>10</td>
            <td>15</td>
        </tr>
        <tr>
            <td>인터페이스</td>
            <td>5</td>
            <td>7</td>
            <td>10</td>
        </tr>
    </tbody>
</table>

- UFP 계산
UFP = C(input) * W(input) + C(output) * W(output) + C(inquiry) * W(inquiry) +C(logical) * W(logical) + C(interface) * W(interface)

여기서 C는 소프트웨어 요소의 개수, W는 소프트웨어 요소의 가중치이다.

2. 기능점수 모델:VAF(Value Adjustment Factor) 산정
- 14개 기술적 분야에 대한 복잡도를 고려하여 0~5의 점수 부여
    * 데이터 통신, 분산 데이터, 성능 중요도, 과중한 하드웨어 사용
    * 높은 트랜잭션 비율, 온라인 데이터 입력, 온라인 업데이트
    * 복잡한 계산, 설치 용이성, 오퍼레이션 용이성, 이식성
    * 유지보수성, end-user의 효율, 재사용성
- TCF(Total Complexity Factor) 계산
    * TCF = 0.65 + [0.01 * (VAF1 + VAF2 + ... + VAF14)]
    * TCF 값 범위: 0.65 ~ 1.35

3. 기능점수 모델:AFP(Adjusted Function Point) 산정
- AFP(FP) = UFP * TCF
    * AFP : 단지 크기에 대한 추정 값
- 노력 추정(effort) = (w FP)/(y FP/1MM) = z MM
    * FP/1MM: 개발 팀의 생산성
        + 개발자 1인이 1개월에 개발할 수 있는 평균 FP

4. 객체 점수 모델
- 객체지향 시스템을 개발할 때 객체를 기초로 한 추정 방법
- 절차
    1. 문제 도메인에 있는 클래스 개수 추정(Cc)
        * 요구 분석과 UML 모델링을 통해 추정
    2. 사용자 입력의 종류 분류 및 가중치 부여(Wi)
        * UI없음: 2.0
        * 단순한 텍스트 UI: 2.25
        * 그래픽 UI: 2.5
        * 복잡한 UI: 3.0
    3. 최종 클래스 개수(TCc)
        * Cc * Wi + Cc
    4. Effort
        * TCc * MD
            + MD: 클래스 하나를 개발하는데 소요되는 생산성 추정 값(보통 15~20)
    
+ [예제] 프로젝트 초기 추정 클래스 개수: 50, 복잡한UI, 생산성 18MD
    * TCc = 50 * 3.0 + 50 = 200
    * 노력(effort) 추정: 200 * 18 = 3600MD, 3600/365 = 9.86MM

## **프로젝트 일정 관리**
- 프로젝트 일정 관리 프로세스
    * 활동 정의(Activity definition)
        + WBS(Work Breakdown Structure)에서 정의한 활동을 달성하기 위해 구체적인 활동 식별
    * 활동 순서 정의(Activity sequencing)
        + 활동 간의 논리적인 순서 관계를 정의
    * 활동 자원 추정(Activity resource estimating)
        + 개별 활동의 수행을 위해 필요한 자원을 추정
    * 활동 기간 추정(Activity duration estimating)
        + 개별 활동을 완료하기 위해 필요한 작업 기간을 추정
    * 일정 계획 수립
        + 개별 활동의 순서, 기간 및 필요한 자원 등을 분석하여 일정을 작성
    * 일정 통제
        + 계획 대비 일정 차이를 모니터링 하여 필요한 조치를 취함

<img src="Activityprocess2.png" alt="Activityprocess2">

- 프로젝트에서의 활동(Activity)
    * 관리 측면에서 진행 상황을 판단할 수 있도록 가시적 산출물을 생산하도록 조직되어야 함

- Milestones(이정표)
    * 프로젝트 진행 과정에서 중요한 이벤트 또는 활동의 완료 시점
    <img src="Milestones.png" alt="Milestones">

- Deliverables(산출물)
    * 고객에게 제공되는 프로젝트 결과물

- 일정계획 수립
    * 개발 프로세스를 이루는 활동을 파악하고 순서와 일정을 정하는 작업
    * 과정
    <img src="Activityprocess3.png" alt="Activityprocess3">

- PERT/CPM 차트
    * Program Evaluation and Review Technique/ Critical Path Method
    * 세분화 된 작업을 효율적으로 일정 관리하고 지원하기 위한 기법
    * 관리에 대한 작업도 포함 가능
    * 작업 시간을 정확하게 예측할 필요

- PERT/CPM 차트의 장점
    * 관리자의 일정 계획 수립
    * 프로젝트 안에 포함된 작업 사이의 관계 표현
    * 병행 작업 계획
    * 일정 시뮬레이션
    * 일정 점검, 관리 가능

- 일정 계획 수립
    * 산출물 생산에 소요되는 작업 결정
    * 작업 예상 시간 결정
    * 선행 작업 파악: 작업의 종속 관계(선후 관계 파악)
<img src="PERT-CPM0.png" alt="PERT-CPM0">

- PERT/CPM 차트 작성
<img src="PERT-CPM1.png" alt="PERT-CPM1">

- PERT/CPM 차트 작성(간략한 표현)
<img src="PERT-CPM2.png" alt="PERT-CPM2">

- 임계경로 구하기
    * 임계경로(Critical Path)
        + Pert Chart 에서 소요 기간이 가장 긴 경로
        + 프로젝트를 완수하기 위해 필요한 최소 시간(minimum time)을 나타냄
    <img src="CriticalPath.png" alt="CriticalPath">

- 간트(Gantt) 차트
    * 활동 별로 작업의 시작과 끝을 나타낸 그래프
    * 가로축: 시간
    * 세로축: 작업
    * 마일스톤: 마름모 표시
    * 파란색 막대: 여유시간
    * 계획 대비 진척도를 표시
    * 개인별 일정표 
    <img src="Gantt.png" alt="Gantt">

- 애자일 일정 계획
스토리카드 단위의 일정 계획
    * 사용자 스토리, 점수(유스케이스 개발에 소요되는 노력), 비즈니스 우선순위, 연관된 스토리 등을 기록
    <img src="Agile.png" alt="Agile">

## **프로젝트 인적 자원 관리**
- 프로젝트에 이상적인 사람(ideal people)을 선정하지 못할 가능성 있음
    * 프로젝트 예산으로 인해 고임금 인력의 채용이 어려울 수 있음
    * 적절한 경험이 있는 인력이 제공되지 않을 수 있음
    * 조직은 소프트웨어 프로젝트를 통해 직원들의 역량이 개발되기를 원함
- 관리자
    * 특히 훈련된 인력이 부족할 때 위와 같은 제약조건 내에서 일할 수 있는 능력을 갖춰야 함

- 조직(팀)의 구성
    * 결과물 및 소프트웨어 개발 생산성에 큰 영향
    * 역할과 책임 배정
    * 조직 관리

- 조직 선택에 영향을 주는 요소
    * 작업의 특성
    * 팀 구성원의 의사 교류 횟수

- 소프트웨어 개발팀 구성
    * 계층형 팀
    * 에고리스(egoless) 팀
    * 책임 프로그래머 팀
    <br>

    1. 계층형 팀
        * 계층형 팀의 특징
            + 초보자와 경험자를 분리
            + 프로젝트 관리자와 고급 프로그래머에게 지휘 권한이 주어짐
            + 의사 교환은 초보 엔지니어나 중간 관리층으로 분산
        * 장점
            + 소프트웨어의 구조가 계층적으로 잘 나누어진 경우에 적합
            + 대규모 프로젝트의 경우 여러 계층으로 구성
        * 단점
            + 기술 인력이 관리를 담당
            + 의사 전달 경로가 김

    2. Egoless 팀
        * Egoless 팀의 특징
            + 민주주의 방식 의사 결정
            + 서로 협동하여 수행함
            + 자신의 일을 알아서 수행
            + 구성원이 동등한 책임과 권한
        * 장점
            + 작업 만족도 높음(더 적극적으로 임하고 문제에 책임감을 느낌)
            + 의사 교류 활발
            + 복잡하고 이해되지 않는 문제가 많은 장기 프로젝트에 적합
        * 단점
            + 책임이 명확하지 않은 일이 발생
            + 대규모 프로젝트에 적합하지 않음(의사 결정 지연 가능)

    3. 책임 프로그래머 팀
        * 책임 프로그래머 팀의 구성
            + 작은 팀으로 구성, 책임 프로그래머가 통제
            + Chief programmer team
                - 외과 수술 팀 구성에서 따옴
            + 책임 프로그래머: 제품 설게, 주요 부분 코딩, 중요한 기술적 의사 결정, 작업 지시
            + 프로그램 사서: 프로그램 리스트 관리, 설계 문서 및 테스트 계획 관리
            + 보조 프로그래머: 기술적 문제에 대하여 상의, 고객/품질 보증 그룹과 접촉, 부분적 분석/설계/구현 담당
            + 프로그래머: 각 모듈을 프로그래밍, 테스팅, 디버깅
        * 장점
            + 의사 결정이 집중 되어있어 의사 결정이 빠름
            + 소규모 프로젝트에 적합
            + 초보 프로그래머를 훈련시키는 기회로 적합
        * 단점
            + 한 사람의 능력과 경험이 프로젝트의 성패 좌우

- 효과적인 팀 규모 선택
    * man/month와 같이 예상 개발 노력이 주어지면, 최적의 팀 규모를 정할 수 있음
        + 팀 규모를 두 배로 늘린다고 개발 시간이 절반으로 단축되지는 않음
    * 서브시스템과 팀은 필요한 지식의 총량과 정보 교환이 감소하도록 크기가 정해져야 한다
    * 프로젝트 반복의 경우, 한 팀의 인원 수는 일정하지 않을 수 있음
    * 스케줄보다 늦을 경우에 그것을 따라잡기 위해 일반적으로 인력을 추가할 수 없다

- 팀에 필요한 기술
    * Architecture
    * Project manager
    * Configuration management and build specialist
    * User interface specailist
    * Technology specialist
    * Hardware and third-party software specialist
    * User documaentation specialist
    * Tester

## **프로젝트 위험 관리**
- 위험관리
    * 위험을 식별하고 프로젝트에 미치는 영향을 최소화하기 위한 계획 수립
- 위험은 어떤 불리한 상황이 발생하는 것
    * 프로젝트 위험은 일정 또는 리소스에 영향을 미침
    * 제품 위험은 개발 중인 소프트웨어의 품질 또는 성능에 영향을 미침
    * 비즈니스 위험은 소프트웨어를 개발하거나 조달하는 조직에 영향을 미침

- 소프트웨어 위험
    |Risk|Affects|Descriptions
    |---|---|---|
    |Staff turnover|Project|Experienced staff will leace the project before it is finished
    |Management change|Project|There will be a change of organisational management with different priorities
    |Hardware unavailability|Project|Hardware that is essential for the project will not be delivered on schedule
    |Requirement change|Project and Product|There will be a larger number of changes to the requirements than anticipated
    |Specification delays|Project and Product|Specifications of essential interfaces are not available on schedule
    |Size underestimate|Project and product|The size of the system has been underestimated
    |CASE tool under-performance|Product|CASE tools which support the project do not perform as anticipated
    |Technology change|Business|The underlying technology on which the system is built is supersded by new technology
    |Product competition|Business|A comptetitive product is marketed before the system is completed
- 위험 관리 프로세스
    * 위험 식별
        + 프로젝트, 제품 및 비즈니스 위험 파악
    * 위험 분석
        + 이러한 위험의 가능성 및 결과 평가
    * 위험 대처 계획 수립
        + 위험의 영향을 회피하거나 최소화하기 위한 계획 작성
    * 위험 모니터링
        + 프로젝트 전반에 걸쳐 위험 모니터링
    
    <img src="RiskProcess.png" alt="RiskProcess">

- 위험 식별
    |Risk type|Possible risks|
    |---|---|
    |Technology|The database used in the system cannot process as many transactions per second as expected<br>Software components that should be reused contain defects that limit their functionality.|
    |People|It is impossible to recruit staff with the skills required.<br>Key staffs are ill and unavailable at critical times.
    |Organisational|The organisation is restructured so that different management are responsible for the project.<br>Organisational financial problems force reductions in the project budget|
    |Tools|The code generated by CASE tools is inefficient.<br>CASE tools cannot be integrated.
    |Requirements|Changes to requirements that require magor design rework are proposed.<br>Customers fail to understance the impace of requirements changes.
    |Estimation|The time required to develop the software is underesteimated.<br>The rate of defect repair is underestimated.|

- 위험 분석
    |Risk|Probability|Effects|
    |---|---|---|
    |Organisational financial problems force reductions in the project budget.|Low|Catastrophic
    |It is impossible to recruit staff with the skills required for the project.|High|Catastrophic
    |Key staff are ill at critical times in the project|Moderate|Serious
    |Software components that should be reused contain defects which limit their functionality|Moderate|Serious
    |Changes to requirements that require magor design rework are proposed.|Moderate|Serious
    |The organisation is restructured so that different management are responsible for the project.|High|Serious

-   위험 대처 계획 수립
    * 회피 전략
        + 위험이 발생할 확률을 줄임
    * 최소화 전략
        + 프로젝트나 제품에 대한 위험의 영향을 줄임
    * 비상계획
        + 위험이 발생할 경우, 그러한 위험을 다루기 위한 비상계획
    
    |Risk|Strategy|
    |---|---|
    |Staff illness|Reorganise team so that there is more overlap of work and people therefore understand each other's jobs.|
    |Defective components|Replace potentially defective components with bought-in components of known reliability.

- 위험 모니터링
    * 프로젝트 전반에 걸쳐 위험 모니터링 및 평가
        + 식별된 각 위험을 발생 가능성이 낮아지는지 여부를 결정하기 위해 정기적으로 평가한다
        + 또한 위험의 영향이 변경되었는지 여부를 평가한다
    * 각 주요 위험은 관리 진도 회의에서 논의되어야 한다.