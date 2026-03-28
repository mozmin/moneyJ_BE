<img width="1800" height="300" alt="money J(노션용2)" src="https://github.com/user-attachments/assets/a6e65a0f-d245-4bb3-92b0-fffd5f2bb9eb" />

<br>
<br>

> 건강한 저축 습관 형성을 위한 소비 패턴 기반 여행 저축 가이드 서비스

## Core Contributions (핵심 기여도)

### 1. 실시간 금융 데이터 연동 파이프라인 구축
  - CODEF API를 활용하여 은행 계좌 및 카드 거래 데이터를 수집하는 안정적인 데이터 파이프라인 설계.
  - 보안을 위한 RSA 암복호화 및 API 응답 디코딩 로직 구현으로 데이터 무결성 확보.

### 2. 공동 금융 비즈니스 로직 API 설계
  - 다수의 사용자가 함께 참여하는 공동 여행 플랜 및 그룹 저축 시스템의 핵심 API 설계 및 구현.
  - 복잡한 그룹 예산 산출 로직과 상태 관리 시스템을 구축하여 협업 금융 기능의 안정성 강화.

<br>

## 2. 개발 환경
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

## 3-1. 소프트웨어 아키텍처
<img width="1245" height="640" alt="image" src="https://github.com/user-attachments/assets/ec922643-07d6-45ab-bd71-67f747b90ddb" />

<br>
<br>

본 시스템은 사용자의 금융 데이터 통합 관리 및 AI 기반 맞춤형 자산 분석 기능을 제공하는 'moneyJ' 서비스의 백엔드 아키텍처로 설계되어 있습니다. <br>
각 도메인(계좌, 카드, 거래, 여행 등)은 역할에 따라 모듈 단위로 분리되어 있으며, Spring Boot의 계층형 아키텍처를 통해 <br>
비즈니스 로직의 독립성과 유지보수성을 보장합니다. 외부 금융 API인 CODEF와의 연동을 통해 실시간 자산 정보를 수집하고, <br>
OpenAI LLM을 활용한 지능형 소비 진단 및 여행 예산 가이드를 제공하는 것이 특징입니다. 전체 시스템은 Docker 기반의 <br>
컨테이너 환경에서 운용되며, Prometheus를 통한 실시간 모니터링 체계를 갖추어 서비스의 안정성과 확장성을 확보하고 있습니다. <br>


| 서비스명               | 설명                                                     |
| ------------------ | ------------------------------------------------------ |
| **인증 및 보안 서비스**  | 카카오 OAuth2 소셜 로그인 연동 및 JWT 기반의 보안 인증/인가 처리                        |
| **금융 계정 관리 서비스**     | 은행 계좌 및 카드 정보의 연결, 동기화, 상태 관리를 담당하는 서비스                 |
| **거래 내역 관리 서비스**         | 실시간 거래 내역 수집 및 소비 데이터의 저장, 조회 및 동기화 스케줄링 수행      |
| **금융 API 연동 서비스 (Codef)**         | 외부 금융망(CODEF)과의 인터페이스를 담당하며 인증 토큰 및 API 통신 제어 |
| **AI 분석 및 진단 서비스** | 거래 데이터를 분석하고 OpenAI와 연동하여 맞춤형 절약 팁 및 소비 요약 생성             |
| **여행 및 예산 관리 서비스**      | 여행 일정별 예산 수립 및 AI 기반의 여행 경비 최적화 가이드 제공                |
| **사용자 정보 관리 서비스**      | 회원 프로필 관리 및 애플리케이션 내 사용자 기반 정보 관리                       |
| **공통 인프라 모듈**  | 전역 예외 처리(Exception), QueryDSL 설정, 비동기 처리 등 시스템 공통 기능 제공             |
| **모니터링 및 운영**         | Prometheus를 통한 시스템 지표 추적 및 Docker 기반 컨테이너 인프라 관리              |


<br>

### 3-2. 시스템 아키텍처
각 서비스 기능은 독립적인 서비스로 구성되어 있으며, 통신은 주로 REST API 및 Kafka, gRPC를 통해 이루어집니다. <br>
또한 서비스 전반은 Docker를 통해 컨테이너화되어 관리됩니다. <br>
<img width="1774" height="875" alt="image" src="https://github.com/user-attachments/assets/f1cb842e-e6da-4150-8e1c-f19714e13cbc" />

<br>

<hr>

## 4. Technical Achievements

### 4-1. CODEF API
**External Financial Data Pipeline (CODEF API)** <br>
금융 서비스의 핵심은 신뢰할 수 있는 데이터를 안전하게 가져오는 것입니다. 이를 위해 CODEF API를 활용하여 사용자의 은행/카드 데이터를 수집하는 전용
파이프라인을 구축하였습니다.

<br>

**파이프라인 개요**
- 보안 중심 설계: RSA-2048 암호화를 통해 사용자 민감 정보(계정 정보 등)를 안전하게 보호
- Connected ID 체계: 일회성 인증이 아닌, 지속적인 데이터 수집을 위한 고유 식별자 관리
- 정규화 및 디코딩: 복잡한 API 응답 데이터를 서비스 도메인 모델로 변환하는 전용 디코더 구현

<br>

**핵심 데이터 구성**

| 요소 | 설명 |
| :--- | :--- |
| **CODEF Access Token** | API 호출을 위한 2단계 인증 토큰 (DB 저장 및 만료 주기 관리) |
| **Connected ID** | 사용자의 금융 기관 계정과 1:1 매핑되는 고유 식별 코드 |
| **RSA Encryption** | 암호, 계좌번호 등 민감 데이터 전송 시 적용되는 비대칭키 암호화 방식 |
| **Response Decoding** | Base64 및 URL 인코딩된 금융 응답 데이터의 가독화 처리 |

<br>

**데이터 연동 흐름도** 
<img width="2400" height="1786" alt="Gemini_Generated_Image_oiakvloiakvloiak" src="https://github.com/user-attachments/assets/fc8f0faf-5c1b-4b5e-81c0-575340d350ab" />

1. 인증 단계 (OAuth 2.0): 서비스 서버와 CODEF API 간의 Client ID/Secret 기반 토큰 발급
2. 계정 연결 단계 (Connected ID): 사용자 금융 인증 정보를 RSA 암호화하여 전송 후 고유 ID 발급 및 저장
3. 데이터 수집 단계 (Scraping): 저장된 Connected ID를 사용하여 은행/카드사의 거래 내역 실시간 요청
4. 가공 및 저장 단계 (Decoding): 응답 데이터를 디코딩하고, 서비스 요구사항에 맞춰 정규화 후 DB 반영
   
---

**1단계: 보안 전송을 위한 RSA 암호화** 
사용자의 금융 계정 정보는 매우 민감하므로, CODEF에서 요구하는 공공기관 수준의 RSA 암호화 방식을 적용하였습니다.

(RsaEncryptor.java)
``` java
public class RsaEncryptor {

    @SneakyThrows
    public static String encryptWithPemPublicKey(String plain, String pem) {

        String base64 = pem.replace("-----BEGIN PUBLIC KEY-----", "")
                .replace("-----END PUBLIC KEY-----", "")
                .replaceAll("\\s", "");

        byte[] der = Base64.getDecoder().decode(base64);

        PublicKey key = KeyFactory.getInstance("RSA")
                .generatePublic(new X509EncodedKeySpec(der));

        Cipher cipher = Cipher.getInstance("RSA/ECB/PKCS1Padding");
        cipher.init(Cipher.ENCRYPT_MODE, key);

        byte[] enc = cipher.doFinal(plain.getBytes(java.nio.charset.StandardCharsets.UTF_8));

        return Base64.getEncoder().encodeToString(enc);
    }
}
```
- 역할: 사용자 비밀번호 및 인증 정보를 CODEF 서버로 전송하기 전 암호화
- 특징: RSA/ECB/PKCS1Padding 등의 표준 보안 규격 준수

<br>

**2단계: 안정적인 API 통신 및 응답 처리** (API Client)

네트워크 지연이나 외부 API 오류에 대응하기 위해 공통 API 클라이언트를 구축하고, 복잡한 응답 형식을 처리하는 디코더를 구현했습니다.

(ApiResponseDecoder.java)
```java
@Slf4j
public class ApiResponseDecoder {

    private static final ObjectMapper objectMapper = new ObjectMapper()
            .configure(DeserializationFeature.ACCEPT_SINGLE_VALUE_AS_ARRAY, true);

    /**
     * URL 인코딩된 응답을 디코딩하고 지정된 DTO 타입으로 파싱
     */
    public static <T> T decode(String encodedResponse, TypeReference<T> typeReference) {
        // (입력값 검증 생략)

        try {
            // 1. URL 디코딩 (StandardCharsets.UTF_8)
            String decodedJson = URLDecoder.decode(encodedResponse, StandardCharsets.UTF_8);

            // 2. Jackson ObjectMapper를 이용한 객체 매핑
            return objectMapper.readValue(decodedJson, typeReference);

        } catch (Exception e) {
            log.error("API 응답 디코딩 또는 파싱 실패", e);
            throw MoneyjException.of(CodefErrorCode.PARSING_ERROR);
        }
    }

    // (Map 형태의 범용 디코딩 메서드 오버로딩 생략...)
}
```

- 역할: 응답 데이터(Base64) 디코딩 및 JSON 파싱을 통한 도메인 객체 변환
- 특징: 응답 코드(CF-00000 등)에 따른 세분화된 예외 처리 시스템 구축

<br>

**3단계: 스케줄링 기반 데이터 동기화** 

사용자가 앱을 열지 않아도 거래 내역이 최신화될 수 있도록 백그라운드 동기화 로직을 설계했습니다.

(AccountSyncScheduler.java)
```java
@Service
@RequiredArgsConstructor
public class AccountSyncScheduler {

    private final AccountRepository accountRepository;
    private final AccountProvider accountProvider;

    /**
     * 매 6시간마다 전체 계좌 스냅샷 동기화
     */
    @Transactional
    @Scheduled(cron = "0 0 0,6,12,18 * * *")
    public void syncAllAccountsSnapshot() {
        List<Account> accounts = accountRepository.findAll();
        if (accounts.isEmpty()) return;

        // 중복 API 호출 방지를 위한 로컬 캐시 (userId + orgCode 조합)
        Map<String, List<ExternalAccountDTO>> cache = new HashMap<>();

        for (Account account : accounts) {
            // (유효성 검사 생략)
            String key = account.getUser().getUserId() + "|" + account.getOrganizationCode();

            // 1. 캐시 확인 및 외부 API 호출 (computeIfAbsent 활용)
            List<ExternalAccountDTO> externalAccounts = cache.computeIfAbsent(key, k -> {
                return accountProvider.fetchBankAccounts(account.getUser().getUserId(), account.getOrganizationCode());
                // (응답 결과에 따른 빈 리스트 반환 로직 생략)
            });

            // 2. 계좌 번호 매칭을 통한 잔액 업데이트
            externalAccounts.stream()
                    .filter(acc -> account.getAccountNumber().equals(acc.accountNumber()))
                    .findFirst()
                    .ifPresent(match -> account.updateBalance(match.balance()));
        }
        
        // (완료 로그 출력 생략)
    }
}
```

- 역할: Connected ID 리스트를 순회하며 정기적으로 금융 데이터(잔액, 내역) 동기화
- 특징: API 호출 제한(Rate Limit)을 고려한 배치 처리 및 장애 시 재시도 전략 적용

<br>

---

### 4-2. CODEF API 트러블슈팅

### 기존 구조의 문제점

1. @Transactional이 적용된 하나의 거대한 클래스(God Class) 내에서 외부 API 호출과 DB 저장이 동시에 발생. 외부 API 지연 시 DB 커넥션 풀이 고갈될 위험이 존재.

3. 도메인과 외부 인프라의 강한 결합

3. 계좌 하나를 연동하기 위해 클라이언트(프론트엔드)가 백엔드로 3번 연속 API를 호출해야 하는 무거운 구조.

4. API 응답을 Map<String, Object>로 받아 하드코딩으로 파싱


### 핵심 개선 사항

<br>

**외부 API 통신과 DB 트랜잭션의 완벽한 분리**

거대 클래스를 역할에 따라 3개의 계층으로 완전히 분리
  
| 클래스명 | 주요 역할 | 트랜잭션 (@Transactional) | 특징 및 의존성 |
| :--- | :--- | :---: | :--- |
| **CodefCredentialFacade** | 전체 흐름 제어 (Orchestration) | 미적용 | 서비스와 API 호출자 사이의 로직 조율 |
| **CodefCredentialApiCaller** | 외부 API 통신 및 데이터 파싱 | 미적용 | **DB 의존성 없음**, 외부 통신 및 데이터 가공 전담 |
| **CodefInstitutionService** | DB 저장 및 영속성 관리 | **적용** | DB CRUD 및 데이터 정합성 보장 |

<br>

**WebClient Wrapping을 통한 비동기 통신 표준화 및 로깅 체계 고도화**

여러 외부 API(CODEF, OpenAI) 연동 과정에서 팀원마다 서로 다른 통신 방식(RestTemplate vs WebClient)을 혼용하여 코드 일관성과 유지보수성이 저하.
이를 해결하고자 Spring Boot의 비동기 non-blocking 통신 방식인 WebClient를 래핑한 커스텀 API Client를 직접 구축하여 팀 내 표준 통신 모듈로 정착

```java
@Component
@RequiredArgsConstructor
public class CodefApiClient {

    private final WebClient codefWebClient;
    private final CodefAuthService codefAuthService;

    /**
     * 외부 API와 실제 통신 수행
     */
    public String executePost(String url, Object body) {
        String token = codefAuthService.getValidAccessToken();

        return codefWebClient.post()
                .uri(url)
                .header(HttpHeaders.AUTHORIZATION, "Bearer " + token)
                .bodyValue(body)
                .exchangeToMono(resp -> {
                    // 응답 상태(4xx, 5xx)에 따른 로깅 및 처리 생략...
                    return resp.bodyToMono(String.class).defaultIfEmpty("");
                })
                .block(); // 외부 연동의 정합성을 위해 동기 처리
    }

    /**
     * 2. 응답 파싱 + 비즈니스 검증 공통화
     * @param <T> 기대하는 데이터 모델 타입
     */
    public <T> T fetchAndDecode(String url, Object body, TypeReference<CodefResponseDTO<T>> typeReference) {

        // 1) API 호출 (Raw 데이터 획득)
        String rawResponse = executePost(url, body);

        // 2) JSON 파싱 (Generic 대응)
        CodefResponseDTO<T> responseDTO = ApiResponseDecoder.decode(rawResponse, typeReference);

        // 3) 비즈니스 에러 검증 (CODEF 결과 코드가 '성공'인지 확인)
        if (responseDTO == null || !responseDTO.result().isSuccess()) {
            throw MoneyjException.of(CodefErrorCode.BUSINESS_ERROR);
        }

        return responseDTO.data(); // 최종 정제된 데이터 반환
    }
}
```

<br>

**ACL(Anti-Corruption Layer)과 Adapter 패턴 도입**

외부 시스템의 변경이 핵심 도메인을 오염시키지 않도록 **ACL(부패 방지 계층)**을 구축.

도메인 계층에는 CardProvider 등 순수 인터페이스(Port)만 정의.

인프라 계층의 CodefCardAdapter가 이를 구현하며, 외부 API 데이터를 우리 서비스만의 표준 DTO(ExternalAccountDTO)로 번역하여 전달.

<br><br>

**클라이언트 네트워크 비용 감소 및 CQRS 적용**

API 통합: 클라이언트가 3번 호출하던 연동 프로세스(ID발급 → 기관등록 → 조회)를 1개의 /connect API로 통합하여 클라이언트의 부담을 줄이고 응답 속도를 개선.

명령과 조회 분리: 조회와 DB 저장이 혼재되어 있던 기존 메서드를 순수 조회(fetchCards)와 저장(linkCard) 명령으로 완벽히 분리하여 사이드 이펙트를 차단.

<br>

**DTO 기반의 안전한 데이터 파싱 구현**

불안정한 Map 파싱을 걷어내고 공통 응답 DTO를 구축.

CODEF API 특유의 가변적인 응답 규격(성공 시 배열, 실패 시 객체 반환)을 처리하기 위해 Jackson의 ACCEPT_SINGLE_VALUE_AS_ARRAY 옵션을 적용한 커스텀 디코더(ApiResponseDecoder)를 구현하여 타입 안정성을 100% 확보.

<br>

### 결과 (Result)

- 안정성 및 확장성 확보: 외부 장애가 내부 시스템으로 전파되지 않는 구조를 만들었고, 향후 금융 데이터 제공자가 변경되더라도 유연하게 대응할 수 있는 기반을 마련.

- 예측 가능한 코드: 객체의 책임을 명확히(SRP) 분리하고 Facade 패턴을 도입함으로써, 메서드 이름만 보고도 동작을 예측할 수 있는 유지보수성 향상.


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

<br>

### Chat Component
사용자의 채팅방, 채팅 이력, 채팅방 고정 등을 관리하는 컴포넌트 <br>
![image](https://github.com/user-attachments/assets/f4501859-3207-4e8d-8e4d-1dda88909773)

<br>

### LLM Chat Component
사용자의 AI 채팅 이력을 관리하는 컴포넌트 <br>
![image](https://github.com/user-attachments/assets/4fa72840-be37-4fb9-9913-d9b319da02d0)

<br>

### Place Component
반려동물 동반 가능한 장소 데이터를 관리하는 컴포넌트 <br>
![image](https://github.com/user-attachments/assets/5bc2947e-5569-4716-9886-a4d0a77fc4b0)


<br>

### Diagnosis Component 
반려동물 사진을 통해 진단 서비스를 처리하는 컴포넌트 <br>
![image](https://github.com/user-attachments/assets/22c51131-9349-4bbd-8f92-7627dda8069c)


<br>

### Integration Component
전체 서비스 컴포넌트를 관리하고 클라이언트 요청 라우팅 및 외부 API 호출을 관리하는 컴포넌트 <br>
![image](https://github.com/user-attachments/assets/30aaa977-8fd1-432c-9ec2-ae9be5ae76a6)
