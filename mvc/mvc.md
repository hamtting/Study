# 웹서버,웹 애플리케이션 서버 알아보기

## 웹세상은 HTTP 위에서 동작 (HTTP는 우주 웹은 지구🌎)
- 웹은 HTTP 프로토콜 기반에서 동작한다.
- HTTP 메시지에 HTML,TEXT,이미지,음성,파일,영상,API를 가지고 사용하는 JSON,XML 거의 모든 형태의 데이터를 담아서 전송이 가능하다.
- 서버간에 데이터를 주고 받을 때도 대부분 HTTP를 사용한다.

## 웹서버(정해진건 빠르게 응답 가능해!유동적인건 대답못하니까 묻지마..)
- 정적 리소스를 제공, 기타 부가 서비스도 제공한다.
- 음악,영상,css,js,이미지등을 제공한다.
- 예로 NGINX,APACHE가 있다.
- 클라이언트가 HTTP 프로토콜을 사용해서 특정 URL로 HTTP요청메세지를 보내면 웹서버는 해당하는 요청을 받아서 처리하고 그에 따른 HTTP응답 메시지를 생성해서 보내준다.
- HTTP응답 메시지 안에는 상태코드랑 HTML이 들어가 있는데 HTTP응답메시지를 받은 웹브라우저가 해석해서 클라이언트에게 화면을 보여준다.

## 웹 어플리케이션 서버(난 뭐든 다 대답해줄 수 있어 그래도 정해진 질문은 웹서버한테 물어봐!)
- 정적 리소스를 제공(웹 서버 기능 포함)와 동적 리소스를 제공한다.
- 동적 HTML,서블릿,JSP,스플링 MVC등 프로그램 코드를 실행해서 애플리케이션 로직을 수행한다.
- 톰캣(Tomcat),Jetty,Undertow등 이러한 was 서버들이 있다.

## 웹 서버(web) vs 웹 어플리케이션 서버(was)
- 요즘에는 web서버도,was도 정적리소스,동적 리소스 제공해준다.(용어 경계도 모호)
- 그러나 was는 애플리케이션 코드를 실행하는데 더 특화되어있다. 

## 웹 시스템 구성(db와 was로만 정말 가능할까?)
- was랑 db만 있으면 충분히 웹 시스템 구성 가능하다.
- was는 정적,동적 리소스 제공이 가능하기 때문이지(web 미안)
- 그러나 was만 있으면 복잡한 동적 리소스 처리해야하는데 정적 리소스도 처리해야해서 과부화 걸릴 가능성이 높다.
- was가 죽으면 오류화면 노출도 불가능하다.
- 이러한 이유로 was가 애플리케이션 로직을 잘 수행할 수 있게 was와 web으로 업무 분담을 시켜 리소르를 효율적으로 관리한다.(역시 web 있어야해)

## 웹 시스템 구성은 web,was,db
<img src="./images/mvc1.PNG" width="800" height="500"/>
- 일반적인 웹 시스템을 보면 웹서버를 앞에 두고 css,js,html등 정적 리소스를 먼저 다 처리한다.
- 그러다가 웹서버가 처리할 수 없는 동적 리소스를 요청하는 url이 들어오면 웹 애플리케이션 서버에게 넘겨준다.
- was는 db에서 데이터를 찾아 결과를 웹서버에게 내려주고 웹서버는 그 결과를 브라우저로 내려준다.
- 이렇게 시스템 구성을하면 was가 중요한 애플리케이션 로직을 전담할 수 있고 시스템이 안정적이고 리소르를 효율적으로 관리할 수 있다.

## 효율적인 리소스 관리
- 정적 리소스가 많이 사용되면 web 서버 증설하고 애플리케이션 리소스가 많이 사용되면 was를 증설하면된다.

## 오류 대비
<img src="./images/mvc2.PNG" width="800" height="500"/>

- 애플리케이션 로직이 들어가 있는 was는 잘 죽는다. db의 영향을 받아서 죽기도 한다.
- 대부분의 장애는 web이 아닌 was에서 난다.
- web이 동적 처리를 was에 요청했는데 was가 죽거나 연결이 잘 안될 경우 was에 오류가 있다는 걸 인지하고 오류화면 HTML을 내려준다.

## 서블릿 존재 이유 (코드 자동화,비즈니스 로직에 집중)
- WAS는 서블릿을 지원한다.
- 서블릿이 없었다면 TCP/IP 연결하는거부터시작해서 HTTP요청메시지 분석하고 HTTP응답메시지를 직접 구현해야한다. 이후 소켓도 종료시켜줘야한다.
- 서블릿이 없을 때 구현해야하는 과정들은 요청이 들어왔을 때 모두 동일하게 구현되야한다.
- 비즈니스 로직을 제외하고 동일한 과정인데 비효율적으로 같은 코드를 작성해야한다.
- 그런데! 서블릿이 동일한 과정에 대해서 자동으로 처리해준다.
- 서블릿을 사용하면 비즈니스 로직만 구현하면된다

## 서블릿
<img src="./images/mvc3.PNG" width="800" height="500"/>

- extends HttpServlet 상속받아서 사용한다.
- HTTP 요청이 들어오면 WAS가 HttpServletRequest와HttpServletResponse 객체를 새롭게 만들어서 서블릿 객체를 호출한다. 
- 서블릿 객체가 호출되면 비즈니스 로직이 구현된 service()가 실행된다.
- service() 호출시 2개의 파라미터가 매개변수로 들어가게된다.
  1) HTTP 요청 정보를 편리하게 사용할 수 있는 HttpServletRequest
  2) HTTP 응답 정보를 편리하게 제공할 수 있는 HttpServletResponse
- HttpServletRequest에서 요청 시 주는 데이터를 편하게 뽑아서 쓸 수 있고 HttpServletResponse는 응답시 편하게 데이터를 세팅해서 보내줄 수 있게 해준다.
- 개발자는 HTTP 스펙을 매우 편리하게 사용할 수 있다.
  
> HTTP 스펙 : HTTP 메서드, HTTP 상태 코드, HTTP 헤더,HTTP 메시지 구조(요청메시지,응답메시지)

## WAS와 서블릿
<img src="./images/mvc4.PNG" width="800" height="500"/>

- 서블릿을 지원하는 WAS안에는 서블릿 컨테이너가 있다.
- WAS는 서블릿 컨테이너 안에 서블릿 객체를 생성한다. 
- WAS가 올라갈 때 서블릿을 같이 실행하고 WAS가 내려갈 때 서블릿을 종료한다.(서블릿 생명주기)
- WAS는 서블릿 컨테이너 안에 여러개의 섭블릿을 모두 관리한다.
  
