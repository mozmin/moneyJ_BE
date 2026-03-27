<img width="1800" height="300" alt="money J(노션용2)" src="https://github.com/user-attachments/assets/a6e65a0f-d245-4bb3-92b0-fffd5f2bb9eb" />

<br>
<br>

> “가고 싶은 여행을, 빚이 아닌 저축으로 준비하자!”
>  
> MZ 세대의 소비 분석과 목표 저축을 돕는 맞춤형 금융 관리 서비스
> 
> 사용자의 소비 패턴을 분석해 스마트한 공동 여행 자금 마련을 돕습니다.

## 1. Project Overview

- **개발 기간:** 2025.08.22 ~ 진행 중
- **참여 인원:** 5명 (Front-end 2명, Back-end 3명)
- **담당 역할:** Backend Developer
  - CODEF API 기반 계좌/카드 데이터 연동 파이프라인 구축
  - 공동 여행 플랜 및 그룹 저축 로직 API 설계
  - 사용자 소비 패턴 기반 데이터 처리

<br>

## 개발 환경
### Language 
![Java](https://img.shields.io/badge/java-%23ED8B00.svg?style=for-the-badge&logo=openjdk&logoColor=white)

### Framework & Runtime
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-6DB33F?style=for-the-badge&logo=spring-boot&logoColor=white)

### ORM (Object-Relational Mapping)
![JPA](https://img.shields.io/badge/JPA-59666C?style=for-the-badge&logo=hibernate&logoColor=white)

### Database
![MySQL](https://img.shields.io/badge/mysql-%2300f.svg?style=for-the-badge&logo=mysql&logoColor=white)

### Infra & Cloud 서비스 (AWS)
![AWS EC2](https://img.shields.io/badge/AWS_EC2-FF9900?style=for-the-badge&logo=amazonec2&logoColor=white)
![AWS RDS](https://img.shields.io/badge/AWS_RDS-527FFF?style=for-the-badge&logo=amazonrds&logoColor=white)

### External Services & AI
![CODEF API](https://img.shields.io/badge/CODEF_API-005571?style=for-the-badge)
![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=for-the-badge&logo=openai&logoColor=white)

### DevOps & CI/CD
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white)



<hr>

## 아키텍처
<img width="1245" height="640" alt="image" src="https://github.com/user-attachments/assets/ec922643-07d6-45ab-bd71-67f747b90ddb" />

본 시스템은 사용자의 사진 진단 및 AI 기반 챗봇 기능을 제공하는 MSA반의 서비스 구조로 설계되어 있습니다. <br>
각 마이크로서비스는 역할에 따라 분리되어 있으며, 독립적인 데이터베이스와 서버 인스턴스를 통해 <br>
도메인별 책임과 데이터 독립성을 보장합니다. 서버 간 통신은 고성능의 gRPC 프로토콜을 기반으로 구성되어, <br>
빠르고 안정적인 서비스 호출을 목표로 합니다. 전체 시스템은 Eureka 기반의 서비스 디스커버리와 <br>
API 게이트웨이를 통해 유기적으로 연결되며, 확장성과 장애 격리에 유리한 구조를 갖추고 있습니다. <br>


| 서비스명               | 설명                                                     |
| ------------------ | ------------------------------------------------------ |
| **API 게이트웨이 서비스**  | 클라이언트와 모든 내부 서비스 간의 요청을 라우팅 및 인증 처리                    |
| **Eureka 서비스**     | 각 서비스의 등록과 디스커버리를 담당하는 중심 허브                           |
| **장소 서비스**         | 장소 관련 기능을 제공하며 `vett_place` 데이터베이스와 연동                 |
| **채팅 서비스**         | 사용자의 일반 대화 처리, `vett_chat` 데이터베이스와 연동                  |
| **인증 및 회원 관리 서비스** | 사용자 인증 및 권한 검증과 회원의 정보를 관리<br>`vett_member` 데이터베이스와 연동 |
| **AI 채팅 서비스**      | AI LLM 기반 대화 처리, `vett_llm` 데이터베이스와 연동                 |
| **AI 진단 서비스**      | 사용자의 사진 기반 진단 처리, `vett_diagnosis` 데이터베이스와 연동          |
| **외부 API 관리 서비스**  | 외부 시스템과의 인터페이스를 담당하는 게이트웨이 역할                          |
| **관리자 서버**         | 관리자 인터페이스 기능 제어                                        |

<br>

### 시스템 아키텍처
각 서비스 기능은 독립적인 서비스로 구성되어 있으며, 통신은 주로 REST API 및 Kafka, gRPC를 통해 이루어집니다. <br>
또한 서비스 전반은 Docker를 통해 컨테이너화되어 관리됩니다. <br>
<img width="1774" height="875" alt="image" src="https://github.com/user-attachments/assets/f1cb842e-e6da-4150-8e1c-f19714e13cbc" />

<br>

<hr>

## 🚀 Core Engineering & Technical Achievements

### 1. 장애 격리를 위한 외부 API(CODEF) 아키텍처 고도화 (Port & Adapter)
**[문제 인식]** 초기 설계에서는 외부 API 통신 로직과 내부 DB 트랜잭션 계층이 강하게 결합되어 있었습니다. 이로 인해 외부 API(CODEF)의 응답 지연이 발생할 경우, 내부 DB 커넥션 풀까지 고갈되어 전체 시스템 장애로 이어질 수 있는 치명적인 구조적 문제를 발견했습니다.

**[해결 및 결과]** 이를 해결하기 위해 백엔드 도메인 계층이 외부 기술 사양에 종속되지 않도록 **Port & Adapter 아키텍처**를 도입했습니다. 인터페이스(Port)를 통해 기술을 격리하고, 이를 구현한 어댑터(WebClient 기반 Caller)를 통해 통신하도록 구조를 개편했습니다. 결과적으로 외부 API의 장애나 지연이 내부 리소스에 미치는 영향을 차단하여 시스템 안정성을 획기적으로 향상시켰습니다.
*(👉 상세 트러블슈팅 내용과 아키텍처 다이어그램은 아래 섹션에서 확인 가능합니다.)*

### 2. WebClient Wrapping을 통한 비동기 통신 표준화 및 로깅 체계 고도화
여러 외부 API(CODEF, OpenAI) 연동 과정에서 팀원마다 서로 다른 통신 방식(RestTemplate vs WebClient)을 혼용하여 코드 일관성과 유지보수성이 저하되는 문제를 겪었습니다. 이를 해결하고자 Spring Boot의 비동기 non-blocking 통신 방식인 **WebClient를 래핑한 커스텀 API Client**를 직접 구축하여 팀 내 표준 통신 모듈로 정착시켰습니다. 이를 통해 코드 컨벤션을 통일하고, 고도화된 로깅 체계를 적용하여 외부 API 통신 시 발생하는 디코딩 에러 및 타입 추적 디버깅 효율을 대폭 증대시켰습니다.

### 3. 공동 여행 목표 기반 스마트 저축 플랜 관리 시스템 (SNPL 모델 구현)
MZ세대의 부채 문제를 해결하기 위한 'Save Now, Pay Later(SNPL)' 금융 모델의 핵심 로직을 구현했습니다. 여러 사용자가 하나의 여행 목표를 공유하고, 각자의 분담금에 맞춰 실시간으로 저축 진행 상황을 관리할 수 있는 **공동 목표 및 여행 플랜 API**를 설계하고 구현했습니다. 사용자의 소비 패턴을 분석하여 목표 달성을 위한 맞춤형 저축 플랜 계산 로직을 연동함으로써, 단순한 기능 구현을 넘어 비즈니스 가치를 창출하는 핵심 로직을 주도적으로 개발했습니다.

<hr>

### Transactional Outbox Pattern
MSA 환경에서 가장 중요한 요소는 서버 간 데이터 동기화입니다. 이를 위해 Transactional Outbox Pattern을 적용하여 <br>
데이터베이스 일관성과 이벤트 발행의 신뢰성을 동시에 보장할 수 있습니다. <br>

#### 패턴 개요
- Outbox 테이블에 데이터 변경과 관련된 이벤트 정보를 저장
- Kafka로 메시지를 안전하게 발행하며 트랜잭션의 원자성 유지
- 장애 발생 시에도 Outbox 테이블의 상태를 통해 후속 처리 가능

#### Outbox 예시
| 필드               | 값                                                     |
| ---------------- | ----------------------------------------------------- |
| `id`             | de06bc17-abe1-4a41-83b8-838a2cf437e8                  |
| `aggregate_type` | User                                                  |
| `event_type`     | UserDeleted                                           |
| `payload`        | `{ "id": "H-87D...", "name": "오규찬", "email": "..." }` |
| `timestamp`      | 2025-04-23 18:30:25.123                               |
| `status`         | READY\_TO\_PUBLISH                                    |

Outbox 상태는 아래와 같은 Enum으로 관리됩니다. <br>

```
public enum OutboxStatus {
  READY_TO_PUBLISH,
  PUBLISHED,
  MESSAGE_CONSUME,
  FAILED,
  PERMANENTLY_FAILED
}
```
#### 패턴 흐름도
![image](https://github.com/user-attachments/assets/0f4adeaf-9fb3-4453-ac69-4fb395289da5)

#### 1단계: 회원 정보 삭제 요청
- 인증 및 회원 관리 서비스에서 회원 탈퇴 요청이 들어옵니다.
- 해당 요청은 트랜잭션 내에서 처리됩니다.


#### 2단계: 트랜잭션 내부 이벤트 처리 (Outbox 기록)
| 시점              | 설명                                                                                                                               |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| `Before_commit` | 트랜잭션 커밋 직전에 **Spring 이벤트 리스너**가 동작하여 Outbox 테이블에 <br> 이벤트(예: `UserDeleted`)를 **비동기적으로 저장**합니다.                                        |
| 저장 내용           | Outbox 테이블에는 `aggregate_type`, `event_type`, `payload`, `timestamp`, `status` 등이 <br> 기록됩니다. `status`는 초기값 `READY_TO_PUBLISH`로 설정됩니다. |


#### 3단계: 트랜잭션 내부 이벤트 처리 (Outbox 기록)
| 시점             | 설명                                                          |
| -------------- | ----------------------------------------------------------- |
| `After_commit` | 트랜잭션이 **성공적으로 커밋된 이후**, Spring의 이벤트 리스너가 Kafka로 메시지를 발행합니다. |
| 메시지 내용         | Outbox 테이블에 저장된 `payload` 데이터가 Kafka 메시지로 전송됩니다.            |
| 상태 변경          | 발행 성공 시, Outbox 테이블의 `status`가 `PUBLISHED`로 변경됩니다.          |


#### 4단계: Kafka 시스템 메시지 전달
- Kafka 시스템은 메시지를 수신하고, 이를 구독 중인 여러 서비스로 전달합니다.


#### 5단계: Kafka 시스템 메시지 전달
| 대상                     | 설명                                                                       |
| ---------------------- | ------------------------------------------------------------------------ |
| **내부 Kafka 리스너**       | 인증 서비스 내 Kafka 리스너가 발행 성공을 확인하고, <br> Outbox 상태를 `MESSAGE_CONSUME` 등으로 갱신합니다. |
| **외부 서비스** | Kafka 메시지를 수신한 뒤, 메시지의 회원 ID를 통해 <br> gRPC로 회원 정보를 조회하거나 삭제 작업을 수행합니다.        |
| **gRPC 서비스 호출**        | Kafka 메시지 내 포함된 ID를 사용해 인증 서비스의 gRPC API로 최신 정보를 요청합니다.                  |
| **데이터 동기화**            | 각 외부 서비스는 gRPC 응답을 기반으로 동기화 테이블을 업데이트 합니다.             |

<br>

#### Kafka 이벤트 발행 – Transactional Outbox Pattern 적용
아래 코드는 Spring의 @TransactionalEventListener를 활용하여 <br>
트랜잭션 커밋 전/후로 Outbox 이벤트를 안전하게 처리하고 Kafka에 발행하는 방식입니다. <br>

<br>

##### BEFORE_COMMIT
```Java
@TransactionalEventListener(phase = TransactionPhase.BEFORE_COMMIT)
public void handleOutboxEvent(OutboxEvent event) {
    memberOutboxService.saveNewOutboxProcess(event);
}
```
- 트랜잭션 커밋 직전에 동작
- Outbox 테이블에 이벤트(event)를 저장하여 데이터와 이벤트 발행 내역을 함께 기록
- 데이터베이스 트랜잭션과 Outbox 동기화 보장

<br>

##### AFTER_COMMIT
```Java
@TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)
@Transactional(propagation = Propagation.REQUIRES_NEW)
@Retryable(
    retryFor = { KafkaSendException.class },
    maxAttempts = 3,
    backoff = @Backoff(delay = 2000),
    recover = "recover"
)
public void handleKafkaEvent(OutboxEvent event) {
    ...
}
```
- 트랜잭션 커밋 후에 실행되며, Kafka로 메시지를 발행
- @Transactional(REQUIRES_NEW)을 통해 별도 트랜잭션으로 메시지 전송
- @Retryable을 사용해 Kafka 발송 실패 시 최대 3회 재시도, 2초 간격
- 발행 성공 시 OutboxStatus.PUBLISHED로 상태 변경
- 실패 시 OutboxStatus.FAILED로 설정하고 KafkaSendException 발생

<br>

##### 실패 이벤트 처리
```Java
@Scheduled(fixedDelay = 60000 * 3) // 3 minute
  public void retryFailedMessages() {
    List<Outbox> failedEvents = memberOutboxService.getFailedOutboxEvents();
    for (Outbox outbox : failedEvents) {
        String topic = memberOutboxService.getMemberKafkaTopic(outbox.getEventType());
        String payload = outbox.getPayload();
        try {
            kafkaTemplate.send(topic,payload);
            log.info("Successfully retried Kafka message, eventId: {}", outbox.getId());
        } catch (Exception e) {
            log.error("Failed to retry Kafka message, eventId: {}", outbox.getId(), e);
        }
    }
}
```
- @Scheduled 애노테이션을 이용해 3분마다 주기적 실행
- OutboxStatus.FAILED 상태인 이벤트를 조회
- Kafka 전송을 재시도하고 성공 시 로그 기록
- 실패 시에도 로깅을 통해 추후 분석 가능

<hr>

## Component & API URI Collection
### Member Component
인증 및 권한 관리, 회원정보 관리를 담당하는 컴포넌트 <br>
![image](https://github.com/user-attachments/assets/281fb738-64b3-40c2-a834-98414108e8fc)

### Member Component API
| URI                                    | Method | 설명                |
| -------------------------------------- | ------ | ----------------- |
| `/auth/identity/sign-up`               | POST   | 사용자 회원가입          |
| `/auth/identity/sign-in`               | POST   | 사용자 로그인           |
| `/auth/identity/reissue`               | POST   | 사용자 접근 토큰 갱신      |
| `/auth/identity/social`                | POST   | 사용자 소셜 로그인        |
| `/auth/identity/is-duplicate/{userId}` | GET    | 사용자 아이디 중복체크      |
| `/auth/identity/find-id`               | POST   | 사용자 아이디 찾기        |
| `/auth/identity/verify-code/{userId}`  | POST   | 이메일 인증을 위한 코드 전송  |
| `/auth/identity/is-verify`             | POST   | 사용자의 인증 코드 유효성 검사 |
| `/auth/identity/password`              | PATCH  | 사용자 비밀번호 변경       |
| `/auth/api/user/{id}`                  | GET    | 사용자 정보 반환         |
| `/auth/api/user/logout`                | POST   | 사용자 로그아웃          |
| `/auth/api/user/{id}`                  | DELETE | 사용자 회원 탈퇴         |
| `/auth/api/user/{id}`                  | PATCH  | 사용자 회원정보 변경       |

<br>

### Chat Component
사용자의 채팅방, 채팅 이력, 채팅방 고정 등을 관리하는 컴포넌트 <br>
![image](https://github.com/user-attachments/assets/f4501859-3207-4e8d-8e4d-1dda88909773)

### Chat Component API
| URI                                             | Method    | 설명                   |
| ----------------------------------------------- | --------- | -------------------- |
| `/chat/room`                                    | POST      | 새로운 그룹 채팅방 생성        |
| `/chat/rooms`                                   | GET       | 모든 그룹 채팅방 반환         |
| `/chat/rooms/{memberId}`                        | GET       | 특정 사용자가 참여중인 채팅방 반환  |
| `/chat/room/{roomId}/{memberId}`                | POST      | 특정 사용자가 그룹 채팅방에 참여   |
| `/chat/room/{roomId}/{memberId}`                | DELETE    | 특정 사용자가 그룹 채팅방 참여 해지 |
| `/chat/room/is-participate/{roomId}/{memberId}` | GET       | 특정 사용자의 채팅방 참여 여부 확인 |
| `/chat/rooms/keyword/{keyword}`                 | GET       | 키워드로 채팅방 검색          |
| `/chat/pin/{roomId}/{memberId}`                 | GET       | 특정 채팅방 즐겨찾기 여부 확인    |
| `/chat/pin/{roomId}/{memberId}`                 | POST      | 채팅방 즐겨찾기 등록          |
| `/chat/pin/{pinId}`                             | DELETE    | 즐겨찾기 삭제              |
| `/chat/message/{roomId}`                        | GET       | 특정 채팅방의 메시지 조회       |
| `/chat/unread-clear/{roomId}/{memberId}`        | POST      | 읽지 않은 메시지 수 초기화      |
| `/chat/unread-count/{roomId}/{memberId}`        | GET       | 읽지 않은 메시지 수 반환       |
| `/chat/message`                                 | WebSocket | pub/sub 방식 메시지 송수신   |

<br>

### LLM Chat Component
사용자의 AI 채팅 이력을 관리하는 컴포넌트 <br>
![image](https://github.com/user-attachments/assets/4fa72840-be37-4fb9-9913-d9b319da02d0)

### LLM Chat Component API
| URI                                         | Method | 설명                  |
| ------------------------------------------- | ------ | ------------------- |
| `/py/llm/chat`                              | POST   | RAG 기반 LLM 모델 채팅 반환 |
| `/py/llm/diagnosis`                         | POST   | 진단 결과에 대한 LLM 응답 반환 |
| `/llm/chat-section/{memberId}/{title}`      | POST   | 사용자 AI 채팅방 개설       |
| `/llm/chat-section/{chatSectionId}`         | DELETE | AI 채팅방 삭제           |
| `/llm/chat-section/{chatSectionId}/{title}` | PATCH  | AI 채팅방 제목 변경        |
| `/llm/chat-sections/{memberId}`             | GET    | 사용자 AI 채팅방 리스트 반환   |
| `/llm/chat/{chatSectionId}`                 | GET    | AI 채팅방의 채팅 정보 조회    |
| `/llm/chat/{chatSectionId}`                 | POST   | AI 채팅방의 채팅 정보 저장    |

<br>

### Place Component
반려동물 동반 가능한 장소 데이터를 관리하는 컴포넌트 <br>
![image](https://github.com/user-attachments/assets/5bc2947e-5569-4716-9886-a4d0a77fc4b0)

### Place Component API
| URI                           | Method | 설명                       |
| ----------------------------- | ------ | ------------------------ |
| `/place/all`                  | GET    | 모든 장소 데이터 반환             |
| `/place/category`             | GET    | 카테고리 별 장소 데이터 반환         |
| `/place/categories`           | GET    | 전체 카테고리 종류 반환            |
| `/place/dist/{category}`      | POST   | 거리순으로 장소 정렬 후 반환         |
| `/place/open/{category}`      | GET    | 현재 운영중인 장소 리스트 (카테고리 필터) |
| `/place/open/dist/{category}` | POST   | 운영중인 장소 거리순 반환 (카테고리 필터) |
| `/place/search/{keyword}`     | GET    | 키워드로 장소 검색               |
| `/place/filter`               | POST   | 필터를 통한 장소 검색             |

<br>

### Diagnosis Component 
반려동물 사진을 통해 진단 서비스를 처리하는 컴포넌트 <br>
![image](https://github.com/user-attachments/assets/22c51131-9349-4bbd-8f92-7627dda8069c)

### Diagnosis Component API ( 개발 중 )
| URI                | Method | 설명            |
| ------------------ | ------ | ------------- |
| `/py/predict/skin` | POST   | 반려동물 피부 질환 진단 |
| `/py/predict/eye`  | POST   | 반려동물 안구 질환 진단 |

<br>

### Integration Component
전체 서비스 컴포넌트를 관리하고 클라이언트 요청 라우팅 및 외부 API 호출을 관리하는 컴포넌트 <br>
![image](https://github.com/user-attachments/assets/30aaa977-8fd1-432c-9ec2-ae9be5ae76a6)

### Integration Component API
| URI                                | Method | 설명              |
| ---------------------------------- | ------ | --------------- |
| `/proxy/api/address-to-coordinate` | POST   | 주소 → 좌표 변환      |
| `/proxy/api/route`                 | POST   | 목적지까지의 교통 정보 반환 |
