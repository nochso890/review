# ifkakao 2019 참석 후기

## 1. Keynote

* 카카오 개발자 센터
  * 메세지 전송 API 오픈
* 빅데이터 공개
  * 공개 가능한 다양한 데이터셋을 이용한 경진을 개최할 예정
* 버팔로 / 추천시스템
  * [오픈 소스](https://github.com/kakao/buffalo)로 공개
  * 현재 공개된 오픈 소스 중 수배 이상 빠르며 메모리가 절약된 상태
* 카카오 기술 공유 사이트 오픈
  * [사이트](https://tech.kakao.com/)
  * 카카오의 서비스 개발 사례 및 트러블 슈팅 과정에서 얻은 기술과 노하우 공유
  * 개발자 커뮤니티 지원

### 카카오뱅크

카카오뱅크가 만들어가는 금융의 혁신

* 1000만 고객 달성
  * 대한민국 구민 4명 중 1명이 쓰는 카카오 뱅크
* 금액 규모
  * 예금 & 적금: 17.5조
  * 대출 11.3조
  * 시중 메이저 은행에 비하면 턱없이 부족하지만, 중소 은행 수준까지는 2년만에 달성
* 전체 은행 앱 방문자수 1위
* 카카오뱅크 이후 금융은?
  * 새로운 모바일앱 표준
    * 시중 여러 은행이 모바일 앱 개편 진행
  * 오픈소스 기반 은행 시스템 개편
    * 기존 금융권 시스템의 구조를 깬 리눅스 채택
      * 이를 비롯한 Node, Nginx, Tomcat 등 적극적 사용
    * 금융 DB의 핵심이였던 오라클 자리를 오픈소스 DB인 MySQL 등오로 전환
    * 빅뱅 방식이 아닌 점진적 개선 진행
* 이렇게 가능한 이유는
  * 모바일 퍼스트와 기술 주도

#### 모바일 환경
  
* 언제나 들고 다니며, 이동에 편리한 작은 화면
* 여러 모바일 서비스들을 보면 PC의 긴 호흡을 모바일에서는 잘게잘게 잘라서 사용 하는 등의 작업이 필요
* 간결한 인터페이스, 소셜 기능을 이용한 활성화
* 모바일 시대 은행의 재정의?
  * 이전의 은행은 safe place, banking license였다면 미래의 은행은 **Utility**로 전환중
  * 즉, 사용자가 있는 곳이 은행이다.
  * 비지니스 재정의 수준으로 재설계
    * 지금까지는 PC를 대응하듯이 진행
    * 하지만 모바일에서는 완전히 재정의가 필요
  * 모바일 완결성의 카카오뱅크
  * 26주 적금
    * 금융에서의 공식은 금리
    * 26주 적금은 금리가 아닌 재미로 진행


#### 기술 주도

* 골드만 삭스는 몇백명의 트레이더를 2명으로 줄임
* 기술 중심 전략을 바탕으로 개발자 역할을 확장
  * 개발자는 본인이 만든 제품의 **첫번째 고객**
  * 외주 개발자라면 내가 만든 제품에 만족하는게 첫번째일까? No. 납기를 맞추는게 가장 중요
* 조직 구성비
  * 개발 41%
  * 서비스/상품 20%
  * 고객 서비스 18%
  * 기타 21%
  * 금융권을 기준으로 보면 굉장히 놀라운 비율
* 문화
  * 카카오뱅크는 카카오의 문화를 도입
    * 창의성/자기주도성/수평커뮤니케이션
  * 재밌는 사례
    * 주 52시간 체크 하는 웹 사이트를 **개발자 개인이 필요해서 직접 개발**

정리하면  
모바일에 대한 다른 접근 + 기술 기반 전략 및 문화 = 차별화된 새로운 금융 서비스

## 2. 서버 성능 테스트 Tips - 사례를 통해 얻은 교훈들

* 하고 있는 일
  * 카카오 게임즈에서 런칭한 대부분 서비스의 서버 성능 테스트

### 2-1. 서버 성능 테스트 개요

* 교차로에 이미 막힌 차들과 뒤에서 계속 몰려오는 차들 사이의 해결책은?
* 대응 방안을 정하기 전에.. 사전에 미리 파악하는것이 바로 성능테스트의 필요성
* 성능 테스트의 목적
  * 시스템의 성능 기준 정리
  * 실제 사용자의 액션과 유사한 시나리오 작성 -> 실제 유저의 부하와 유사한 부하 유입
  * 시스템의 병목 구간을 찾아서 튜닝 완료

### 2-2. 성능테스트 진행 사례 및 교훈

1. 이용자의 다양한 액션 누락

* 원인
 * PVP는 후반 컨텐츠라서 아직 부하가 올라가면 안되는데...
 * PVP부터 진행하는 유저들이 있을줄이야!
 * 성능 테스트에서 해당 시나리오는 진행 안되어 있음
* 다양한 흐름 세분화 및 API별 시나리오 명세 작성
 * API Call Count 체크, 총 시나리오 길이 등 명세 작성

2. 최종 시스템의 성능 테스트 누락

* php-fpm이 설치된 장비의 memory full이 원인으로 파악
* 성능 테스트때는 왜 발견되지 않았나?
  * 성능 테스트시
    * 다수 WAS, 1대의 멤캐시
  * 실제 런칭시
    * 다수 WAS, 다수의 맴캐시
  * 성능 테스트 종료 후 변경된 시스템 구조에 대해서는 성능 테스트가 진행되지 않았던 것
* 가급적 최종 런칭 스펙과 동일한 구성으로 성능 테스트 진행하도록 프로세스 강화
  * 성능 테스트 종료 후 시스템 구조 변경이 필요하면 **서비스 담당자들이 합의** 하도록 변경

3. 예외 상황에 대한 테스트 누락

* 로그 서버 다운으로 API서버로 연쇄 다운되는 현상
  * 원인은 API -> 로그 서버를 호출하는데 로그 서버가 죽어 있어 계속 응답 지연이 발생
  * 응답지연이 발생하다가 결국 API서버도 다운
* 모든 구성 서버들에 대해 예외 상황 테스트 진행
  * 즉, 연관된 다른 서비스가 장애 발생하면 우리 서비스에는 어떤 일이 발생하는지 확인
  * 오토 스케일링, DB 페일오버 등에 대해서도 검증 진행

4. 커넥션을 유지하는 100만 사용자

* 이용자 100만명이 30분 동안 모두 인입되며 각각 커넥션 유지
  * 대규모 트래픽의 분산, 커넥션 모두 유지
  * 로드밸런싱 중요
* 그래서 AWS의 NLB를 도입
* 50만까지도 정상 작동
  * 70만 vuser 인입시 응답 지연 현상 및 Error 발생 발견
  * 서버 설정 후 다시 테스트 하니 100만 달성
  * 기분 좋게 퇴근후 다음날 다시 테스트 하니 또다시 70만에서 장애 발생
  * 다시 테스트 하니 이번엔 100만 발생
  * 즉, 언제 어느때 실행해도 그날의 **첫번째 테스트에는 항상 70만에서 장애** 발생

* AWS 가이드문서: NLB에는 프리 웜업이 필요 없는데??
  * 근데 왜 계속 첫번째만 발생하지?
* 2주간의 테스트 결과를 다시 AWS에 전달하니?
  * NLB에서도 프리 웜업이 필요하다는 답변
  * NLB의 프리 웜업을 요청하여 현상 해결
* 교훈
  * 가이드를 참고하되 목표치까지 테스트 하는 것이 확실한 결과 측정 가능

5. API 검증 실패

* API 검증이란?
  * 성능 테스트 진행전 테스트 대상 API들이 정상 작동하는 것을 확인하는 작업
  * 요청 파라미터, 응답 값을 확인
* 실패 상황
  * 근데 단위 서버당 성능 측정 수치가 비정상적으로 높았음
  * 왜이렇게 성능이 잘나오지?
  * 확인해보니 **Mock 서버**였음
* 왜 이런 상황이?
  * 해외 개발사의 서비스라 사전에 성능 테스트에 대해 커뮤니케이션 부족
  * 데이터 쓰기성 API는 200 OK 응답인 것만 확인 했음
* 개선
  * 성능 테스트 전 사전에 개발자 - QA간에 성능 테스트에 대한 논의 강화
  * API 검증 강화 - 데이터 쓰기성 API는 어드민 등에서 실제 데이터 확인 과정을 추가

6. 서버 push로 액션이 시작되는 서비스

* 클라이언트와 서버는 웹 소캣으로 통신하면 서버 푸시로 액션이 시작
* 테스트가 어렵다
  * nGrinder는 Vuser 단일 쓰레드로 실행됨
  * 하나의 쓰레드로 리느싱 상태 유지하면서, 별도의 쓰레드로 통신이 필요
  * 성능 투르이 부하량이 증가하고, 원하게 시나리오 동작이 어려움
  * Bot 클라이언트로 테스트 진행했으나, 지속적인 개발자 리소스가 필요함
* 문제 해결
  * 네티(Netty)를 이용하여 공용 I/O 쓰레드를 사용하게 중간 서버를 만듬
  * 서버와의 요청/응답은 I/O 쓰레드가 수행

7. 120개의 봇을 수동으로 실행한다?

* 특이 사항
  * 외부 개발사에서 제작한 Bot 클라이언트로 테스트를 진행
  * Bot 클라이언트 1개당 500 EA Vuser 생성 (목표 동접 6만)
  * 총 120개의 클라이언트를 **동시에 실행** 시켜야만함
* 문제 해결
  * nGrinder를 Bot 클라이언트를 제어하도록 변경
  * 한개의 Controller가 N개의 Bot을 실행하는 BAT File 구동 및 제어

### 2-3. 더 좋은 성능 테스트를 위해 준비하고 있는 것

* 시스템 성능 기준 마련
  * EC2 종류 별 적정 TPS, 혹은 수용 가능한 동접수
  * 예) 
    * 시스템 구성에 대한 적정 TPS
      * Front와 API로 구성되어 있고, PHP-FPM과 Nginx니깐 2000TPS 가능합니다
    * 라는 답변을 하고싶음
  * 그래서
    * 결과 데이터를 분류 하고 저장하고 있음
    * 나중에 빅데이터처럼 보편적인 수치를 뽑아낼수 있을것 같음
* 테스트 가능 환경의 확장
  * 현재는 웹 서버를 주로 테스트
  * 이후에는 TCP 등에 대한 테스트가 가능하도록 **라이브러리 개선 및 사례 수집**

### Q & A

nGrinder를 사용하는 이유는?

* 오픈소스
* 웹 API가 주로 테스트 대상이라 nGrinder로 해도 무방
* 원하는 시나리오대로 **스크립트를 작성해서 사용할 수 있었음**

AWS로 성능 테스트 하면 과금은 어떻게 감당하시는지?

* 처음부터 서버 구성을 해서 진행하진 않고, 단일 서버로 먼저 시작
* 단일 서버가 잘되면 2대로 늘려서 분산 처리 확인
* 그 이후 장비를 늘려거 테스트를 한다.
* 사용하지 않을때는 항상 끄고 있기 때문에 비용을 최소화 하려고 함

성능 테스트 대상인 서버들의 CPU를 비롯한 기타 지표는 어디서 확인을?

* 클라우드와치 등을 이용해 각 분산 시스템들의 성능 지표를 받아서 확인함

## 3. 초당옥수수의 취소를 막아라! : 수만 건의 주문을 1초내에 처리하는 기술

### 3-1. 초당 옥수수의 비밀

* 엄청 달달한 옥수수
  * 당분 함량이 기존 옥수수 대비 2배 이상
* 농작물은 수확후 보관후 보낼수가 없으니, 수확 전에 주문을 받고, 수확후 바로 배송 시작
* 농작물 상태에 따라 배송 지연 혹은 배송 취소를 진행

### 3-2. 레거시 시스템의 문제
 
* 배송 지연 안내 TMS 배치 수행
  * 실시간 처리가 아닌 배치 (1시간 간격)
    * 평소 수행시간 몇분 이내
    * 배송 지연이 밀리는 시즌 대비해서 1시간으로 둠
  * 복잡한 비지니스 로직
  * 처리 속도
    * 단일 스레드에서 반복되는 테스크 수행
    * 요청 받은 대량의 주문 리스트 대상일 경우 속도 이슈

### 3-3. 우리의 미션

* 미션 1. 실시간 TMS 발송
  * 왜 실시간으로 하면 안되지?
    * 카카오 공통 TMS 명세서는 종류가 300개 이상
    * 처리에 필요한 DB, 주문 API 등이 많이 필요
    * 실시간으로 하기엔 성능 이슈 발생
  * 개선
    * 배치로 처리하지 않고, TMS 처리 비동기 워커로 변경
    * Rest API로 레빗MQ에 데이터를 보내고, 여러대의 워커에서 메세지 발송

* 미션 2. 복잡한 비지니스 코드 리팩토링
  * 요청 데이터에 대한 벨리데이션과 비지니스 로직의 벨리데이션을 분리 시작
* 미션 3. 처리 속도 개선
  * 반복되는 Task 수행시 병목 발생
  * 전체 로직은 중간에 수행되는 비지니스 처리가 끝날때까지 대기할 수 밖에 없음
    * 비지니스 처리가 전체 수행의 90%이상을 차지
  * 비지니스 처리를 위한 비동기 워커를 생성
  * 요청이 오면 벨리데이션 수행후 비지니스 처리 큐로 전달
  * 비지니스 처리 큐의 메세지를 비지니스 워커에서 데이터를 받아 다시 TMS 큐로 메세지 전달
  * TMS 큐의 메세지를 TMS 워커가 데이터를 컨슘한 뒤 발송 진행

### 3-4. 액터가 된 비동기 워커

* 쉽게 분산처리 할 수 있는 구조
  * 워커 서버수나 컨슈머 수를 늘리면 성능이 향상 되도록 최소한의 단위로 큐 메세지를 생성
* 컨슈머 처리시 예외 발생
  * 성공 / 재처리/ 실패 등
  * 실패한 메세지들은 dead 큐로 전달
  * 별도의 배치가 실시간으로 돌면서 dead 큐에 쌓인 데이터가 있는지 체크한다
    * dead 큐에 쌓인 데이터가 있으면 DB에 큐 메세지를 저장한다.
    * DB에 저장 중에 실패하더라도 로그가 있어 로그를 보고 수동 등록 가능
  * 실패 관리 어드민에서 실패 사유와 수동 처리 등의 기능으로 재처리 한다.
    * 실패 관리 어드민에서는 메세지의 삭제와 재전송 기능을 갖고 있음
* 왜 레빗MQ?
  * 레빗MQ의 메세지 헤더와 큐 속성을 활용해 재처리/실패 전략 수립 가능
  * 헤더에 리트라이 카운트를 관리해 애플리케이션에서는 인터셉터에서 리트라이 카운트를 보고 dead 큐로 뺄지 이후 단계로 넘길지 판단한다.

> 제 생각)
AWS SQS와 Spring Cloud AWS에서 리트라이, DLQ 등의 기능을 지원하고 있는데 레빗MQ는 이게 안되는건가?

### 3-5. 배포없이 빠르게 롤백하는 전략

* 주키퍼에서 넘겨준 flag값을 각 애플리케이션의 로컬 캐시에 저장하여 레거시/신규 코드로의 전환에 사용

## 4. Practical Microservices in gRPC Go feat. GraphQL, Kafka(서포터즈 기사 개발기)

* 서포터즈 기사
  * 시급제 대리기사
* 카카오 T 대리개발파트 상황
  * 외부 영업
    * 국내외를 가리지 않고 인재 영입
  * 주요 기술
    * Rails, Java, JRuby, Groovy 등
  * 운영 대응
  * 학습 비용 증가
* 생각의 정리부터
  * 어떻게 설계/구성할까
    * 짧은 문서에 담아 소수의 선택된 사람들에게 계획을 승인받고 사내에 공유한다.
* RFC
  * Request for Comments
* 프로그래밍 언어
  * Java의 장단점
    * 장점
      * 모두가 익숙하다
      * 스프링 프레임워크가 모든걸 해준다
      * 개발자 구인이 용이하다
      * OS 독립적
    * 단점
      * 스프링 프레임워크 내부를 보기가 어렵다
      * 개발자의 편차가 크다
      * JVM 튜닝은 골치 아프다
  * 파트의 선호도
    * 자바 극혐 vs 자바 익숙
    * 그래서 내린 결론은? **Go**
  * 언어별 HTTP 속도 비교에서 Go가 일단 승리
  * GO
    * 배우기 쉽고 자유도가 낮은 코딩 스타일
      * 개발자간 편차 없이 거의 동일한 결과
    * No Magic
      * 모두 들여다 볼 수 있음
    * 컨테이너 시대의 언어
      * 커맢일 결과물로 단 하나의 실행파일이 나옴
  * Go 의존성 주입, FX
    * 스프링 프레임워크의 장점인 의존성 주입을 오픈소스 FX로 대체 가능

### 서비스 구조

* 카카오 T 대리 초기엔 모놀리틱
  * 루비온 레일즈 Rest API
  * 라인수 27만 라인 정도
  * 35%의 라인 커버리지
    * 나머지 60%는 어떻게 돌아가는지 믿을 수 없음
  * 배포 스트레스
* 왜 MSA?
  * 고립된 기능, 고립된 데이터(상태)
  * 자주 작은 배포를 하고 싶음
  * 기술적 개성이 강한 우리, 장점을 활용하자
    * 다른 프로그래밍 언어, 데이터 스토리지 기술 사용
    * 미래에도 유리
* 우리는 MSA로...
  * 하나의 온전한 코드 저장소를 소유하는 것은 보상이 주어지고 가치 있는 경험
  * 서비스 소유로 책임감
  * 신규 인력이 계속 채용되는 중이고 서비스가 확장중이라 MSA가 적합
* MSA 단점
  * 기존에 한번의 API 호출이 MSA에서는 다수로 늘어남
    * API Gateway로 처리
  * 서비스간 네트워크 통신 비용
  * 공통 기능에 대한 라이브러리화
  * 테스트가 복잡
    * 현재는 경험을 쌓는중

### 서비스 계층

* HTTP+JSON vs gRPC
  * 성능 차이가 압도적
* gRPC UI
  * CLI나 Client 구현 없이 API 테스트 가능
  * Postman과 비슷하지만 실제로는 gRPC를 수행
* IDL Registry
  * 서비스에 대한 정의가 서비스와 공존하기 위해 gRPC proto파일들을 중앙화

### 테스트는 어떻게

* Load 테스트
  * ghz로 gRPC Load 테스트
  * REST 요청에 대한 테스트는 nGrinder로 진행
* 통합 테스트
  * 도커 컴포즈로 작성된 환경을 Postman 테스트 스크립트로 테스트
* 배포와 운영은 DKOS로
  * 사내 쿠버네티스 플랫폼

### if 카프카

* 서포터즈 기사가 출근할때 상태를 알아야 하는 서비스들이 많음
  * 먄약에 카프카가 없다면 콜백 or 배치 작업등이 필요
* MSA에서 캬프카 사용으로 얻은 이점
  * 서비스간 메세지 흐름 단순화
  * 패킷 오버헤드 감소

### 서비스 모니터링

* 2019.06.21 서포터즈 기사 출시
* 서비스 지연이 발생
  * 피크타임에 서포터즈 기사 기능으로 인해 서비스 지연 발생
  * 서비스내 가장 요청이 잦음
* 모니터링 시스템 부재의 원인과 대책
  * 촉박한 출시 일정으로 구축이 안되있었음
  * Prometheus & Grafana 도입
* Prometheus 
  * 이벤트 모니터링과 알람에 사용되는 오픈소스 매트릭 수집 도구
  * pull 방식
* Grafana
  * Prometheus Web UI에서 간단한 그래프 확인 가능
    * 다만 상용 서비스에서 쓰기에 아쉬움
  * Grafana 선택
* 장애의 원인은 무엇?
  * 한개의 마이크로 서비스가 프로덕션이 아닌, 스테이징 서비스로 접근했음
  * 뉴렐릭, 제니퍼 등 기존 APM으로는 요청의 전체 흐름을 분석하기 어려움
* 분산 로그 추적 시스템 도입 - Zipkin

### 레거시

* 우리가 원하는 것
  * 레거시의 변화를 최소화
  * 기존 기능, 신규 기능 모두 MSA로 분리하고 싶음
  * 레거시가 교살되고 싶음
    * 점진적 개선
* 개선
  * 트래픽 스로틀링 기능 추가
  * 설정 서버를 통해 트래픽 조절
  * Nginx 리버스 프록시를 통해 양쪽 다 운영
* 하고싶은일
  * 서킷 브레이크 도입

## 5. Airflow를 활용하여 아름다운 데이터 파이프라인 구성하기

* 2014년 3월에 비해 2019년 3월을 비교하면 100배 성장
  * 2015년 3월에 비해선 2019년 3월 10배 성장

### 5-1. 카카오페이지의 데이터 분석 문제

* MSA는 분석을 어렵게 만든다?
  * DB가 너무 많음 (회원, 주문, 웹툰 등등)
  * 서로 다른 DB간에 JOIN이 필요한 경우에는?
    * Data Lake
      * HDFS
  * 그래서 모든 데이터를 HDFS에 밀어넣자!
* 이 Lake를 구축하는 전반적인 작업을 **Airflow 사용**

### 5-2. Workflow management system

* Airflow?
  * Workflow 플랫폼
  * Airbnb에서 개발
  * 2015년 오픈소스로 풀림
* 워크 플로우 시스템으로 분류되는 프로젝트는 현재 250여개로 집계됨
* 워크플로우?
  * 작업
    * 수집/정제/지표생성/리포팅 등등
  * 작업간 의존성
  * 위 2개를 합쳐서 워크 플로우
  * 의존 관계에 따라 순차 실행
  * 모든 작업은 ASAP으로 종료
  * 중간 작업이 실패하면
    * 연관성 없는 작업은 계속 실행
* Airflow
  * 워크 플로우 생성 방식: 파이썬 코드
  * Rich UI
  * 가용성: 스케줄러가 Single 포인트
  * 표현이 간결하고 자유도도 높다.

### 5-3. Apache Airflow 시연

> 시연을 보고 느낀 점은 Spring Cloud Data Flow와 비슷해보인다.
만약 우리팀이 선택한다면 Spring Cloud Data Flow를 선택할것 같다.
다만 젠킨스 파이프라인 스크립트에 비해 얼마나 장점이 있는지는 확인이 필요할것 같다.

### 5-4. Airflow 아키텍처

* 스케줄러
  * 실행 주지가 되면 작업을 생성
  * 의존하는 작업이 모두 성공하면 Broker에 넘김
* 워커
  * 실제 작업 주체
* Broker
  * 실행 가능한 작업들이 들어가는 공간
* Meta DB
  * 메타 정보를 담고 있는 장소


## 6. 광고 데이터 처리 시스템 소개

* 기술
  * 유입
    * Play 프레임워크
  * Batch
    * HIVE와 IMPALA
  * Stores
    * 하둡 HDFS, 카산드라, HBASE, ES, Redis
  * 데이터 제공
    * 대시보드: Apache Kylin
    * 여러 데이터 소스 연동 분석용 노트북: 제플린
    * 직접 이벤트를 전달할때: 카프카
    * API로 전달할때: 스프링 프레임워크
* 요구사항
  * 데이터를 다각도로 빠르게 분석하면 좋겠다.
    * 다양한 조합의 빠른 분석
      * OLAP Cube
    * 조회하는 방식이 친숙했으면
      * SQL
    * BI툴과의 연동이 쉬웠으면
      * JDBC/ODBC
    * 새로운 조합의 데이터를 쉽게 만들었으면
  * 그래서 결국 Kylin도입
  * Kylin?
    * ebay, Apache Project
    * ANSI SQL
    * MOLAP Cube
    * BI 툴과 통합 가능
    * Web UI 지원
* Operation
  * Kibana
    * ES UI를 위해
  * Airflow
    * 워크 플로우 매니징을 위해
  * Sentry
    * 로그 알람을 위해
* 요구사항
  * 배치 작업의 운영이 쉬웠으면 좋겠다.
    * 작업간 의존성 관리가 잘되었으면
    * 재시도/재실행이 쉬웠으면
    * 손쉽게 기능 추가가 가능
    * 처리 상태를 한눈에 볼수 있었으면
  * Airflow 선택
    * 세션 4참고

### 데이터 파이프라인

* OLAP 데이터
  * 배치 작업 위주
  * 처리 과정
    * DB에서 광고 정보, 매체 정보등을 가져옴
    * Tableau를 통해 데이터 분석가에게 데이터 전달
  * 