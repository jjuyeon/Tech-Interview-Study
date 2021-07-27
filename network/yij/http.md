# 7계층 프로토콜 HTTP

## HTTP 프로토콜

웹을 만드는 기술들

- 필수

  > **HTTP(HTTPS -> SSL/TLS), HTML, Javascript, CSS**, ASP/ASP.NET, **JSP**, PHP, DB

- 선택
  > Python, Spring, Jquery, Ajax

<br>

## HTTP 프로토콜

: HyperText Transfer Protocol(하이퍼 텍스트 전송 프로토콜)

> www에서 쓰이는 핵심 프로토콜로 문서의 전송을 위해 쓰이며, 오늘날 거의 모든 웹 애플리케이션에서 사용되고 있다.  
>  -> 음성, 화상 등 여러 종류의 데이터를 MIME로 정의하여 전송 가능

### HTTP 특징

: Request/Response (요청/응답) 동작에 기반하여 서비스 제공

### HTTP 1.0

![httpdiff](img/httpdiff.png)

: "연결 수립, 동작, 연결 해제"의 단순함이 특징 -> 하나의 URL은 하나의 TCP 연결
HTML 문서를 전송 받은 뒤 연결을 끊고 다시 연결하여 데이터를 전송한다

#### 문제점: 단순 동작(연결 수립, 동작, 연결 해제)이 반복되어 통신 부하 문제 발생

### HTTP Request Protocol

![req](img/request.png)

: 요청하는 방식을 정의하고 클라이언트의 정보를 담고 있는 Request Protocol 구조

- Request Line
- Header
- Empty Line
- Body

#### Request Line

| **요청 타입** | 공백 | **URI** | 공백 | HTTP 버전 |
| ------------- | ---- | ------- | ---- | --------- |

##### HTTP Method

![method](img/method.jpg)

| GET                                              | POST                                     |
| ------------------------------------------------ | ---------------------------------------- |
| 특정 데이터를 요청                               | 클라이언트가 서버로 데이터를 전송        |
| URI에 요청 데이터를 query string으로 붙여서 전송 | HTTP Request Body에 데이터를 넣어서 전송 |
