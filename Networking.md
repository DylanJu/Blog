# Networking

### 1. CORS Cross-Origin Resource Sharing

##### 정의

CORS는 다른 origin(웹서버, 도메인)에 접근할 때, 어떻게 브라우저와 서버가 커뮤니케이션 해야 하는가에 대한 W3C의 프로토콜이다.
MS가 처음 XMLHttpRequest를 만들 때만 해도 보안상의 이유로 다른 origin의 접근을 막는 것이 타당했다.
브라우저는 인증 토큰, 쿠키 그리고 개인관련 메타데이터와 같은 다른 애플리케이션에 노출되지 말아야 할 사용자 데이터를 저장하기 때문이다.
그래서 애플리케이션 코드에서 접근하지 못하는 수 많은 보호된 헤더Protected Headers가 있다.
하지만 OPEN API와 Mashup이 대세가 되면서 Cross-Origin에 대한 요구가 늘어났고 W3C가 이를 수용한 것이 CORS다.

##### 사례
www.example.com이라는 사이트에서 외부(ex. 페이스북 등)로 Ajax 요청시 다음과 같은 에러메세지를 볼 수 있다.

<크롬>
  No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin ‘http://www.example.com' is therefore not allowed access.

<파이어폭스>
  교차 원본 요청 차단: 동일 출처 정책으로 인해 ‘http://www.example.com'에 있는 원격 자원을 읽을 수 없습니다. 자원을 같은 도메인으로 이동시키거나 CORS를 활성화하여 해결할 수 있습니다.

www.example.com이 same-origin이 아니라 cross-origin으로 Ajax 요청을 보냈기 때문이다.
same-origin: www.example.com/index.php
cross-origin: www.google.com

하지만 cdn으로 Jquery를 쓰거나 외부에서 이미지 파일을 가져오는 경우, 이 오류는 타탕해보이지 않는다.
CORS는 HTML에서 <img>나 <link>로 다른 origin의 이미지 파일이나 CSS 파일을 불러올 수 있지만
하지만 <script></script> 태그 안에서는 다른 origin에 Request 요청을 보낼 수 없다.

CORS가 허용되는 경우는 다음과 같다.
* CORS 규격에 맞춘 XMLHttpRequest 혹은 Fetch APIs
* 웹 폰트 (css의 @font-face로 사용한 경우)
* WebGL textures
* HTML Canvas의 drawImage 사용해 그려진 이미지나 비디오 프레임
* 스타일시트
* Scripts (for unmuted exceptions)

##### CORS 규격에 맞춘 XMLHttpRequest

CORS XHR은 클라이언트의 Request와 서버의 Response가 규격에 맞게 응해줘야 정상 작동한다.

먼저 클라이언트 Request에서 해야할 일은 새로운 HTTP headers를 추가하는 것이다.
이 HTTP headers는 서버가 웹 브라우저를 사용하여 해당 정보를 읽을 수 있는 출처 집합을 설명 할 수 있는 데이터가 담겨있다.

"Simple" "Preflight" "Credentials" 의 3가지 Request 방식이 존재한다.
Simple Request: 간단하게 보낼 수 있지만 제약이 많은 요청방법이다.
Preflight Request: HTTP OPTIONS 요청으로 사전 승인을 받은 뒤, 본요청을 보내는 방법이다. Simple에 비해 활용폭이 크다.
Credentials Request: 본요청에 쿠키와 HTTP 인증정보를 함께담아 요청하는 방법이다.

(1) Simple Request
* 허용된 Method: GET, HEAD, POST
* 허용된 header: Accept, Accept-Language, Content-Language, Content-Type
* 허용된 Content-Type 헤더: application/x-www-form-urlencoded, multipart/form-data, text/plain

(2) Preflight Request
* Simple Request Method(GET, HEAD, POST) 외의 메소드를 사용하거나
POST를 사용한 요청이 application/x-www-form-urlencoded, multipart/form-data, or text/plain 이외의
다른 값을 가진 Content-Type과 함께 요청 데이터를 전송하는데 사용된 경우에 사용된다.
* 요청 내에 커스텀 헤더를 설정한 경우(예를 들자면 요청이 X-PINGOTHER와 같은 헤더를 사용하는 경우)

(3) Credentials Request
* 사용자 자격증명으로 CORS를 활성화 시키는 방법. 자격증명에는 쿠키, HTTP Authentication 등이 있다.
* xhr.withCredentials = true;값을 전송
* 이 방식은 서버 Response에서 와일드카드 방식을 사용할 수 없다.

##### CORS Response

CORS는 규격에 맞는 Request만으로 작동하지 않는다.
Request에 맞게 서버에서도 규격에 맞는 Response를 해줘야 Cross-Origins 간에 커뮤니케이션이 가능하다.
CORS Response에서 다루는 Header의 종류는 다음과 같다.
* Access-Control-Allow-Origin: 허용된 Cross-Origin 주소. 보통 www.site-A.com 같은 URL이다.
                               (가장 단순한 적용방법은 와일드카드라는 방식으로 특정 URL대신 * 을 써서 모든 URL을 허용하는 것이다.)
* Access-Control-Expose-Headers: 브라우저가 접근할 수 있도록 해주는 서버 화이트리스트 헤더를 적용
* Access-Control-Max-Age: Preflight 방식이 얼마나 오랫동안 캐시될지 정해주는 헤더. ms로 응답한다.
* Access-Control-Allow-Credentials: Credentials 방식의 응답헤더. Boolean 값으로 응답한다.
* Access-Control-Allow-Methods: 허용하는 HTTP Method의 종류
* Access-Control-Allow-Headers: Preflight 방식에서 허용하는 본요청의 헤더 종류


구체적인 예제는 https://developer.mozilla.org/ko/docs/Web/HTTP/Access_control_CORS 에 잘 설명되어 있다.

출처1: https://developer.mozilla.org/ko/docs/Web/HTTP/Access_control_CORS
출처2: http://stackoverflow.com/questions/10636611/how-does-access-control-allow-origin-header-work
출처3: http://hanmomhanda.github.io/2015/07/21/Cross-Origin-Resource-Sharing/
출처4: http://adrenal.tistory.com/16
