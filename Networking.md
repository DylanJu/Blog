# Networking

1. CORS Cross-Origin Resource Sharing

**정의**

CORS는 다른 origin(웹서버, 도메인)에 접근할 때, 어떻게 브라우져와 서버가 커뮤니케이션 해야 하는가에 대한 W3C의 프로토콜이다.
MS가 처음 XMLHttpRequest를 만들 때만 해도 보안상의 이유로 다른 origin의 접근을 막는 것이 타당했다.
하지만 OPEN API와 Mashup이 대세가 되면서 Cross-Origin에 대한 요구가 늘어났고 W3C가 이를 수용한 것이 CORS다.

조금 더 구체적으로 들어가보자.
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

위에서 설명했든 개발방식의 변화에 따라 다양한 CORS 방법이 생겼는데, 대표적인 방법 2가지가 있다.

1. Preflight Request
A.com 에서 B.com 으로 cross-origin request를 보낼 경우에 본요청 이전에 사전요청을 보내 접근허락을 맡는 방법이다.
Preflight Request는 Options method를 이용한다.
