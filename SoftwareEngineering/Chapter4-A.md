# **요구사항 명세서 작성**

## **요구사항 명세서 구성**
1. Introduction
2. Extenal Interface Requirements
    1. User Interface
    2. Hardware Interface
    3. Software Interface
    4. Communication Interface
3. System Feature
    1. System Feature1
        1. Description and Priority
        2. Functional Requirements
4. Other Nonfunctional Requirements
    1. Performance Requirements
    2. Safety Requirements
    3. Security Requirements
    4. Software Quiality Attributes
5. Other Requirements
    1. 하드웨어 제약 조건
    2. 자원, 인력에 대한 제약 조건
6. Appendix

## **1. Introduction**
- Purpose
    * 문서의 목적을 개략적으로 기술
    * 문서에 대해 관심을 가져야 할 이해 당사자를 명시

- Scope
    * 소프트웨어 제품이 수행하게 될 기능, 주요 이점, 목적 및 목표를 포함하여 해당 소프트웨어에 대해 간략하게 설명
    * 소프트웨어를 기업 목표 또는 비즈니스 전략과 연관시킨다.

- Definitions, acronyms, and abbreviations
    * 요구사항 명세서를 이해하는데 필요한 용어의 정의, 약어, 축약어 들에 대하여 기술
- References
    * 요구사항 명세서를 작성하는데 참고한 모든 문서를 나열

## **2. External Interface Requirements**
- User Interface
    * 소프트웨어 제품과 사용자 간의 각 인터페이스의 논리적 특성을 설명
    * 샘플 화면 이미지, 따라야 할 GUI 표준 또는 제품군 스타일 가이드, 화면 레이아웃 제약 조건, 모든 화면에 나타나는 표준 버튼 및 기능(ex: 도움말, ), 오류 메시지 표시 표준 등을 포함

- Hardware Interface
    * 소프트웨어 제품과 시스템의 하드웨어 구성 요소 사이의 각 인터페이스의 논리적 특성과 물리적 특성을 설명
    * 지원되는 장치 유형 소프트웨어 및 하드웨어 간의 데이터 및 제어 상호 작용의 특성, 사용할 통신 프로토콜을 포함 가능
- Software Interface
    * 소프트웨어 제품과 데이터베이스, 운영 체제, 도구, 라이브러리, 통합될 상용 컴포넌트를 포함하여 여타 특정 소프트웨어 컴포넌트(이름 및 버전) 간의 연결을 설명
    * 시스템에 들어오거나 나가는 데이터 항목이나 메시지를 확인하고 각각의 목적을 설명
    * 필요한 서비스와 통신의 특성을 기술
    * 소프트웨어 컴포넌트들이 공유 할 데이터를 식별
- Communication Interface
    * e-메일, 웹 브라우저, 네트워크 서버 통신 프로토콜 등을 포함하여 이 소프트웨어 제품에 필요한 모든 통신 기능과 관련된 요구 사항을 설명
    * 적절한 메시지 포맷을 정의
    * FTP 또는 HTTP처럼 사용될 통신 표준을 식별
    * 통신 보안 또는 암호와 이슈, 데이터 전송 속도, 동기화 메커니즘 등을 지정

## **3. System Features**
- System Feature 
    * Description and Priority
        + 기능에 대해 간단히 설명하고, 우선순위를 높음, 중간, 또는 낮음으로 표시
    * Functional Requirements
        + 이 기능과 관련된 자세한 기능 요구사항을 나열
        + 예상되는 오류 상태 또는 유효하지 않은 입력에 소프트웨어 제품이 어떻게 대응해야 하는지를 포함
        + 요구 사항은 간결하고, 완전하며, 모호하지 않고, 검증 가능해야 함
        + 각 요구사항은 식별자(일련 번호 또는 의미 있는 태그)로 식별 되어야 함

ex)
- 기능: 메뉴추천
    * 설명 및 우선순위
        + 사용자가 선택한 식사 시간 정보를 바탕으로 식사 메뉴를 추천한다
        + 메뉴 추천 방식은 랜덤, 사용자 선호도 기반, 날씨 정보 기반, 사용자 선호도 + 날씨 정보 기반의 방식이 있다.
        + 우선순위: 높음(필수 기능)
    * 기능적 요구사항
    ...

## **4. Other Nonfunctional Requirements**
- Performance Requirements
    * 성능이 중요한 요구사항일 경우, 반응 시간, 처리 소요시간, 처리율 등을 기술
- Safety Requirements
    * 안전이 중요한 요구사항일 경우, 제품 사용으로 인해 발생할 수 있는 손실, 손해, 또는 상해와 관련된 요구 사항을 명시
    * 방지해야 할 조치 뿐만 아니라 취해야 할 안전 조치 등을 정의
    * 제품의 설계 또는 사용에 영향을 미치는 안전 문제를 명시한 외부 정책 또는 규정을 참조
    * 만족해야만 하는 안전인증을 정의
- Security Requirements
    * 보안이 중요한 요구사항일 경우, 제품 사용 또는 제품에서 사용하거나 작성한 데이터 보호와 관련된 보안/개인 정보 보호 문제 등과 관련된 요구 사항을 지정
    * 사용자 ID 인증 요구 사항 정의
    * 제품에 영향을 미치는 보안 문제가 포함된 외부 정책 또는 규정 참조
    * 충족해야하는 보안 또는 개인 정보, 인증을 정의
- Software Quality Attributes
    * 고객 또는 개발자에게 중요한 제품 품질 특성을 추가로 지정
    * adapability, correctness, flexibility, usability, reusability, portability, reliability, testability, maintainability, availability 등을 고려

## **5. Other Requirements**
- 하드웨어 제약조건
    * 소프트웨어의 동작이나 운용 시 하드웨어에 대한 제약이나 요구사항을 반드시 명세 해야 할 경우, 기억 장치 규모, 통신 수용도 등을 기술함
- 자원, 인력에 대한 제약조건
    * 소프트웨어의 개발, 동작 또는 운용 시 자원 및 인력 등에 대한 제약이나 요구사항을 반드시 명세 해야 할 경우, 자원 및 인력 규모 등을 기술함