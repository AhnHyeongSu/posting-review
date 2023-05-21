# HTTP 캐싱

**캐시는 어떻게 동작할까?**

- 캐시가 존재하지 않는다면 어떻게 서버와 클라이언트는 통신할까?
    - 데이터가 변경되지 않아도 계속 네트워크를 통해서 데이터를 다운로드 받아야 한다.
    - 인터넷 네트워크는 매우 느리고 비싸다.
    - 브라우저 로딩 속도가 느리다.
    - 느린 사용자 경험

**캐시를 적용한다면?**

```
cache-control: max-age=60
```

위 예제는 캐시의 생명주기를 60(초)로 지정하였다.

즉, 60초 안에 웹브라우저에 똑같은 리소스를 요청한다면, 브라우저는 캐시에서 리소스를 가져온다.



**😱 생명주기로만 캐시를 관리할 때 문제점?**

클라이언트와 서버가 가진 데이터가 같은 경우에도 불구하고, 리소스를 다시 다운받아야한다.

캐시된 리소스의 변경사항을 정확히 파악하여 캐시를 갱신할 수 없을까?



## 검증 헤더 추가 (Last-Modified)

- 웹 브라우저는 GET /star.jpg 로 요청

```
HTTP/1.1 200 OK
Content-Type: image/jpeg
cache-control: max-age=60 
**Last-Modified**: Thu, 04 Jun 2020 07:19:24 GMT
Content-Length: 34012

lkj123kljoiasudlkjaweioluywlnfdo912u34ljko98udjklasl kjdfl;qkawj9;o4ruawsldkal;skdjfa;ow9ejkl3123123
```

- Last-Modified 헤더는 데이터의 최종 수정일을 의미한다.
- 캐시 시간이 초과되고 나면, 웹 브라우저는 서버에 다시 리소스를 요청을 하면서 Last-modified 로 받았던 값을 if-modified-since 헤더를 붙여줌

```
GET /star.jpg
**If-modified-since**: Thu, 04 Jun 2020 07:19:24 GMT
```



- 서버는 날짜를 비교하여 클라이언트가 if-modified-since 헤더로 보낸 날짜 이후로 리소스가 변경되지 않았다면, body가 없는 **304 Not Modified**를 보낸다.

```
HTTP/1.1 **304 Not Modified**
Content-Type: image/jpeg
cache-control: max-age=60 
**Last-Modified**: Thu, 04 Jun 2020 07:19:24 GMT
Content-Length: 34012
```



😱 **날짜로 캐시를 관리하는 것의 문제점은?**

- 1초 미만 단위로 캐시 조정 불가능
- 날짜 기반 로직이 필요함
- 같은 결과를 갖는 데이터지만 날짜만 달라지는 경우
- 서버에서 별도로 캐시 로직을 관리하고 싶을 때



## **검증 헤더 추가 (ETag)**

- Etag(Entity Tag)는 데이터에 임의의 고유한 버전을 달아두는 것임
- 버전은 해시를 주로 사용함, ex) ETag: "v1.0", ETag: "a2gr35dwww"
- 데이터가 변경될 때 ETag를 변경함
- 날짜를 비교하는 로직 없이 단순히 해시와 같은지만 비교할 수 있다.

```
GET /star.jpg
**If-None-Match**: "aaaaaaaaaa"
```

```
HTTP/1.1 304 Not Modified
Content-Type: image/jpeg
Cache-Control: max-age=60 
**ETag**: "aaaaaaaaaa" 
Content-Length: 34012
```



## ETag, If-None-Match 정리

- 클라이언트가 특정 리소스의 ETag 서버에 보내어 같으면 캐시 유지, 다르면 다시 받기
- **캐시 제어 로직을 서버에서 완전히 관리**
- 클라이언트는 단순히 이 값을 서버에 제공할 뿐임
- 실무에서는, 서버에서는 베타 오픈 3일 동안 파일이 변경되어도 ETag를 동일하게 유지
- 또는, 애플리케이션 배포 주기에 맞춰 ETag를 서버에서 일괄적으로 갱신하여 활용할 수 있음

---

- Cache-Control: 캐시를 제어하는 모든 헤더
- Pragma: 캐시를 제어(하위호환)
- Expires: 캐시 유효 기간 제어(하위호환)

캐시 지시어(Directives)

- Cache-Control: max-age
    - 캐시 유효 시간, 초 단위
- Cache-Control: **no-cache**
    - 데이터는 캐시해도 되지만, 항상 원(origin) 서버에 검증하고 사용
- Cache-Control: **no-store**
    - 데이터에 민감한 정보가 있으므로 저장하면 안됨
    - 메모리에서 사용하고 최대한 빨리 삭제

Pragma

- HTTP/1.0 하위호환을 위해 사용
- ex) Pragma: no-cache

Expires

- 캐시 만료일을 지정
- HTTP/1.0 하위호환을 위해 사용
- ex) expires: Mon, 01 Jan 1990 00:00:00 GMT
- 캐시 만료일을 정확한 날짜로 지정
- 초 단위가 훨씬 유연하므로 사용하지 않음
- Cache-Control: max-age가 함께 사용될 경우, Expires는 무시됨

검증 헤더(Validator)

- ETag: "v1.0"
- ETag: "asid912rtt5el"
- Last-Modified: Thu, 04 Jun 2020 08:12:43 GMT

조건부 요청 헤더

- If-Match와 If-None-Match 헤더는 ETag를 값으로 사용
- If-Modified-Since와 If-Unmodified-Since 헤더는 Last-Modified 값을 사용



## 프록시 캐시

- 한국에 있는 클라이언트가 미국에 있는 Origin 서버에 약 500 ms(0.5초)만에 접속한다고 해보자.
- 프록시 캐시 서버는 클라이언트와 오리진 서버 중간에 위치하여 0.5 초보다 빠르게 응답할 수 있다.

- 웹 브라우저 내에 위치한 캐시를 **private 캐시** 라고 한다.
- 프록시 캐시 서버에 위치한 캐시를 **public 캐시**라고 한다.

서버는 캐시 지시어를 이용하여 캐시를 다양하게 제어할 수 있다.

- Cache-Control: public
    - 응답이 public 캐시에 저장됨
- Cache-Control: private
    - 응답이 해당 사용자만을 위한 것이다.
- Cache-Control: s-maxage
    - 프록시 캐시에만 적용되는 max-age
- Age: 60 (HTTP 헤더)
    - 오리진 서버에서 응답 후 프록시 캐시 내에 머문 시간(초 단위)



## 캐시 무효화

캐시 무효화는 웹 브라우저가 임의로 캐싱하는 것을 막기 위해 필요하다.

- Cache-Control: no-cache, no-store, must-revalidate
    - no-cache
        - 데이터는 캐시해도 된다. 하지만 항상 Origin 서버에 검증해야 한다.
    - no-store
        - 민감한 데이터이므로 저장하면 안된다.
    - must-revalidate
        - 캐시가 만료되고 나서, 다시 한번 조회시 Origin 서버에 검증해야 한다.

- Pragma: no-cache
    - HTTP 1.0 하위호환을 위한 헤더다.



### no-cache vs must-revalidate

no-cache와 must-revalidate는 Origin 서버의 정상 응답시에는 동작이 동일하다.

**만약, Origin 서버가 응답하지 않는 경우 동작이 다르다.**

no-cache는 Origin 서버에 접근할 수 없는 경우, 오류를 응답하지 않고 이전 캐시 데이터를 반환할 수도 있다.

must-revalidate는 **Origin 서버에 접근할 수 없을 경우, 항상 오류가 발생시킨다.**

또한, 504 Gateway Timeout 을 상태코드로 반환한다.



---

모든 개발자를 위한 HTTP 웹 기본 지식 https://www.inflearn.com/course/http-웹-네트워크/dashboard