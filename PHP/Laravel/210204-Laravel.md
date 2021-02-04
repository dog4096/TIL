# 라라벨의 디렉토리 구조

## Root

***
### App
***
Console, Exception, Http 등 핵심 코드들이 대부분 들어가 있다. 기본으로 생성된 폴더 이외에 사용자가 추가 디렉토리도 대부분 여기에 생성하게 된다.
### Bootstrap
***
프레임워크 부팅시 필요한 Application 클래스를 생성. Route, Service등의 캐시파일도 존재
###Config
***
어플리케이션의 설정 파일이 존재. 데이터베이스도 이곳에서 설정하나 보통 env함수를 통해 설정하므로 .env 파일에서 설정
###Database
***
Migration을 위한 Migrations 폴더와 더미 데이터를 위한 factories폴더 더미데이터를 삽입하는 seeders 폴더가 있다. SQLite의 데이터를 위한 폴더로 할 수도 있다.   
응용하면 자동으로 데이터베이스를 구축하고 더미 데이터를 삽입하는 등의 행동을 할 수 있다.
###Public
***
진입점인 index.php가 존재한다. 컴파일된 css, javascript 등의 파일들도 이곳에 포함된다. (직접 수정하는게 아님)
###Resources
***
view 파일 (.blade.php), LESS, SASS 자바스크립트 컴파일 되기 이전 파일들이 포함된다. (이곳이 프론트작업의 메인)
###Routes
***
모든 라우트들을 정의하는 곳이다.  
web.php는 Laravel에서 web 미들웨어 그룹에 기본 정의된 Middleware들을 거친다. (세션, CSRF 보호, 쿠키 암호화 등)  
api.php는 Laravel에서 api 미들웨어 그룹에 기본 정의된 Middleware들을 거친다. (접속속도 제한, Stateless, 토큰 인증 등)
###Storage
***
컴파일 된 blade 템플릿, 파일 기반 세션, 파일 캐시, 로그 등의 파일이 있다. 직접 건들지는 않을 것 이다.
###Test
***
PHPUnit 테스트를 위한 폴더.
###Vendor
***
composer 의존성 폴더.
