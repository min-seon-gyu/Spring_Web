# 학습시작기간 2023.02.26

## 웹 애플리케이션의 이해
웹 - HTTP 기반
HTTP 메세지에 모든 것을 전송
- HTML, TEXT
- IMAGE, 음성, 영상, 파일
- JSON, XML, API
- 거의 모든 형태의 데이터 전송 가능
- 서버간에 데이터를 주고 받을 때에도 HTTP 사용

### 웹 서버
- HTTP 기반으로 동작
- 정적 리소스 제공, 기타 부가기능
- 정적(파일) HTML, CSS, JS, 이미지, 영상
- 예) NGINX, APACHE

### 웹 애플리케이션 서버(WAS - Web Application Server)
- HTTP 기반으로 동작
- 웹 서버 기능 포함+ (정적 리소스 제공 가능)
- 프로그램 코드를 실행해서 애플리케이션 로직 수행
	- 동적 HTML, HTTP API(JSON)
	- 서블릿, JSP, 스프링 MVC
- 예) Tomcat, Jetty, Undertow

### 웹 서버와 웹 애플리케이션 서버 차이
- 웹 서버는 정적 리소스(파일), WAS는 애플리케이션 로직
- 용어와 경계가 애매하다
	- 웹 서버도 프로그램을 실행하는 기능을 포함
	- WAS도 웹 서버의 기능을 제공
- JAVA는 서블릿 컨테이너 기능을 제공하면 WAS
	- 서블릿 없이 JAVA 코드를 실행하는 서버 프레임워크도 있음
- WAS는 애플리케이션 코드를 실행하는데 더 특화

### 웹 시스템 구성 - WAS, DB

![](https://velog.velcdn.com/images/gcael/post/b7488d13-5a61-435b-a8cf-f07f6cbd58b7/image.PNG)

- WAS, DB만으로 시스템 구성 가능
- WAS는 정적 리소스, 애플리케이션 로직 모두 제공 가능

단점
- WAS가 너무 많은 역할을 담당, 서버 과부화 우려
- 가장 비싼 애플리케이션 로직이 정적 리소스 때문에 수행이 어려울 수 있다.
- WAS 장애 시 오류 화면도 출력 불가능

### 웹 시스템 구성 - WEB, WAS, DB

![](https://velog.velcdn.com/images/gcael/post/8be3a05d-486e-4bf8-96e8-5d57dde00aa2/image.PNG)

- 정적 리소스는 웹 서버가 처리
- 웹 서버는 애플리케이션 로직같은 동적인 처리가 필요하면 WAS에 요청을 위임
- WAS는 중요한 애플리케이션 로직 처리 전담
- 효율적인 리소스 관리
	- 정적 리소스가 많이 사용되면 WEB 서버 증설
	- 애플리케이션 리소스가 많이 사용되면 WAS 증설
- 정적 리소스만 제공하는 웹 서버는 잘 죽지 않음
- 애플리케이션 로직이 동작하는 WAS 서버는 잘 죽음
- WAS, DB 장애시, WEB 서버가 오류 화면 제공 가능

### 서블릿
HTTP 요청, 응답 흐름
- HTTP 요청시
	- WAS는 Request, Response 객체를 새로 만들어서 서블릿 객체 호출
	- 개발자는 Request 객체에서 HTTP 요청 정보를 편리하게 꺼내서 사용
	- 개발자는 Response 객체에 HTTP 응답 정보를 편리하게 입력
	- WAS는 Response 객체에 담겨있는 내용으로 HTTP 응답 정보를 생성

### 서블릿 컨테이너
- 톰캣처럼 서블릿을 지원하는 WAS를 서블릿 컨테이너라고 한다.
- 서블릿 컨테이너는 서블릿 객체를 생성, 초기화, 호출, 종료하는 생명주기 관리
- 서블릿 객체는 싱글톤으로 관리
	- 최초 로딩 시점에 서블릿 객체를 미리 만들어놓고 재활용
	- 공유 변수 사용 주의
	- 서블릿 컨테이너 종료시 함께 종료
- JSP도 서블릿으로 변환 되어서 사용
- 동시 요청을 위한 멀티 스레드 처리 지원

### 스레드
- 애플리케이션 코드를 하나하나 순차적으로 실행하는 것은 스레드
- 자바 메인 메서드를 처음 실행하면 main이라는 이름의 스레드가 실행
- 스레드가 없다면 자바 애플리케이션 실행이 불가능
- 스레드는 한 번에 하나의 코드 라인만 수행
- 동시 처리가 필요하면 스레드를 추가 생성

### 요청 마다 스레드 생성 장단점
장점
- 동시 요청을 처리할 수 있다.
- 리소스(CPU, 메모리)가 허용할 때 까지 처리가능
- 하나의 스레드가 지연 되어도, 나머지 스레드는 동작이 가능하다.

단점
- 스레드는 생성 비용이 너무 비싸다.
- 고객의 요청이 올 때마다 생성한다면, 응답 속도가 늦어진다.
- 스레드는 컨텍스트 스위칭 비용이 발생한다.
- 스레드 생성에 재한이 없다.
	- 고객 요청이 너무 많이 오면 CPU, 메모리 임계점을 넘어서 서버가 죽을 수 있다.

### 스레드 풀
- 특징
	- 필요한 스레드를 스레드 풀에 보관하고 관리한다.
	- 스레드 풀에 생성 가능한 스레드의 최대치를 관리한다. 톰캣은 최대 200개 기본설정(변경 가능)
- 사용
	- 스레드가 필요하면, 이미 생성되어 있는 스레드를 스레드 풀에서 꺼내서 사용한다.
	- 사용을 종료하면, 스레드 풀에 해당 스레드를 반납한다.
	- 최대 스레드가 모드 사용중이면 기다리는 요청을 거절하거나 특정 숫자만큼만 대기하도록 설정 할 수 있다.
- 장점
	- 미리 스레드가 생성되어 있으므로, 스레드를 생성, 종료하는 비용이 절약되고 응답시간이 빠르다
	- 생성 가능한 스레드의 최대치가 있으므로, 너무 많은 요청이 들어와도 기존 요청은 안전하게 처리할 수 있다.

### 스레드 풀(실무 팁)
- WAS의 주요 튜닝 포인트는 최대 스레드 수이다.
- 이 값을 너무 높게 설정했을 때 동시 요청이 많으면 CPU, 메모리 리소스 임계점 초과로 서버 다운
- 이 값을 너무 낮게 설정했을 때 동시 요청이 많으면 서버 리소스는 여유롭지만, 클라이언트는 금방 응답 지연
- 클라우드면 우선 서버를 늘리고 튜닝, 아니면 열심히 튜닝

### 스레드 풀의 적정 숫자
- 애플리케이션 로직의 복잡도, CPU, 메모리, IO 리소스 상황에 따라 모두 다르다.
- 성능 테스트
	- 최대한 실제 서비스와 유사하게 성능 테스트 시도
	- 툴 : nGrinder, APACHE ab

### WAS의 멀테 스레드 지원의 핵심
- 멀티 스레드에 대한 부분은 WAS가 처리 한다.
- 멀티 스레드 관련 코드를 신경쓰지 않아도 된다.
- 멀티 스레드 환경이므로 싱글톤 객체(서블릿, 스프링 빈)는 주의해서 사용한다.

### HTTP API
- HTML이 아니라 데이터를 전달한다. UI가 필요하면 클라이언트에서 별도로 처리한다.
- 주로 JSON 형식을 사용한다.
- 다양한 시스템에서 호출한다.
- 웹 클라이언트, 앱, 서버 TO 서버

### 서버 사이드 렌더링, 클라이언트 사이드 렌더링
- SSR - 서버 사이드 렌더링
	- HTML 최종 결과를 서버에서 만들어서 웹 브라우저에 전달
	- 주로 정적인 화면에 사용
	- 관련기술 : JSP, 타임리프
- CSR - 클라이언트 사이드 렌더링
	- HTML 결과를 자바스크립트를 사용해 웹 브라우저에서 동적으로 생성해서 적용
	- 주로 동적인 화면 사용, 웹 환경을 마치 앱 처럼 필요한 부분부분 변경가능
	- 예) 구글 지도, Gmail
	- 관련기술 : React, Vue.js

## 서블릿
서블릿은 톰캣 같은 웹 애플리케이션 서버를 직접 설치하고,그 위에 서블릿 코드를 클래스 파일로 빌드해서 올린 다음, 톰캣 서버를 실행하면 된다. 하지만 이 과정은 매우 번거롭다.
스프링 부트는 톰캣 서버를 내장하고 있으므로, 톰캣 서버 설치 없이 편리하게 서블릿 코드를 실행할 수 있다.

스프링 부트는 서블릿을 직접 등록해서 사용할 수 있도록 @ServletComponentScan 을 지원한다.

```java
@WebServlet(name = "helloServlet", urlPatterns = "/hello")
public class HelloServlet extends HttpServlet {

	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response)
		throws ServletException, IOException {
	}
}
```
@WebServlet 서블릿 애노테이션
- name: 서블릿 이름
- urlPatterns: URL 매핑
	
HTTP 요청을 통해 매핑된 URL이 호출되면 서블릿 컨테이너는 다음 메서드를 실행한다.

	protected void service(HttpServletRequest request, HttpServletResponse response)

HTTP 요청 메시지 로그로 확인하기

	application.properties에 logging.level.org.apache.coyote.http11=debug를 추가하면 된다.
	
### 서블릿 컨테이너 동작 방식 설명

#### 내장 톰캣 서버 생성
![](https://velog.velcdn.com/images/gcael/post/09372ba9-563a-4bbb-b91c-0ba36c7d55c7/image.PNG)

####  애플리케이션 서버의 요청 응답 구조
![](https://velog.velcdn.com/images/gcael/post/3a958070-45d0-4cf1-8466-e147278adf64/image.PNG)

### HttpServletRequest 역할
HTTP 요청 메시지를 개발자가 직접 파싱해서 사용해도 되지만, 매우 불편할 것이다. 서블릿은 개발자가 HTTP 요청 메시지를 편리하게 사용할 수 있도록 개발자 대신에 HTTP 요청 메시지를 파싱한다. 그리고 그 결과를 HttpServletRequest 객체에 담아서 제공한다.

#### HTTP 요청 메세지
```
POST /save HTTP/1.1
Host: localhost:8080
Content-Type: application/x-www-form-urlencoded
username=kim&age=20
```

- START LINE
	- HTTP 메소드
	- URL
	- 쿼리 스트링
	- 스키마, 프로토콜
- 헤더
	- 헤더 조회
- 바디
	- form 파라미터 형식 조회
	- message body 데이터 직접 조회

#### HttpServletRequest 부가 기능

임시 저장소 기능
- 해당 HTTP 요청이 시작부터 끝날 때 까지 유지되는 임시 저장소 기능
	- 저장: request.setAttribute(name, value)
	- 조회: request.getAttribute(name)

세션 관리 기능
- request.getSession(create: true)

### HTTP 요청 데이터 
- GET - 쿼리 파라미터
- POST - HTML Form
- HTTP message body에 데이터를 직접 담아서 요청

#### HTTP 요청 데이터 - GET 쿼리 파라미터
메시지 바디 없이, URL의 쿼리 파라미터를 사용해서 데이터를 전달한다.
예) 검색, 필터, 페이징등에서 많이 사용하는 방식

```java
String username = request.getParameter("username"); //단일 파라미터 조회
Enumeration<String> parameterNames = request.getParameterNames(); //파라미터 이름들 모두 조회
Map<String, String[]> parameterMap = request.getParameterMap(); //파라미터를 Map으로 조회
String[] usernames = request.getParameterValues("username"); //복수 파라미터 조회
```

복수 파라미터에서 단일 파라미터 조회
> username=hello&username=kim 과 같이 파라미터 이름은 하나인데, 값이 중복이면 어떻게 될까?
request.getParameter() 는 하나의 파라미터 이름에 대해서 단 하나의 값만 있을 때 사용해야 한다. 
지금처럼 중복일 때는 request.getParameterValues() 를 사용해야 한다.
참고로 이렇게 중복일 때 request.getParameter() 를 사용하면 request.getParameterValues()의 첫 번째 값을 반환한다.

#### HTTP 요청 데이터 - POST - HTML Form
주로 회원 가입, 상품 주문 등에서 사용하는 방식이다

##### 특징
- content-type: application/x-www-form-urlencoded
- 메시지 바디에 쿼리 파리미터 형식으로 데이터를 전달한다. username=hello&age=20
- application/x-www-form-urlencoded 형식은 앞서 GET에서 살펴본 쿼리 파라미터 형식과 같다. 따라서 쿼리 파라미터 조회 메서드를 그대로 사용하면 된다.

> GET URL 쿼리 파라미터 형식으로 클라이언트에서 서버로 데이터를 전달할 때는 HTTP 메시지 바디를
사용하지 않기 때문에 content-type이 없다.


> POST HTML Form 형식으로 데이터를 전달하면 HTTP 메시지 바디에 해당 데이터를 포함해서 보내기
때문에 바디에 포함된 데이터가 어떤 형식인지 content-type을 꼭 지정해야 한다. 이렇게 폼으로 데이터를
전송하는 형식을 application/x-www-form-urlencoded 라 한다.

## 서블릿, JSP, MVC 패턴

## MVC 프레임워크 만들기

## 스프링 MVC - 구조 이해

## 스프링 MVC - 기본기능

## 스프링 MVC - 웹 페이지 만들기
