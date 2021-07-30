# :question: 기타

#### reference
https://mangkyu.tistory.com/95<br>
https://brunch.co.kr/@adrenalinee31/1<br>
https://kingbbode.tistory.com/26<br>
https://interconnection.tistory.com/74
<hr>

## Question
1. [CORS란 무엇인가요?](#2-cors-cross-origin-resource-sharing)
- CORS는 다른 도메인간의 리소스 공유가 가능하도록 하는 역할을 합니다.
- 본래 다른 도메인끼리 요청을 주고 받는 것은 SOP 정책에 의해 막히는게 기본값이었습니다. 
- 하지만 웹 생태계가 다양해지면서 여러 서비스들간에 보다 자유롭게 데이터가 주고 받아질 필요가 생겼습니다.
- 그래서, 1) JSONP을 통해 해결하거나 2) CORS 방식으로 특정 HTTP 헤더를 추가하여 타도메인과의 자원 공유가 가능하도록 합니다.
<br><br>

2. [CORS 에러가 발생하는 원인을 SOP와 관련하여 설명해주세요.](#1-sop-same-origin-policy)
```
// 클라이언트 5000번 포트, 서버 8080 포트에서 API 요청을 보냈을 때
XMLHttpRequest cannot load 'http://localhost:5000'. No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'http://localhost:8080' is therefore not allowed access.
```
- 도메인(URL)이 다른 2개의 사이트가 요청을 주고 받는 것은 동일 출처 정책(SOP)에 의해 CORS 에러를 발생시킵니다.
- 즉, 보안 상의 이유로 응답을 받지 못하도록 막은 에러이므로, 서버 단에서 특정 도메인을 허용하도록 설정하여 CORS 에러를 해결합니다.
<br><br>

1. [쿠키와 세션의 차이점에 대해 설명해주세요.](#3-쿠키-vs-세션)
- 쿠키는 지정한 만료기간까지 브라우저를 종료해도 계속 남아 있습니다.
- 반면 세션은 만료기간을 지정할 수 있지만, 브라우저가 종료하면 만료기간에 상관없이 삭제됩니다.
<hr>

## :nerd_face:	What I study

### 1. SOP (Same Origin Policy)
- 동일 도메인끼리만 API 등의 데이터 접근이 가능하도록 하는 정책
- 도메인(URL)이 다른 2개의 사이트가 데이터를 주고 받을 때, 데이터 접근이 불가능하도록 막는다.
- 리소스 요청을 보내기 위해선 요청을 보내고자 하는 대상과 **1) 프로토콜, 2) 포트**가 같아야한다. (서브 도메인 네임은 상관없음)
- SOP 정책으로 인해 생기는 문제 => Cross Domain 에러!
<br><br>

### 2. CORS (Cross Origin Resource Sharing)
- SOP를 해결해주는 방식 (Same Origin <-> Cross Origin)
- **다른 출처간에 리소스를 공유할 수 있도록 해주는 역할을 한다.**
- 다른 도메인으로부터 리소스가 요청될 경우, 해당 리소스는 cross-origin HTTP 요청에 의해 요청된다.
- 기본적으로 많은 브라우저들은 보안 상의 이유로(ex. 개인정보 해킹) 스크립트에서의 cross-origin HTTP 요청을 제한한다.
#### 1) JSONP (JSON with Padding)
- JSON data를 callback 함수에 넣어서 전송하는 방식
  - data는 text형이고, callback 함수명으로 감싸서 보낸다.
  - JSONP callback은 서버에서 지원해줘야 사용 가능하다.
- HTML의 script 요소로부터 요청되는 호출에는 보안상 정책이 적용되지 않는다는 점을 이용한 우회 방법<br>즉, JSONP는 데이터 타입 요청이 아닌, ```<script>``` 호출 방식이다.
#### 2) HTTP header (<- CORS 방식)
- 웹 브라우저가 사용하는 정보를 읽을 수 있도록 하기 위해, 허가된 출처 집합을 서버에게 알려주도록 허용하는 특정 HTTP header를 추가한다.
```
1. 클라이언트는 요청할 때, header Origin 필드에 요청 출처를 남긴다.
2. 서버는 응답할 때, 헤더의 Access-Control-Allow-Origin 필드에 서버가 허용하는 출처를 남긴다.
3. 이때 클라이언트는 자신의 Origin과 Access-Control-Allow-Origin을 비교한다.
4. 비교한 값이 같을 경우 응답을 받고, 아니면 응답을 버린다.
```
- **도메인을** * **으로 설정하면, 모든 도메인에 대해 리소스 요청을 허락한다.**

|HTTP Header||
|:---:|:---:|
|Access-Control-Allow-Origin|접근 가능한 ```url``` 설정|
|Access-Control-Allow-Credentials|접근 가능한 ```쿠키``` 설정|
|Access-Control-Allow-Headers|접근 가능한 ```헤더``` 설정|
|Access-Control-Allow-Methods|접근 가능한 ```http method``` 설정|

![cors](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdPuRqo%2FbtqK85aXnRY%2FApvYbxghCX73bagiCaf6lK%2Fimg.png)
<br><br>

### 3. 쿠키 vs 세션
- HTTP 프로토콜 환경에서 연결이 시작되면, 서버는 클라이언트의 상태를 항상 확인해야 한다.
- 그러므로 HTTP 프로토콜의 특징이자 약점인 stateless, connectionless를 보안하기 위해서 사용한다.

## ** 쿠키와 세션의 차이를 물어볼 때, 라이프사이클이 가장 중요함! **
||쿠키|세션|
|---|:---:|:---:|
|저장위치|Client|Server|
|저장형식|Text|Object|
|라이프사이클|쿠키에 지정된 기간까지|브라우저 종료될 때까지|
|용량제한|~4KB|서버가 허용하는 한, 제한 X|
|속도|빠름|느림|
|보안|Bad|Good|

- 라이프사이클
  - 쿠키: 지정한 기간까지 브라우저를 종료해도 계속 남아 있는다.
  - 세션: 만료시간을 지정할 수 있지만, 브라우저가 종료하면 만료시간에 상관없이 삭제된다.
- 속도
  - 쿠키: 클라이언트에 모든 데이터가 저장되어 있어서 서버에 요청할 때 속도가 빠르다.
  - 세션: 실제 저장된 정보가 서버에 있어, 서버에서 처리가 필요해 속도가 느리다.
- 보안
  - 쿠키: 클라이언트에 저장되므로 보안에 취약하다.
  - 세션: 쿠키를 이용해 session id만 저장하고, 서버에서 id를 보고 처리하므로 비교적으로 보안이 좋다.

#### 1) 쿠키 동작방식
```
1. 클라이언트의 요청
2. 웹 서버에서 쿠키를 생성
3. 응답 시에 HTTP 헤더에 쿠키를 포함하여 전달
4. 클라이언트 로컬 PC에 쿠키 저장
5. 브라우저가 만료되도 지정된 기간까지 클라이언트에서 보관
6. 동일 요청시, 쿠키가 존재하면 HTTP 헤더에 쿠키를 포함하여 전달
7. 서버에서 쿠키를 읽어, 이전 상태 정보를 변경 할 필요가 있을 때 쿠키를 업데이트하여 변경된 쿠키를 HTTP 헤더에 포함시켜 응답
```
#### 2) 세션 동작방식
- 클라이언트가 요청을 보내면, 해당 서버의 엔진이 클라이언트에게 유일한 session id를 부여한다.
- 세션을 사용한 인증 방식은 인증에 성공했을 때마다, 세션 값을 저장하기 때문에 데이터베이스에 부하가 걸릴 수 있다.
```
1. 클라이언트의 요청
2. 서버에 요청 시, session id를 서버에 전달
3. 서버는 클라이언트의 HTTP request header의 필드인 Cookie를 확인하고, 어떤 session id를 보냈는지 확인
4. 저장된 session id가 없다면 생성하여 클라이언트에게 전달
5. session id를 서버에 저장
6. 클라이언트 재접속 시, 가지고 있는 session id 값을 서버에 전달
```
#### 3) 왜 항상 세션을 사용하지 않나요?
- 세션은 서버의 자원을 사용한다.
- 그러므로 과도하게 사용하면 서버의 메모리가 과도하게 사용되어 과부하가 걸릴 수 있고 속도가 느려질 수 있다.

#### 4) 쿠키, 세션과 캐시의 차이점?
- 캐시는 정적 파일, css, js파일 등을 브라우저, 서버 앞단에 저장해두고 사용하는 것이다.
- 한 번 캐시에 저장되면 브라우저를 참고한다. 그렇기 때문에 서버에서 변경이 되어도 사용자에는 변경되지 않게 보일 수 있다.
  - 이를 해결하기 위해선, 캐시를 지워주거나 응답 header에 캐시 만료시간을 명시해주는 방법 등이 있다.
<br><br>

### 4. JWT 인증 방식 (JSON Web Token)
- 두 객체가 JSON 객체를 사용하여 필요한 모든 정보를 안전하게 전달하는 방법
```
| aaaa | . |   bbbb  | . |    cccc   |
|header|   | payload |   | signature |
```

#### 1) Header (헤더)
- 토큰 타입(typ) 지정
- 해싱 알고리즘(alg) 지정
```
{
    "typ" : "JWT",
    "alg" : "HS256"
}
```
#### 2) Payload (정보)
- 토큰에 담을 정보가 들어있다.
- (name - value)의 쌍으로 이루어져 있다.
- 여러 개의 클레임(정보의 한 조각)을 넣을 수 있지만, 너무 많아지면 토큰의 길이가 길어질 수 있다.
  - 등록된 클레임: 토큰에 대한 정보를 담기 위해 이미 정해진 클레임
  - 공개 클레임: 충돌이 방지된 이름을 가져야 한다. 이를 위해서, URL 형식으로 짓는다.
  - 비공개 클레임: 클라이언트/서버가 협의하여 사용되는 클레임 이름, 이름이 중복되어 충돌될 수 있으므로 사용할 때 유의해야 한다.

#### 3) Signature (서명)
- header와 payload의 인코딩 값을 합친 뒤, 비밀키로 hash하여 생성한다.
<hr><br>

![jwt](https://camo.githubusercontent.com/32bdb18e5014d84369fc5316b03000ae9428a79c8d857beb4cf4f2931fa0bd9f/68747470733a2f2f7374617469632e7061636b742d63646e2e636f6d2f70726f64756374732f393738313738343339353430372f67726170686963732f4230333635335f30385f30322e6a7067)
- 로그인 시, access token을 전달받는다.
  - 유효기간이 짧은 토큰을 사용하면, 자주 로그인 해야하는 번거로움이 발생한다.
  - 유효기간이 긴 토큰을 사용하면, 제 3자에게 탈취당할 수 있는 문제가 발생한다.
  - 이러한 문제를 보안하기 위해, JWT는 refresh token
  - 을 사용한다.
- 토큰은 전달받은 후, 검증만 하면 되기 때문에 서버에 부담이 덜하다.
- access token의 유효기간이 만료되면, referech token을 통해 새로 발급받을 수 있다.
  - access token을 강제로 만료시킬 수 없다는 단점이 있다.

##### 참고: https://github.com/gyoogle/tech-interview-for-developer/blob/master/Web/JWT(JSON%20Web%20Token).md