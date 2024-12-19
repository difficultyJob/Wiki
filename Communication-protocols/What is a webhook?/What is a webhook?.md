## 웹훅이란?


> **웹훅(Webhook)**
>
> 데이터가 변경되었을 때 서버가 클라이언트에 실시간으로 보내는 HTTP 기반 콜백 메커니즘


- 웹 서비스의 이벤트 데이터를 전달하는 HTTP 기반 콜백 함수
- 특정 이벤트가 발생하면 웹훅이 클라이언트에게 이벤트 데이터를 보냄
- **HTTP 기반의 웹 특징**과 **훅(Hook)** 기능을 합친 용어
- HTTP를 사용하기 때문에 새로운 인프라를 추가하지 않고도 웹 서비스와 쉽게 통합될 수 있음

## 웹훅의 동작 방식

1. **URL 등록** : 클라이언트는 이벤트 발생 시 서버가 호출할 URL을 서버에 등록한다.
2. **이벤트 발생** : 서버에서 특정 이벤트가 발생하면 사전에 등록된 URL로 HTTP 요청을 보낸다.
3. **데이터 처리** : 클라이언트는 서버에서 전달받은 데이터를 처리한다.

## 웹훅과 API 폴링의 차이

![alt text](image.png)

### 예시

- **API 폴링** : 친구가 받을 때까지 계속 전화 🤷📞
- **웹훅** : “시간 나면 전화 줘~” 라고 문자 💬

### **API 폴링(Polling)**

- 클라이언트가 주기적으로 서버 API를 호출해 이벤트가 발생했는지 확인하는 방식

**장점**

| 장점 | 설명 |
| --- | --- |
| **간단한 구현** | 단순한 HTTP 요청만으로 동작하므로 서버와 클라이언트 간 통신 구조가 간단하여 구현이 쉬움 |
| **일정한 상태 확인** | 주기적으로 상태를 확인하여 데이터의 최신 데이터를 일정한 간격으로 확보할 수 있음 |
| **독립적인 운영** | 서버가 클라이언트를 호출하지 않으므로 보안 및 네트워크 설정이 단순 |

**단점**

| 단점 | 설명 |
| --- | --- |
| **리소스 낭비**  | 이벤트가 발생하지 않아도 반복적인 요청으로 리소스를 소모 |
| **비효율성** | 이벤트 발생 후 실시간으로 데이터를 받기 어려움 |
| **확장성 문제** | 많은 클라이언트가 동시에 요청 하면 서버 부하 증가 |

### 웹훅

- 서버에서 특정 이벤트가 발생했을 때 클라이언트를 호출하는 방식
- 서버에서 특정 이벤트가 발생했을 때 클라이언트를 호출하는 방식 → **역방향 API** 또는 **푸시 API**라고도 불림

**장점**
| 장점 | 설명 |
| --- | --- |
| **효율적 리소스 사용** | 서버가 필요한 경우에만 데이터를 전달하므로 서버와 클라이언트의 리소스 사용을 최소화 |
| **실시간 데이터 전달** | 클라이언트가 즉시 데이터를 받아 빠른 처리 가능 |
| **간결한 데이터 흐름** | 불필요한 반복 요청이 없어 클라이언트와 서버의 코드가 단순화되고 유지보수가 용이 |

**단점**
| 문제 | 설명 |
| --- | --- |
| **신뢰성 문제**    | 서버가 웹훅 요청을 실패하거나 클라이언트가 수신하지 못하는 경우 데이터가 유실될 수 있음 → 해결 방안 : 재시도 메커니즘 및 백업 폴링 설정으로 보완 가능 |
| **보안 문제**    | 외부 URL로 요청을 보내는 방식이므로, URL이 노출되면 악용 가능성이 있음<br>→ 해결 방안 : 인증 토큰, IP 화이트리스트, HTTPS를 통한 보안 강화 |
| **복잡한 설정**    | 클라이언트에서 URL을 등록하고 서버와의 인증 체계를 설계해야 하는 추가 작업이 필요함 |


## 웹훅 설계 시 주의할 점

1. **API 설계**
    - 서버가 클라이언트를 호출할 API의 구조와 데이터를 명확히 정의해야 한다.
        - 예) 요청 메서드(GET, POST), 데이터 형식(JSON/XML), 필수 필드 등 명확히 정의
    - 클라이언트는 웹훅 요청에 성공/실패를 명확히 응답해야 하고, 요청이 실패했을 때 서버가 재시도할지 여부를 결정해야 한다.
2. **보안**
    - 서버가 요청을 보낼 때, 헤더에 인증 토큰을 포함해 요청의 신뢰성을 보장한다.
    - IP 화이트리스트를 이용해 서버가 웹훅 요청을 보낼 수 있는 IP 범위를 제한해 보안을 강화한다.
    - 데이터를 암호화해 중간에서 탈취당하지 않도록 HTTPS를 사용한다.
3. **URL 관리**
    - 웹훅 요청이 올 URL을 클라이언트에서 서버에 정확히 등록해야 한다. URL 변경이 필요할 경우 적절한 재등록 절차가 필요하다.
    - 등록한 URL이 정상적으로 동작하는지 주기적으로 점검해야 한다.

## 참고

https://docs.tosspayments.com/resources/glossary/webhook

[https://ko.wikipedia.org/wiki/웹훅](https://ko.wikipedia.org/wiki/%EC%9B%B9%ED%9B%85)