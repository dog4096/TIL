## Laravel MiddleWare 생성
1. 먼저 \App\Http\MiddleWare 폴더안에 클래스를 생성한다.<br>
  -별도의 클래스를 상속받을 필요는 없다.
2. public function handle(Request $request, Closure $next) 함수를 작성한다. <br>
3. $next($request)를 호출하는 시점이 Http 리퀘스트를 처리하는 시점이다. <br>
 -$next($request)를 호출하는 기준으로 앞에 작성하면 요청을 처리하기 전에 실행되고 <br>
  밑에 작성하면 요청이 처리된 후 실행된다.
4. \app\Http\Kernel.php 에 해당 MiddleWare에 대한 설정을 한다.````