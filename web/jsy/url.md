# :question: URL

#### reference
https://hanamon.kr/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EA%B8%B0%EB%B3%B8-url-uri-urn-%EC%B0%A8%EC%9D%B4%EC%A0%90/ <br>
https://ssungkang.tistory.com/entry/WEB-URI-vs-URL-vs-URN-%EB%B9%84%EA%B5%90-%EB%B6%84%EC%84%9D<br>
https://velog.io/@inyong_pang/URI-URL-URN-5gk5pz1o4s
<hr>

## Question
1. URI, URL, URN이란?
- **URL**은 **자원을 어떻게 얻을 것이고 어디에서 가져와야하는지 명시**하는 문자열이고, **URN**은 자원을 어떻게 접근할 것인지 명시하는 것이 아니라 **자원 자체를 특정하는** 문자열이다.
- **URI**는 URL과 URN을 포함하는 개념으로, 자원을 가리키는 유일한 주소이다.

<hr>

## :nerd_face:	What I study

![diagram](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcsPw40%2FbtqLkgwQyPh%2Fnm67jkkVSEPF5kT9gRwyy0%2Fimg.png)

<br>

![diff](https://i.stack.imgur.com/2iD7U.jpg)

### 1. URI

```
https://github.com/jjuyeon/Tech-Interview-Study/issues/78
```

- Uniform Resource Identifier (통합 자원 식별자)
- 하나의 자원을 가리키는 유일한 주소 
  - 자원: HTTP에서 요청한 대상
  - 자원을 고유하게 식별하고 위치를 지정할 수 있다.
- URL과 URN을 포함하는 개념

<br>

### 2. URL

```
https://www.website.com/foldername/folderfile.pdf

http://img.naver.net/static/www/dl_qr_naver.png - 네이버 앱QR코드의 이미지에 대한 URL
```

- Uniform Resource Locator (통합 자원 지시자)
- 자원의 location을 나타내는데 사용된다.
  - web에서 자원이 어디 있고 어떻게 접근할 수 있는지에 대한 구체적인 위치를 나타낸다.
- 우리가 아는 일반적인 웹 주소 형식
  - **요즘 대부분의 URI는 URL이다.**
  - Web Server(ex. Apache, Nginx)가 rewrite기술로 인해, URL을 단순히 특정 파일의 위치를 나타내는 것보다 자원 식별자(URI)로서 사용되고 있다.

<br>

### 3. URN

```
urn:isbn:0451450523 - URN으로 1926년에 출간된 the Last Unicorn의 도서식별번호

urn:oid:2.16.840 - URN으로 미국을 의미하는 OID이다
```

- Uniform Resource Name (통합 자원 이름)
- 자원의 name을 나타내는데 사용된다.
- 자원의 위치에 영향을 받지 않는 유일무이한 이름을 나타낸다.
- 자원을 어느 곳으로 옮기더라도 문제없이 동작한다.
- 자원의 이름이 변하지 않는 한, 여러 종류의 네트워크 접속 프로토콜(ex. http, https)로 접근해도 문제가 없다.
  - 무엇인지를 말하는 What의 개념으로 서로 중복되지 않는 유일한 값이어야한다.
  - 하나의 자원에는 절대로 겹치는 urn이 있으면 안된다. (불변, 유일)
- 실제 자원을 찾기 위해서 urn을 url로 변환하여 이용한다.
- 아직 널리 채택되지 않았다!
