# Chapter1. URL과 리소스

## **3,1 메시지의 흐름**

- HTTP메시지는 HTTP애플리케이션 간에 주고받은 데이터의 블록들이다. 이 데이터의 블록들은 메시지의 내용과 의미를 설명하는 텍스트 메타 정보로 시작하고 그 다음에 선택적으로 데이터가 올 수 있다. 이 메시지는 클라이언트, 서버, 프락시 사이를 흐른다
- '인바운드', '아웃바운드', '업스트림', '다운스트림'은 메시지의 방향을 의미하는 용어다

### **3.1.1 메시지는 원 서버 방향을 인바운드로 하여 송신된다**

- HTTP는 인바운드와 아웃바운드라는 용어를 트랜잭션 방향을 표현하기 위해 사용한다. 메시지가 원 서버로 향하는 것은 인바운드로 이동하는 것이고, 모든 처리가 끑난 뒤에 메시지가 사용자 에이전트로 돌아오는 것은 아웃바웃드로 이동하는 것이다

### **3.1.2 다운스트림으로 흐르는 메시지**

- HTTP 메시지는 강물과 같이 흐른다. 요청 메시지나 응답 메시지냐에 관계없이 모든 메시지는 다운스트림으로 흐른다
- 메시지의 발송자는 수신자의 업스트림이다

## **3.2 메시지의 각 부분**

- HTTP 메시지는 단순한, 데이터의 구조화된 블록이다
- 각 메시지는 클라이언트로부터 요청이나 서버로부터의 응답 중 하나를 포함한다. 메시지는 시작줄, 헤더 블록, 본문 이렇게 세 부분으로 이루너진다. 시작줄은 이것이 어떤 메시지인지 서술하며, 헤더 블록은 속성을, 본문은 데이터를 담고 있다. 본문은 아예 없을 수도 있다
- 시작줄과 헤더는 그냥 줄 단위로 분리된 아스키 문자열이다. 각 줄은 캐리지 리턴과 개행문자로 구성된 두 글자의 문자열로 끝난다. 이 줄바꿈 문자열은 'CRLF'라고 쓴다. HTTP명세에 따른다면 줄바꿈 문자열은 CRLF이지만 견고한 애플리케이션이라면 그냥 개행 문자도 받아들일 수 이썽야 한다는 점을 알아야 한다. 오래되거나 잘못 만들어진 HTTP 애플리케이션들 중에서는 캐리지 리턴과 개행 문자 모두를 항상 전송하지는 않는 것들도 있다
- 엔터티 본문이나 메시지 본문은 단순히 선택적인 데이터 덩어리이다. 시작줄이나 헤더와는 달리, 본문은 텍스트나 이진 데이터를 포함할 수도 있고 그냥 비어있을 수도 있다
- 헤더는 본문에 대한 꽤 많은 정보를 준다. Content-Type 줄은 본문이 무엇인지 말해준다. Content-Length 줄은 본문의 크기를 말해준다

### **3.2.1 메시지 문법**

- 모든 HTTP 메시지는 요청 메시지나 응답 메시지로 분류된다. 요청 메시지는 웹 서버에 어떤 동작을 요구한다. 응답 메시지는 요청의 결과를 클라이언트에게 돌려준다. 요청과 응답 모두 기본적으로 구조가 같다
- 요청 메시지의 형식은 다음과 같다

  ```
  <메서드> <요청 URL> <버전>
  <헤더>

  <엔터티 본문>
  ```

- 응답 메시지의 형식은 다음과 같다

  ```
  <버전> <상태 코드> <사유 구절>
  <헤더>

  <엔터티 본문>
  ```

**메서드**

- 클라이언트 측에서 서버가 리소스에 대해 수행해주길 바라는 동작이다. 'GET', 'HEAD', 'POST'와 같이 한 단어로 되어 있다

**요청 URL**

- 요청 대상이 되는 리소스를 지칭하는 완전한 URL 혹은 URL의 경로 구성요소다. 오나전한 URL이 아닌 URL의 경로 구성요소라고 해도, 클라이언트가 서버와 직접 대화하고 있고 경로 구성요소가 리소스를 가리키는 절대 경로이기만 하면 대체로 문제가 없다
- 서버는 URL에서 생략된 호스트/포트가 자신을 가리키는 것으로 간주할 것이다

**버전**

- HTTP 형식은 다음과 같다
  ```
  HTTP/<메이저>.<마이너>
  ```
- 메이저와 마이너는 모두 정수다

**상태 코드**

- 요청 중에 무엇이 일어났는지 설명하는 세 자리의 숫자다. 각 토드의 첫 번째 자릿수는 상태의 일반적인 분류를 나타낸다

**사유 구절**

- 숫자로 된 상태 코드의 의미를 사람이 이해할 수 있게 설명해주는 짧은 문구로, 상태 코드 이후부터 줄바꿈 문자열까지 사유 구절이다
- 사유 구절은 오로지 사람에게 읽히기 위한 목적으로만 존재하는 것이다. 에를 들어 'HTTP/1.0 200 NOT OK' 와 'HTTP1.0 200 OK'는 사유 구절이 전혀 달라 보임에도 불구하고 동등하게 성공을 의미하는 것으로 처리되어야 한다

**헤더들**

- 이름, 콜론, 선택적인 공백 값, CRLF가 순서대로 나타나는 0개 이상의 헤더들, 이 헤더의 목록은 빈 줄(CRLF)로 끝나 헤더 목록의 끝과 엔터티 본문의 시작을 표시한다
- HTTP/1.1과 같은 몇몇 버전의 HTTP는 요청이나 응답에 어떤 특정 헤더가 포함되어야만 유효한 것으로 간주한다

**엔터티 본문**

- 엔터티 본문은 임의이ㅡ 데이터 블록을 포함한다. 모든 메시지가 엔터티 본문을 갖는것은 아니므로, 때때로 메시지는 그냥 CRLF으로 끝나게 된다
- 헤더나 엔터티 본문이 없더라도 HTTP 헤더의 집합은 항상 빈 줄(CRLF)로 끝나야 함에 주의하라
- 이와 같이 널리 쓰이지만 규칙을 잘 지키지 않는 구현체와의 호환을 위해, 클라이언트와 서버는 마지막 CRLF없이 끝나는 메시지도 받아들일 수 있어야 한다

### **3.2.2 시작줄**

- 모든 HTTP 메시지는 시작줄로 시작한다. 요청 메시지의 시작줄은 무엇을 해야 하는지 말해준다. 응답 메시지의 시작줄은 무슨 일이 일어났는지 말해준다

**요청줄**

- 오청 메시지는 서버에게 리소스에 대해 무언가를 해달라고 부탁한다. 요청 메시지의 시작줄, 혹은 요청줄에는 서버에서 어떤 동작이 일어나야 하는지 설명해주는 메서드와 그 동작에 대한 대상을 지칭하는 요청 URL이 들어있다
- 또한 요청줄은 클라이언트가 어떤 HTTP 버전으로 말하고 있는지 서버에게 알려주는 HTTP 버전도 포함한다
- 이 모든 필드는 공백으로 구분된다
- HTTP/1.0 이전에는 요청줄에 HTTP 버전이 들어있을 필요가 없었다

**응답줄**

- 응답 메시지는 수행 결과에 대한 상태 정보와 결과 데이터를 클라이언트에게 돌려준다. 응답 메시지의 시작줄 혹은 응답줄에는 응답 메시지에서 쓰인 HTTP의 버전, 숫자로 된 상태 코드, 수행 상태에 대해 설명해주는 텍스트로 된 사유 구절이 들어있다
- 이 모든 필드는 공백으로 구분도니다
- HTTP/1.0 이전 시절에는 응답에 응답줄이 들어있을 필요가 없었다

**메서드**

- 요청의 시작줄은 메서드로 시작하며, 서버에게 무엇을 해야 하는지 말해준다. 예를 들어 'GET /special/saw-blade.gif HTTP/1.0' 이라는 줄에서 메서드는 GET이다
- HTTP명세는 공통 요청 메서드의 집합을 정의한다
- HTTP는 쉽게 확장할 수 있도록 설계되었기 때문에, 다른 서버는 그들만의 메서드를 추가로 구현했을 수도있다. 이러한 추가 메서드는 HTTP 명세를 확장하는 것이기 때문에 확장 메서드라고 불린다

**상태 코드**

- 메서드가 서버에게 무엇을 해야 하는지 말해주는 것처럼, 상태 코드는 클라이언트에게 무엇이 일어났는지 말해준다. 상태코드는 응답의 시작줄에 위치한다
- 서버는 요청한 리소스가 발견되지 않았거나, 그 리소스에 접근할 권한이 없거나, 어쩌면 그 리소스가 다른 곳으로 옮겨졌다고 알려올 수도 있다
- 상태 코드는 각 응답 메시지의 시작줄에 담겨 반환된다. 숫자로 된 코드와, 문자열로 되어있어서 사람이 이해하기 쉬운 메시지 두 형태 모두로 반환된다. 사유 구절이 사람에게 쉽게 읽히는 한편, 숫자로된 코드는 프로그램이 에러를 처리하기 쉽다
- 상태 코드들은 세 자리 숫자로 된 그들의 코드값을 기준으로 묶인다. 200에서 299까지의 상태코드는 성공을 나타낸다. 300에서 399까지의 코드는 리소스가 옮겨졌음을 뜻한다. 400에서 499까지의 코드는 클라이언트가 뭔가 잘못된 요청을 했음을 의미한다. 500에서 599까지의 코드는 서버에서 뭔가 실패했음을 의미한다

**사유 구절**

- 사유 구절은 응답 시작줄의 마지막 구성요소다. 이것은 상태 코드에 대한 글로 된 설명을 제공한다
- 사유 구절은 상태 코드와 일대일로 대응된다. 사유 구절은, 애플리케이션 개발자들이 그들의 사용자에게 요청 중에 무슨 일이 일어났는지 알려주기 위해 넘겨줄 수 있는, 상태 코드의 사람이 이해하기 쉬운 버전이다
- HTTP 명세는 사유 구절이 어때야 한다는 어떤 엄격한 규칙도 제공하지 않는다

**버전 번호**

- 버전 번호는 HTTP/x,y 형식으로 요청과 응답 메시지 양쪽 모두에 기술된다. 이것은 HTTP 애플리케이션들이 자신이 따르는 프로토콜의 버전을 상대방에게 말해주기 위한 수단이 된다
- 버전 번호는 HTTP로 대화하는 애플리케이션들에게 대화 상대의 능력과 메시지의 형식에 대한 단서를 제공해주기 위한 것이다. HTTP 버전 1.1 애플리케이션과 대화하는 HTTP 버전 1.2 애플리케이션은 1.2 버전의 새로운 기능을 사용할 수 없다는 것을 알아야 한다
- 버전 1.1 애플리케이션은 아마도 1.2 버전의 기능을 구현하지 않았을 것이기 때문이다
- 버전 번호는 어떤 애플리케이션이 지원하는 가장 높은 HTTP 버전을 가리킨다
- 응답의 프로토콜이 HTTP/1.1 이라는 것은 사실 응답을 보낸 애플리케잇녀이 HTTP/1.1 까지 이해할 수 있음을 의미하는 것이다
- 버전 번호는 분수로 다루어지지 않음에 주의하라. 버전의 각 숫자는 각각 분리된 ㅜㅆ자로 다루어진다. 따라서 어느 쪽이 큰지 HTTP 버전을 비교할 떄 각 숫자는 반드시 따로따로 비교해야 한다

### **3.2.3 헤더**

- 시작줄 다음에는 0개, 1개 혹은 여러 개의 HTTP 헤더가 온다
- HTTP 헤더 릴드는 요청과 응답 메시지에 추가 정보를 더한다. 그들은 기본적으로 이름/값 쌍의 목록이다

**헤더 분류**

- HTTP 헤더 명세는 여러 헤더 필드를 정의한다. 애플리케이션은 또한 자유롭게 자신만의 헤더를 만들어낼 수 있다
- HTTP헤더는 다음과 같이 분류된다
  - 일반헤더
    - 요청과 응답 양쪽에 모두 나타날 수 있음
  - 요청 헤더
    - 요청에 대한 부가 정보를 제공
  - 응답 헤더
    - 응답에 대한 부가 정보를 제공
  - Entity 헤더
    - 본문 크기와 콘텐츠, 혹은 리소스 그 자체를 서술
  - 확장 헤더
    - 명세에 정의되지 않은 새로운 헤더
- 각 HTTP헤더는 간단한 문법을 가진다. 이름, 쉼표, 공백(없어도 된다), 필드 값, CRLF가 순서대로 온다

**헤더를 여러줄로 나누기**

- 긴 헤더 줄은 그들을 여러 줄로 쪼개서 더 릭기 좋게 만들 수 있는데, 추가 줄 앞에는 최소 하나의 스페이스 혹은 탭 문자가 와야 한다
  ```
  HTTP/1.0 200 OK
  Content-Type: image/gif
  Content-Length: 8572
  Server: Test Server
    Version 1.0
  ```
- 이 예에서, 응답 메시지는 여러줄로 값이 쪼개진 Server헤더를 포함하고 있다. 그 헤더의 완전한 값은 'Test Server Version 1.0'이다

### **3.2.4 엔터티 본문**

- HTTP 메시지의 세 번째 부분은 선택적인 엔터티 본문이다. 엔터티 본문은 HTTP 메시지의 화물이라고 할 수 있다. 그것들은 HTTP가 수송하도록 설계된 것이다
- HTTP 메시지는 이미지, 비디오, HTML 문서, 소프트웨어 애플리케이션, 신용카드 트랜잭션, 전자우편 등 여러 종류의 디지털 데이터를 실어 나를 수 있다

### **3.2.5 버전 0.9 메시지**

- HTTP 버전 0.9는 HTTP 프로토콜의 초기 버전이다. 그것은 오늘날 HTTP가 갖고있는 요청과 응답 메시지의 시초이지만, 훨씬 단순한 프로토콜로 되어있다
- HTTP/0.9 메시지도 마찬가지로 요청과 응답으로 이루어져있지만, 요청은 그저 메서드와 요청 URL을 갖고 있을 뿐이며, 응답은 오직 엔터티로만 되어있다. 버전 정보도 없고, 상태 코드나 사유 구절도 없으며, 헤더도 포함되어있지 않다
- 이와 같은 지나칠 정도의 단순함 때문에, HTTP/0.9 로는 다양한 상황에 대응할 수 없다
- HTTP/0.9에 대해 설명한 것은, 여전이 그것을 사용하는 클라이언트, 서버, 기타 애플리케이션들이 있개 때문에, 애플리케이션 개발자들이 HTTP/0.9의 제약에 대해 알아둘 수 있게 하기 위함이다

## **3.3 메서드**

- 비록 서버가 모든 메서드를 구현하지 않았다 하더라도 메서드는 대부분 제한적으로 사용될 것이다
- 이 제한은 일반적으로 서버 설정에 의해 정해지며, 따라서 사이트마다 또 서버마다 다를 수 있다

### **3.3.1 안전한 메서드**

- HTTP는 안전한 메서드라 불리는 메서드의 집합을 정의한다. GET과 HEAD 메서드는 안전하다고 할 수 있는데, 이는 GET이나 HEAD 메서드를 사용하는 HTTP 요청의 결과로 서버에 어떤 작용도 없음을 의미한다
- 작용이 없다는 것은, HTTP 요청의 결과로 인해 서버에서 일어나는 일은 아무것도 없다는 의미이다
- 안전한 메서드가 서버에 작용을 유발하지 않는다는 보장은 없다. 안전한 메서드의 목적은, 서버에 어떤 영향을 줄 수 있는 안저하지 않은 메서드가 사용될 때 사용자들에게 그 사실을 알려주는 HTTP 애플리케이션을 만들 수 있도록 하는 것에 있다

### **3.3.2 GET**

- GET은 가장 흔히 쓰이는 메서드가. 주로 서버에게 리소스를 달라고 요청하기 위해 쓰인다. HTTP/1.1 은 서버가 이 메서드를 구현할 것을 요구한다

### **3.3.3 HEAD**

- HEAD 메서드는 정확히 GET 처럼 행동하지만, 서버는 응답으로 헤더만을 돌려준다. 엔터티 본문은 결코 반환되지 않는다. 이는 클라이언트가 리소스를 실제로 가져올 필요 없이 헤더만을 조사할 수 있도록 해준다
  1. 리소스를 가져오지 않고도 그에 대해 무엇인가를 알아낼 수 있다
  2. 응답의 상태 코드를 통해, 개체가 존재하는지 알 수 있다
  3. 헤더를 확인하여 리소스가 변경되었는지 검사할 수 있다
- 서버 개발자들은 반드시 반환되는 헤더가 GET으로 얻는 것과 정확히 일치함을 보장해야 한다. 또한 HTTP/1.1 준수를 위해서는 HEAD 메서드가 반드시 구현되어 있어야 한다

### **3.3.4 PUT**

- GET 메서드가 서버로부터 문서를 읽어 들이는데 반해 PUT 메서드는 서버에 문서를 쓴다
- PUT 메서드의 의미는, 서버가 요청의 본문을 가지고 요청 URL의 이름대로 새 문서를 만들거나, 이미 URL이 존재한다면 본문을 사용해서 교체하는 것이다
- PUT은 콘텐츠를 변겨어할 수 있게 해주기 떄문에, 많은 웹서버가 PUT을 수행하기전에 사용자에게 비밀번호를 입력해서 로그인을 하도록 요구할 것이다

### **POST**

- POST 메서드는 서버에 입력 데이터를 전송하기 위해 설계되었다. 실제로 HTML 폼을 지원하기 위해 흔히 사용된다
- 채워진 폼에 담긴 데이터는 서버로 전송되며, 서버는 이를 모아서 필요로 하는곳에 보낸다

### **3.3.6 TRACE**

- 클라이언트가 어떤 요청을 할 떄, 그 요청은 방화벽, 프락시, 게이트웨이 등의 애플리케이션을 통과할 수 있다. 이들에게는 원래의 HTTP 요청을 수정할 수 있는 기회가 있다. TRACE 메서드는 클라이언트에게 자신의 요청이 서버에 도달했을때 어떻게 보이게 되는지 알려준다
- TRACE 요청은 목적지 서버에서 '루프백'진단을 시작한다. 요청 전송의 마지막 단계에 있는 서버는 자신이 받은 요청 메시지를 본문에 넣어 TRACE 응답을 되돌려준다. 클라이언트는 자신과 목적지 서버 사이에 있는 모든 HTTP 애플리케이션의 요청/응답 연쇄를 따라가면서 자신이 보낸 메시지가 망가졌거나 수정되었는지, 만약 그렇다면 어떻게 변경되었는지 확인할 수 있다
- TRACE 메서드는 주로 진단을 위해 사용된다. 예를 들면 요청이 의도한 요청/응답 연쇄를 거쳐가는지 검사할 수 있다
- TRACE는 진단을 위해 사용할 때는 괜찮지만, 그 대신 중간 애플리케이션이 여러 다른 종류의 요청들을 일관되게 다룬다고 가정하는 문제가 있다. 많은 HTTP 애플리케이션은 메서드에 따라 다르게 동작한다
- TRACE는 메서드를 구별하는 메커니즘을 제공하지 않는다. 어떻게 TRACE 요청을 처리할 것인지에 대해서는 일반적으로 중간 애플리케이션이 결정을 내린다
- TRACE 요청은 어떠한 엔터티 본문도 보낼 수 없다. TRACE 응답의 엔터티 본문에는 서버가 받은 요청이 그대로 들어있다

### **3.3.7 OPTIONS**

- OPTIONS 메서드는 웹 서버에게 여러가지 종류의 지원 범위에 대해 물어본다. 서버에게 특정 리소스에 대해 어떤 메서드가 지원되는지 물어볼 수 있다
- 이 메서드는 여러 리로스에 대해 실제로 접근하지 않고도 그것들을 어떻게 접근하는 것이 최선인지 확인할 수 있는 수단을 클라이언트 애플리케이션에게 제공한다

### **3.3.8 DELETE**

- DELETE 메서드는 서버에게 요청 URL로 지정한 리소스를 삭제할 것을 요청한다. 그러나 클라이언트는 삭제가 수행되는 것을 보장하지 못한다. 왜냐하면 HTTP 명세는 서버가 클라이언트에게 알리지 않고 요청을 무시하는 것을 허용하기 떄문이다

### ** 3.3.9 확장 메서드**

- HTTP는 필요에 따라 확장해도 문제가 없도록 설계되어 있으므로, 새로 기능을 추가해도 과거에 구현된 소프트웨어들의 오동작을 유발하지 않는다. 확장 메서즈는 HTTP/1.1 명세에 정의되지 않은 메서드다
- 모든 확장 메서드가 형식을 갖춘 명세로 정의된것은 아니라는 점에 주의해야 한다. 만약 당신이 어떤 확장 메서드를 정의한다면, 그것은 대부분의 HTTP 애플리케이션이 이해할 수 없을 것이다
- 이런 상황에서는 확장 메서드에 대해 관용적인 것이 최고다. 프락시는 종단간 행위를 망가뜨리지 않을 수 있다면, 알려지지 않은 메서드가 담긴 메시지를 다운스트림 서버로 전달하려고 시도한다. 그헣지 않다면 프락시는 501 Not Implemented 상태코드로 응답해야 한다

## **3.4 상태코드**

- 상태 코드는 클라이언트에게 그들의 트랙잭션을 이해할 수 있는 쉬운 방법을 제공한다

### **3.4.1 100-199 : 정보성 상태 코드**

- 정보성 상태 코드는 HTTP/1.1 에서 도입되었다
- 특히 100 Continue 상태코드는 약간 혼란스럽다. 100 continue는 HTTP 클라이언트 애플리케이션이 서버에 엔터티 본문을 전송하기 전에 그 엔터티 본문을 서버가 받아들일 것인지 확인하려고 할 대, 그 확인 작업을 최적화 하기 위한 의도로 도입된 것이다. 이는 HTTP 프로그래머를 혼란스럽게 하는 경향이 있다

**클라이언트와 100 Continue**

- 만약 클라이언트가 엔터티를 서버에게 보내려고 하고, 그 전에 100 Continue 응답을 기다리겠다면, 클라이언트는 값을 100-continue로 하는 Expext 요청 헤더를 보낼 필요가 있다. 만약 클라이언트가 엔터티를 보내지 않으려 한다면, 100-continue Expext 해더를 보내지 않아야 한다. 왜냐하면 이것은 클라이언트가 엔터티를 보낼 것이라고 생각하게 만들어 서버를 혼란에 빠뜨릴 뿐이기 때문이다
- 100=continue는 여러 측면에서 최적화를 위한 것이다. 클라이언트 애플리케이션은 100-continue를 서버가 다루거나 사용할 수 없는 큰 엔터티를 서버에게 보내지 않으려는 목적으로만 사용해야 한다
- 100-continue 값이 담긴 Expext 헤더를 보낸 클라이언트는 서버가 100 Continue 응답을 보내주기만을 막연히 기다리기만 해서는 안된다. 약간의 타입아웃후에 클라이언트는 그냥 엔터티를 보내햐 한다
- 클라이언트 개발자는 예상하지 못한 100 Continue 응답에도 대비해야 한다

**서버와 100 Continue**

- 서버가 100-continue 값이 담긴 Expext 헤더가 포함된 요청을 받는다면, 100 Continue 응답 혹은 에러 코드로 답해야 한다. 서버는 절대로 100-continue 응답을 받을 것을 의도하지 않은 클라이언트에게 100-continue 상태 코드를 보내서는 안된다
- 서버가 100 Continue 응답을 보낼 기회를 갖기 전에 어떤 이유로 인해 엔터티의 일부를 수신하였다면, 서버는 이 상태 코드를 보낼 필요가 없다. 왜냐하면 클라이언트는 이미 계속 전송하기로 결정하였기 때문이다. 그러나 서버가 요청을 끝가지 다 읽은 후에는 그 요청에 대한 최종 응답을 보내야 한다
- 만약 서버가 100 continue 응답을 받을 것을 의도한 요청을 받고 난 상태에서 엔터티 본문을 읽기 전에 요청을 끝내기로 결정했다면, 서버는 그냥 응답을 보내고 연결을 닫아서는 안된다. 클라이언트가 응답을 받을 수 없게 되기 때문이다

**프락시와 100 Continue**

- 클라이언트로부터 100-continue 응답을 의도한 요청을 받은 프락시는 몇 가지 해야 할 일이 있다. 만약 다음 홉(next-hop) 서버가 HTTP/1.1을 따르거나 혹은 어떤 버전을 따르는지 모른다면, Expext 헤더를 포함시켜서 요청을 다음으로 전달해야 한다. 만약 다음 홉의 서버가 1.1 보다 이전 버전의 HTTP를 다른다는 것을 알고 있다면, 프락시는 417 Expectation Failed 에러로 응답해야한다
- 만약 프락시가 HTTP/1.0이나 이전 버전을 따르는 클라이언트를 대신하여 Expext 헤더와 100-continue 값을 요청에 포함시키기로 결정했다면, 프락시는 100 Continue 응답응ㄹ 클라이언트에게 전달해서는 안된다. 왜냐하면 클라이언트는 그것을 어떻게 해야 할지 모를 것이기 때문이다

### **3.4.2 200-299 : 성공 상태 코드**

- 클라이언트가 요청을 보내면, 그 요청을 대개 성공한다. 서버는 대응하는 성공을 의미하는 상태 코드의 배열을 갖고 있으며, 각각 다른 종류의 요청에 대응한다
  - **상태코드 200 , 사유구절 : OK**
    - 요청을 정상이고, 엔터티 본문은 요청된 리소스를 포함하고 있다
  - **상태코드 201 , 사유구절 : Accepted**
    - 서버 개체를 생성하려는 요청을 위한 것, 응답은 생성된 리소스에 대한 최대한 구체적인 참조가 담긴 Location 헤더와 함께, 그 리소스를 참조할 수 있는 여러 URL을 엔터티 본문에 포함해야 한다. 서버는 상태코드를 보내기에 앞서 반드시 객체를 생성해야 한다
  - **상태코드 202 , 사유구절: Accepted**
    - 요청은 받아들려졌으나 서버는 아직 그에 대한 어떤 동작도 수행하지 않았다. 서버가 요청의 처리를 완료할 것인지에 대한 어떤 보장도 ㅇ벗다. 이것은 단지 요청이 받아들이기에 적법해 보인다는 의미일 뿐이다. 서버는 엔터티 본문에 요청에 대한 상태와 가급적으면 요청의 처리가 언제 완료될 것인지에 대한 추정도 포함해야 한다
  - **상태코드 203 , 사유구절: Non-Authoritatice information**
    - 엔터티 헤더에 들어있는 정보가 원래 서버가 아닌 리소스의 사본에서 왔다. 중개자가 리소스의 사본을 갖고 있엇지만 리소스에 대한 메타 정보를 검증하지 못한 경우 이런 일이 발생할 수 있다. 이 응답 코드는 필수적으로 사용되어야 하는 것은 아니다. 이것은 엔터티 헤더가 원래 서버에서 온 것이었다면 응답이 200 상태였을 애플리케이션을 위한 선택사항이다
  - **상태코드 204 , 사유구절: No Content**
    - 응답 메시지는 헤더와 상태줄을 포함하지만 엔터티 본문은 포함하지 않는다. 주로 웹브라우저를 새 문서로 이동시키지 안하고 갱신하고자 할때 사용한다
  - **상태코드 205 , 사유구절: Reset Content**
    - 주로 브라우저는 위해 사용되는 또 하나의 코드. 브라우저에게 현재 페이지에 있는 HTML 폼에 채원진 모든 값을 비우라고 말한다
  - **상태코드 206 , 사유구절: Partial Content**
    - 부분 혹은 범위 요청이 성공했다. 나중에 우리는 클라이언트가 특별한 헤더를 사용해서 문서의 부분 혹은 특정 범위를 요청할 수 있다는 것을 보게 될 것이다. 이 상태 코드는 범위 요청이 성공했음을 의미한다. 206 응답은 Content-Range 와 Date 헤더를 반드시 포함해야 하며, Etag와 Content-Location 중 하나의 헤더도 반드시 포함해야 한다

### **3.4.3 300-399 : 리다이렉션 상태 코드**

- 리다이렉션 상태 코드는 클라이언트가 관심있어 하는 리소스에 대해 다른 위치를 사용하라고 말해주거나 그 리소스의 내용 대신 다른 대안 응답을 제공한다. 만약 리소스가 옮겨졌다면, 클라이언트에게 리소스가 옮겨졌으며 어디서 찾을 수 있는지 알려주기 위해 리다이렉션 상태 코드와 Location 헤더를 보낼 수 있다. 이는 브라우저가 사용자를 귀찮게 하지 않고 알아서 새 위치로 이동할 수 있게 해준다
- 리다이렉션 상태 코드 중 멏몇은 리소스에 대한 애플리케이션의 로컬 복사본이 원래 서버와 비교했을 떄 유효한지 확인하기 위해 사용된다
- 일반적으로, HEAD가 아닌 요청에 대해 리다이렉션 상태 코드를 포함한 응답을 할 때, 리다이렉트될 URL에 대한 링크와 설명을 포함시키는 것은 좋은 습관이다

  - **상태코드 300 , 사유구절: Multiple Choices**
    - 클라이언트가 동시에 여러 리소스를 가리키는 URL을 요청한 경우, 그 리소스의 목록과 함게 반환한다. 사용자는 목록에서 원하는 하나를 선택할 수 있다. 어떤 서버가 하나의 HTML 문서를 영어와 프랑스어 모두로 제공하는 경우 등에 사용할 수 있을 것이다. 서버는 Location 헤더에 선호하는 URL을 포함시킬 수 있다
  - **상태코드: 301, 사유구절: Moved Permanently**
    - 요청한 URL이 옮겨졌을 때 사용한다. 응답은 Location 헤더에 현재 리소스가 존재하고 있는 URL을 포함해야 한다
  - **상태코드: 302 , 사유구절: Found**
    - 301 상태코드와 같다. 그러나 클라이언트는 Location 헤더로 주어진 URL을 시소스를 임시코 가리키기 위한 목적으로 사용해야 한다. 이후의 요청에서는 원래 URL을 사용해야한다
  - **상태코드 303 , 사유구절: See Other**
    - 클라이언트에게 리소스를 다른 URL에서 가져와야 한다고 말해주고자 할떄 쓰인다. 새 URL은 응답 메시지의 Location 헤더에 들어있다. 이 상태 코드의 주 목적은 POST 요청에 대한 응답으로 클라이언트에게 리소스의 위치를 알려주는 것이다
  - **상태코드 304 , 사유구절:Nost Modified**
    - 클라이언트는 헤더를 이용해 조건부 요청을 만들 수 있다. 만약 클라이언트가 GET과 같은 조건부 요청을 보냈고 그 요청한 리소스가 최근에 구정된 일이 없다면, 이 코드는 리소스가 수정되지 않았음을 의미한다. 이 상태 코드를 동반한 응답은 엔터티 본문을 가져서는 안된다
  - **상태코드 305 , 사유구절: Use Proxy**
    - 리소스가 반드시 프락시를 통해서 접근되어야 함을 나타내기 위해 사용한다. 프락시의 위치는 Location 헤더를 통해 주어진다. 클라이언트는 이 응답을 특정 리소스에 대한 것이라고만 해석한다. 클라이언트는 모든 요청에 대해 이 프락시를 통해야 한다고 상정하지 않으며, 그 리소스를 갖고 있는 서버에 대한 요청이라 할지라도 마찬가지다. 이점은 중요하다. 프락시가 요청에 잘못 간섭한다면 이는 오동작을 유발할 수 있고, 보안 문제를 일으킬 수 있다
  - **상태코드: 306, 사유구절: (사용되지 않음)**
    - 현재는 사용되지 않는다
  - **상태코드 307, 사유구절:Temporary Redirect**
    - 301 상태 코드와 비슷하다. 그러나 클라이언트는 Location 헤더로 주어진 URL을 리소스를 임시로 가리키기 위한 목적으로 사용해야 한다. 이후의 요청에서는 원래 URL을 사용해야한다

- 302,303,307 상태 코드는 중복되는 부분이 있는데, 이 상태 코드들이 어떻게 사용되는가에 대해서는 약간 미묘한 차이가 있다. 이는 주로 HTTP/1.0과 HTTP/1.1 애플리케이션이 이 상태 코드를 다루는 방식의 차이점에 기인한다
- HTTP/1.0 클라이언트가 POST요청을 보내고 302 리다이렉트 상태 코드가 담긴 응답을 받으면, 클라이언트는 Location 헤더에 들어있는 리다이렉트 URL을 GET요청으로 따라갈 것이다
- HTTP/1.0 서버가 HTTP/1.0 클라이언트로부터 POST 요청을 받은 뒤 302 상태 코드를 보내는 상황이라면, 서버는 클라이언트가 리다이렉션 URL에 대한 GET 요청으로 리다이렉트를 따라가길 기대한다
- HTTP/1.1 명세는 그러한 리다이렉션을 위해 303 상태코드를 사용한다. 이 혼란을 막기위해, HTTP/1.1 명세는 HTTP/1.1 클라이언트의 일시적인 리다이렉트를 위해 302 상태 대신 307 상태 코드를 사용하라고 한다. 그리고 서버는 302 상태코드는 HTTP/1.0 클라이언트에게 사용하기 위해 남겨둘 수 있을 것이다
- 결국 서버는 리다이렉트 응답에 들어갈 가장 적절한 리다이렉트 상태 코드를 선택하기 위해 클라이언트의 HTTP 버전을 검사할 필요가 있다

### **3.4.4 400-499 : 클라이언트 에러 상태 코드**

- 가끔 클라이언트는 서버가 다룰 수 없는 무엇인가를 보낸다. 잘못 구성된 요청 메시지 같은 것이 있을 수 있으며, 가장 흔한 것은 존재하지 않는 URL에 대한 요청이다
- 많은 클라이언트 에러가 당신을 귀찮게 하지 않고 브라우저에 의해 처리된다. 404를 비롯한 몇몇은 알아서 처리도지 않고 당신에게 전달될 것이다
  - **상태코드 400 , 사유구절: Bad Request**
    - 클라이언트가 잘못된 요청을 보냈다고 말해준다
  - **상태코드 401 , 사유구절: Unathorized**
    - 리소스를 얻기 전에 클라이언트에게 스스로를 인증하라고 요구하는 내용의 응답을 적절한 헤더와 함께 반환한다
  - **상태코드 402 , 사유구절: Payment Required**
    - 현재 이 상태 코드는 스이지 않지만, 미래에 사용될 가능성을 위해 준비해 두었다
  - **상태코드 403 , 사유구절: Forbidden**
    - 요청이 서버에 의해 거부되었음을 알려주기 위해 사용한다. 만약 서버가 왜 요청이 거부되었는지 알려주고자 한다면, 서버는 그 이유를 설명하는 엔터티 본문을 포함시킬 수 있다. 그러나 이 코드는 보통 서버가 거절의 이유를 숨기고 싶을 떄 사용한다
  - **상태코드 404 , 사유구절: Not Found**
    - 서버가 요청한 URL을 찾을 수 없음을 알려주기 위해 사용하낟. 종종 클라이언트 애플리케이션이 사용자에게 보여주기 위한 엔터티가 포함된다
  - **상태코드 405 , 사유구절: Method Not Allowed**
    - 요청한 URL에 대해, 지원하지 않는 메서드로 요청 받았을 때 사용한다. 요청한 리소스에 대해 어떤 메서드가 사용 가능한지 클라이언트에게 알려주기 위해, 요청에 Allow 헤더가 포함되어야 한다. Allow 헤더에 대해 자세한 것은 '엔터티 헤더'를 보라
  - **상태코드 406 , 사유구절: Not Acceptable**
    - 클라이언트는 자신이 어떤 종류의 엔터티를 받아들이고자 하는지에 대해 매개변수로 명시할 수 있다. 이 코드는 주어진 URL에 대한 리소스 중 클라이언트가 받아들일 수 있는 것이 없는 경우 사용한다. 종종 서버는 클라이언트에게 왜 요청이 만족될 수 없었는지 알려주는 헤더를 포함시킨다
  - **상태코드 407 , 사유구절: Poxy Authentication Required**
    - 401 상태코드와 같으나, 리소스에 대해 인증을 요구하는 프락시 서버를 위해 사용한다
  - **상태코드 408 , 사유구절: Request Timeout**
    - 클라이언트의 요청을 오나수하기에 시간이 너무 많이 걸리는 경우, 서버는 이 상태 코드로 응답하고 연결을 끊을 수 있다.이 타임아웃의 길이는 서버마다 다르지만 대개 어떠한 적법한 요청도 받아들일 수 있을 정도로 충분히 길다
  - **상태코드 409 , 사유구절: Confilict**
    - 요청이 리소스에 대해 일으킬수 있는 몇몇 충돌을 지칭하기 위해 사용한다. 서버는 요청이 충돌을 일으킬 염려가 있다고 생각될 떄 이 요청을 보낼 수 있다. 응답은 충돌에 대해 설명하는 본문을 포함해야 한다
  - **상태코드 410 , 사유구절: Gone**
    - 404와 비슷하나, 서버가 한때 리소스를 갖고 있었다는 점이 다르다. 주로 웹 사이트를 유지보수하면서, 서버 관리자가 클라이언트에게 리소스가 제거된 경우 이를 알려주기 위해 사용한다
  - **상태코드 411 , 사유구절: Length Required**
    - 서버가 요청 메시지에 Content-Length 헤더가 있을 것을 요구할 때 사용한다
  - **상태코드 412 , 사유구절: Precondition Failed**
    - 클라이언트가 조건부 요청을 했는데 그중 하나가 실패했을 때 사용한다. 조건부 요청은 클라이너트가 Expect 헤더를 포함했을 때 방생한다
  - **상태코드 413 , 사유구절: Request Entity Too Large**
    - 서버가 처리할 수 있는 혹은 처리하고자 하는 한계를 넘은 길이의 요청 URL이 포함된 요청을 클라이언트가 보냈을 때 사용한다
  - **상태코드 414 , 사유구절: Request URL Too Long**
    - 서버가 처리할 수 있는 혹은 처리하고자 하는 한계를 넘은 길이의 요청 URL이 포함된 요청을 클라이언트가 보냈을 때 사용한다
  - **상태코드 415 , 사유구절: Unsupported Media Type**
    - 서버가 이해하거나 지원하지 못한ㄴ 내용 유형의 엔터티를 클라이언트가 보냈을 때 사용한다
  - **상태코드 416 , 사유구절: Requested Range Not Satisfiable**
    - 요청 메시지가 리소스의 특정 범위를 요청했는데, 그 범위가 잘못 되었거나 맞지 않을 때 사용한다
  - **상태코드 417 , 사유구절: Expectation Failed**
    - 요청에 포함된 Expect 요청 헤더에 서버가 만족시킬 수 없는 기대가 담겨있는 경우 사용한다. 프락시나 다른 중개자 애플리케이션은, 웜 서버가 요청의 기대를 만족시킬 수 없을 명확한 증거가 있다면 이 응답코드를 전송할 수 있다

### **3.4.5 500-599 : 서버 에러 상태 코드**

- 때때로, 클라이언트가 올바른 요청을 보냈음에도 서버 자체에서 에러가 발생하는 경우가 있다. 이것은 클라이언트가 서버의 제한에 걸린 것일 수도 있고 혹은 게이트웨이 리소스와 같은 서버의 보조 구성요소에서 발생한 에러일 수도 있다
- 프락시는 클라이언트의 입장에서 서버와 대화를 시도할 때 자주 에러를 만나게 된다. 프락시는 문제를 설명하기 위해 5xx 서버 에러 상태 코드를 생성한다
  - **상태코드 500 , 사유구절: Internal Sever Error**
    - 서버가 요청을 처리할 수 없게 만드는 에러를 만났을 때 사용한다
  - **상태코드 501 , 사유구절: Not Implemented**
    - 클라이언트가 서버의 능력을 넘은 요청을 했을 때 사용한다
  - **상태코드 502 , 사유구절: Bad Gatway**
    - 프락시나 게이트웨이처럼 행동하는 서버가 그 요청 응답 연쇄에 있는 다음 링크로부터 가짜 응답에 맞닥뜨렸을 때 사용한다
  - **상태코드 503 , 사유구절: Service Unavailable**
    - 현재는 서버가 요청을 처리해줄 수 없지만 나중에는 가능함을 의미하고자 할 때 사용한다. 만약 서버가 언제 그 리소스를 사용할 수 있게 될지 알고 있다면, 서버는 Retry-After 헤더를 응답에 포함시킬 수 있다
  - **상태코드 504 , 사유구절: Gatway Timeout**
    - 상태 코드 408과 비슷하지만, 다른 서버에게 요청을 보내고 응답을 기다리다 타입아웃이 발생한 게이트웨이나 프락시에서 온 응답이라는 점이 다르다
  - **상태코드 505 , 사유구절: HTTP Version Not Supported**
    - 서버가 지원할 수 없거나 지원하지 않으려고 하는 버전의 프로토콜로 된 요청을 받았을 때 사용한다. 몇몇 서버 애플리케이션들은 오래된 버전의 프로토콜은 지원하지 않는 것을 택한다

## **4.5 헤더**

- 헤더와 메서드는 클라이언트와 서버가 무엇을 하는지 걸정하기 위해 함께 사용된다
- 헤더에는 특정 종류의 메시지에만 사용하룻 있는 헤더와, 더 일반 목적으로 사용할 수 있는 헤더, 그리고 응답과 요청 메시지 양쪽 모두에서 정로블 제공하는 헤더가 있다
- 헤더는 크게 다섯 가지로 분류된다

**일반 헤더 (General Headers)**

- 일반헤더는 클라이언트와 서버 양쪽 모두가 사용ㅎ나다. 이들은 클라이언트, 서버, 그리고 어딘가에 메시지를 보내는 다른 애플리케이션들을 위해 다양한 목적으로 사용된다

  ```
  //Date헤더는 서버와 클라이언트를 가리지 않고 메시지가 만들어진 일시를 지칭하기 위해 사용하는 일반 목적 헤더이다

  Data: Tue, 3 Oct 1974 02:16:00 GMT
  ```

**요청 헤더 (Request Headers)**

- 이름에서 드러나는 것과 갍이 , 요청 헤더는 요청 메시지를 위한 헤더다. 그들은 서버에게 클라이언트가 받고자 하는 데이터의 타입이 무엇인지와 같은 부가 정보를 제공한다

  ```
  //다음의 Accept 헤더는 서버에세 클라이언트가 자신의 요청에 대응하는 어떤 미디어 타입도 받아들일 것임을 의미한다

  Accept: */*
  ```

**응답 헤더 (Response Headers)**

- 응답 메시지는 클라이언트에게 정보를 제공하기 위한 자신만의 헤더를 갖고 있다

  ```
  //다음의 Server 헤더는 클라이언트에게 그가 Tiki-Hut 서버 1.0 버전과 대화하고 있음을 말해준다

  Server: Tiki-Hut/1.0
  ```

**엔터티 헤더 (Entity Headers)**

- 엔터티 헤더란 엔터티 본문에 대한 헤더를 말한다. 예를 들어, 엔터티 헤더는 엔터티 본문에 들어있는 데이터의 타입이 무엇인지 말해줄 수 있다

  ```
  //다음 Content-Type헤더는 애플리케이션에게 데이터가 iso-latin-1 문자집합으로 된 HTML 문서임을 알려준다
  Content-Type: text/html; charset=iso-latin-1
  ```

**확장 헤더 (Extension Headers)**

- 확장 헤더는 애플리케이션 개발자들에 의해 만들어졌지만 아직 승인된 HTTP 명세에는 추가되지 않은 비표준 헤더다
- HTTP 프로그램은 확장 헤더들에 대해 설령 그 의미를 모른다 할지라도 용인하고 전달해야 할 필요가있다

### **3.5.1 일반 헤더**

- 어떤 헤더는 메시지에 대한 아주 기본적인 정보를 제공한다. 이 헤더들으 일반 헤더라고 불린다. 그들은 메시지가 어떤 종류이든 상관없이 유용한 정보를 제공한다
  - **헤더: Connection**
    - 클라이언트와 서버가 요청/응답 연결에 대한 옵션을 정할 수 있게 해준다
  - **헤더: Date**
    - 메시지가 언제 만들어졌는지에 대한 날짜와 시간을 제공한다
  - **헤더: MIME-Version**
    - 발송자가 사용한 MIME의 버전을 알려준다
  - **헤더: Trailer chunked transter**
    - 인코딩으로 인코딩된 메시지의 끝 부분에 위치한 헤더들의 목록을 나열한다
  - **헤더: Transter-Encoding**
    - 수신자에게 안전한 전송을 위해 메시지에 어떤 인코딩이 적용되었는지 말해준다
  - **헤더: Upgrage**
    - 발송자가 '업그레이드'하길 원하는 새 버전이나 프로토콜을 알려준다
  - **헤더: Via**
    - 이 메시지가 어떤 중개자(프락시, 게이트웨이)를 거쳐왔는지 보여준다

**일반 캐시 헤더**

- HTTP/1.0은 HTTP 애플리케이션에게 매번 원 서버로부터 객체를 가져오는 대신 로컬 복사본으로 캐시할 수 있도록 해주는 최초의 헤더를 도립했다. 최신버전의 HTTP는 매우 풍부한 캐시 매개변수의 집합을 가지고 있다
  - 헤더: Cache-Control
    - 메시지와 함께 캐시 지시자를 전달하기 위해 사용한다
  - 헤더: Pragma
    - 메시지와 함께 지시자를 전달하는 또 다른 방법. 캐시에 국한되지 않는다

### **3.5.2 요청 헤더**

- 요청 헤더는 요청 메시지에서만 의미를 갖는 헤더다. 그들은 요청이 최초 발생한 곳에서 누가 혹은 무엇이 그 요청을 보냈는지에 대한 정보나 클라이언트의 선호나 능력에 대한 정보를 준다
- 서버는 요청 헤더가 준 클라리언트에 대한 그 정보를 클라이언트에게 더 나은 응답을 주기 위해 활용할 수 있다
  - **헤더: Client-IP**
    - 클라이언트가 실행된 컴퓨터의 IP를 제공한다
  - **헤더: From**
    - 클라이언트 사용자의 메일 주소를 제공한다
  - **헤더: Host**
    - 요청의 대상이 되는 서버의 호스트 명과 포트를 준다
  - **헤더: Refer**
    - 현재의 요청 URI가 들어있던 문서의 URL을 제공한다
  - **헤더: UA-Color**
    - 클라이언트 기기 디스플레이의 색상 능력에 대한 정보를 제공한다
  - **헤더: UA-CPU**
    - 클라이언트 CPU의 종류나 제조사를 알려준다
  - **헤더: UA-Disp**
    - 클라이언트의 디스플레이 능력에 대한 정보를 제공한다
  - **헤더: UA-OS**
    - 클라이언트 기기에서 동작 중인 운영체제의 이름과 버전을 알려준다
  - **헤더: UA-Pixels**
    - 클라이언트 기기 디스플레이에 대한 픽셀 정보를 제공한다
  - **헤더: User-Agent**
    - 요청을 보낸 애플리케이션의 이름을 서버에게 말해준다

**Accept 관련 헤더**

- 클라이언트는 Accept관련 헤더들은 이용해 서버에게 자신의 선호와 능력을 알려줄 수 있다. 즉 클라이언트가 무엇을 원하고 무엇을 할 수 있는지, 그리고 무엇보다도 원치 않는 것은 무엇인지 알려줄 수 있다
- 서버는 그 후 이 추가 정보를 활용해서 무엇을 보낼 것인가에 대해 더똑똑한 결정을 내릴 수 있다. Accept 관련 헤더들은 서버와 클라이언트 양쪽 모두에게 유익하다. 클라이언트는 그들이 원하는 것을 얻을 수 있으며, 서버는 클라이언트가 사용할 수도 없는 것을 전송하는데 시간과 대역폭을 낭비하지 않을 수 있다
  - 헤더: Accept
    - 서버에게 서버가 보내도 되는 미디어 종류를 말해준다
  - 헤더; Accept-Charset
    - 서버에게 서버가 보내도 되는 문자집합을 말해준다
  - 헤더: Accept-Encoding
    - 서버에게 서버가 보내도 되는 인코딩을 말해준다
  - 헤더: Accpet-Language
    - 서버에게 서버가 보내도 되는 언어를 말해준다
  - 헤더: TE
    - 서버에게 서버가 보내도 되는 확장 전송 코딩을 말해준다

**조건부 요청 헤더**

- 클라이언트는 요청에 몇몇 제약을 넣기도 한다
- 조건부 요청 헤더를 사용하면, 클라이언트는 서버에게 요처엥 응답하기 전에 먼저 조건이 참인지 확인하게 하는 제약을 포함시킬 수 있다
  - 헤더: Expect
    - 클라이언트가 요청에 필요한 서버의 행동을 열거할 수 있게 해준다
  - 헤더: If-Match
    - 문서의 엔터티 태그가 주어진 엔터티 태그와 일치하는 경우에만 문서를 가져온다
  - 헤더: If-Modified-Since
    - 주어진 날짜 이후에 리소스가 변경되지 않았다면 요청을 제한한다
  - 헤더: If-None-Match
    - 문서의 엔터티 태그가 주어진 엔터티 태그와 일치하지 않는 경우에만 문서를 가져온다
  - 헤더: If-Range
    - 문서의 특정 범위에 대한 요청을 할 수 있게 해준다
  - 헤더: If-Unmodified-Since
    - 주어진 날짜 이후에 리소스가 변경되었다면 요청을 제한한다
  - 헤더: Range
    - 서버가 범위 요청을 지원한다면, 리소스에 대한 특정 범위를 요청한다

**요청 보안 헤더**

- HTTP는 자체적으로 요청을 위한간단한 인증요구/응답 체계를 갖고 있다. 그것은 요청하는 클라이언트가 어은 정보의 리소스에 접근하기 전에 자신을 인증하게 함으로써 트랜잭션을 약간 더 안전하게 만들고자 한다
  - **헤더: Authorization**
    - 클라이언트가 서버에게 제공하는 인증 그 자체에 대한 정보를 담고 있다
  - **헤더: Cookie**
    - 클라이언트가 서버에게 토큰을 전달할 때 사용한다. 진짜 보안 헤더는 하니지만, 보안에 영향을 줄 수 있다는 것은 확실하다
  - **헤더: Cookie2**
    - 요청자가 지원하는 쿠키의 버전을 알려줄 때 사용한다

**프락시 요청 헤더**

- 인터넷에서 프락시가 점점 흔해지면서, 그들의 기능을 돕기 위해 몇몇 헤더들이 정의되어 왔다
  - 헤더: Max-Forwards
    - 요청이 원 서버로 향하는 과정에서 다른 프락시나 게이트웨이로 전달될 수 있는 최대 획수. TRACE 메서드와 함께 사용된다
  - 헤더: Proxy-Authorization
    - Authorization과 같으나 프락시에서 인증을 할 때 쓰인다
  - 헤더: Proxy-Connection
    - Connection과 같으나 프락시에서 연결을 맺을 때 쓰인다

### **응답 헤더**

- 응답 메시지는 그들만의 응답 헤더를 갖는다. 응답 헤더는 클라이언트에게 부가 정보를 제공한다. 누가 응답을 보내고 있는지 혹은 응답자의 능력은 어떻게 되는지 알려주며, 더 나아가 응답에 대한 특별한 설명도 제공할 수 있다
- 이 헤더들은 클라이언트가 응답을 잘 다루고 나중에 더 나은 요청을 할 수 있도록 도와준다
  - **헤더: Age**
    - 응답이 얼마나 오래걸렸는지
  - **헤더: Public**
    - 서버가 특정 리소스에 대해 지원하는 요청 메서드의 목록
  - **헤더: Retry-After**
    - 현재 리소스가 사용 불가능한 상태일 때, 언제 가능해지는 날짜 혹은 시각
  - **헤더: Server**
    - 서버 애플리케이션의 이름과 버전
  - **헤더: Title**
    - HTML 문서에서 주어진 것과 같은 제목
  - **헤더: Warning**
    - 사유 구절에 있는 것보다 더 자세한 경고 메시지

**협상 헤더**

- 서버에 프랑스어와 독일어로 번역된 HTML문서가 있는 경우와 같이 여러가지 표현이 가능한 상황이라면, HTTP/1.1은 서버와 클라이언트가 어떤 표현을 택할 것인가에 대한 협상을 할 수 있도록 지워한다
  - **헤더: Accept-Ranges**
    - 서버가 자원에 대해 받아들일 수 있는 범위의 형태
  - **헤더: Vary**
    - 서버가 확인해 보아야 하고 그헣기 때문에 응답에 영향을 줄 수 있는 헤더들의 몰록

**응답 보안 헤더**

- 기본적으로 HTTP인증요구/응답 체계에서 응답 측에 해당하는 요청 보안헤더이다
  - **헤더: Proxy-Authenticate**
    - 프락시에서 클라이언트로부터 보낸 인증요구의 목록
  - **헤더: Set-Cookie**
    - 진짜 보안 헤더는 아니지만, 보안에 영향을 줄 수 있다. 서버가 클라이언트를 인증할 수 있도록 클라이언트 측에 토큰을 설정하기 위해 사용한다
  - **헤더: WWW-Authenticate**
    - 서버에서 클라이언트로 보낸 인증요구의 목록

### **3.5.4 엔터티 헤더**

- HTTP 메시지의 엔터티에 대해 설명하는 헤더들은 많다. 요청과 응답 양쪽 모두 엔터티를 포함할 수 있기 때문에, 이 헤더들은 양 타입의 메시지에 모두 나타날 수 있다
- 엔터티 헤더는 에넡티와 그것은 내용물에 대한, 개체의 타입부터 시작해서 주어진 리소스에 대해 요청 할 수 있는 유효한 메서드들까지, 광범위한 정보를 제공한다
- 일반적으로 엔터티 헤더는 메시지의 수신자에게 자신이 다루고 이쓴 것이 무엇인지 말해준다
  - **헤더: Allow**
    - 이 엔터티에 대해 수행될 수 있는 요청 메서드들을 나열한다
  - **헤더: Location**
    - 클라이언트에게 엔터티가 실제로 어디에 위치하고 있는지 말해준다. 수신자에게 리소스에 대한 위치(URL)를 알려줄 때 사용한다'

**콘텐츠 헤더**

- 콘텐츠 헤더는 엔터티의 콘텐츠에 대한 구체적인 정보를 제공한다
- 콘텐츠의 종류, 크기, 기타 콘텐츠를 처리할 때 유용하게 활옹될 수 있는 것들이다

  - **헤더: Conetent-Base**
    - 본문에서 사용된 상대 URL을 계산하기 위한 기저 URL
  - **헤더: Conetent-Encoding**
    - 본문에 적용된 어떤 인코딩
  - **헤더: Content-Language**
    - 본문을 이해하는데 가장 적절한 자연어
  - **헤더: Content-Legnth**
    - 본문의 길이나 크기
  - **헤더: Content-Location**
    - 리소스가 실제로 어디에 위치하는지
  - **헤더: Content-MD5**
    - 본문의 MD5 체크섬
  - **헤더: Content-Range**
    - 전체 리소스에서 이 엔터티가 해당하는 범위를 바이트 단위로 표현
  - **헤더: Content-Type**
    - 이 본문이 어떤 종류의 객체인지

- **엔터티 캐싱 헤더**

- 일반 캐싱 헤더는 언제 어떻게 캐시가 되어야 하는지에 대한 지시자를 제공한다
- 엔터티 캐싱 헤더는 엔터티 캐싱에 대한 정보를 제공한다
  - **헤더: Etag**
    - 이 엔터티에 대한 엔터티 태그
  - **헤더: Expires**
    - 이 엔터티가 더 이상 유효하지 않아 원본을 다시 받아와야 하는 일시
  - **헤더: Last-Modified**
    - 가장 최근 이 엔터티가 변경된 일시
