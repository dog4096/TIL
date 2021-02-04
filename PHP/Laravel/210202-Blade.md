# 블레이드 템플릿
- 참고 및 출처 : https://laravel.kr/docs/8.x/blade
## 블레이드
***
- .blade.php 형식의 파일 확장자를 사용하고 보통 resources/views 폴더에 저장된다.  
- 평소 사용하는 html + php 코드를 그대로 작성해도 정상적으로 작동된다.
## 템플릿 구조
### 템플릿 상속
#### 부모 레이아웃 템플릿
***
- https://laravel.kr/docs/8.x/blade#defining-a-layout 에 쓰여있는 레이아웃 템플릿 예시를 약간 변형한 코드  
```
<!-- Stored in resources/views/layouts/app.blade.php -->
<html>
    <head>
        <title>App Name - {{ $title }}</title>
        @yield("css")
    </head>
    <body>
        <!-- resources/views/layouts/header.blade.php -->
        @include('layouts.header')
        @section('sidebar')
            This is the master sidebar.
        @show

        <div class="container">
            @yield('content')         `
        </div>
        <!-- resources/views/layouts/footer.blade.php -->
        @include('layouts.footer')
    </body>
</html>
```
위의 코드가 상속받게 될 레이아웃 코드의 예시이다.
- @yield("이름")  
  자식 View 페이지에서 @section("이름")으로 선언된 부분의 코드를 그대로 삽입한다.
- @section("이름")  
  @yield("이름") 으로 선언된 부분에 그대로 삽입되는 코드이다. 보통 @endsection 이나 @stop으로 끝 부분을 알린다.
> 주의할점은 @section으로 코드를 선언했다고 해서 그 부분에 html코드가 그대로 삽입되는게 아니라는 사실이다.  
> 반드시 해당 위치에 @yield를 선언해야 @section의 코드가 삽입된다.  
> (코딩하는 입장에선 변수만 선언하고 사용하지 않은 것과 비슷하다.)

> 또 하나 주의할점은 자식 레이아웃에서 같은 이름으로 중복 선언하게 되면 덮어쓰게 된다. 그걸 방지하기 위해 @parent를 사용한다.  
> 자세한건 밑의 자식 레이아웃 템플릿 부분에서 설명한다. 
- @show  
  @endsection @yield("이름") 이 두가지가 합쳐진 코드이다. 바로 위에 선언된 @section("이름")을 바로 해당 위치에 삽입한다.  
- @include("파일경로")  
  지정된 파일의 코드를 그대로 삽입한다. (php의 include와 비슷하다.)  
  폴더 경로 구분은 역슬래시가 아닌 .으로 표현되며 파일 이름은 절대로 .을 포함되면 안된다. 따라서 .blade.php의 확장자도 생략한다. 
> 모든 상속받은 View들이 동일한 header와 footer를 쓴다면 이곳에서 @include가 좋을 것이다. 이렇게하면 자식들 View에서 별도로 선언할 필요가 없다.  
> 하지만 각각 View마다 다른 header와 footer를 구성해야한다면 @yield로 선언하여 각 자식 View들에서 
> 직접 @section으로 해당하는 파일을 @include 해온다던지의 수정이 필요할 것이다.
- {{ $변수 }}  
  Laravel의 Route나 Controller에서 전달해준 변수를 사용한다. {{ $변수 }} = \<?=$변수?> = <?php echo $변수; ?> 와 같다.
> 세부적으로는 다르다.

<br></br>
#### 자식 레이아웃 템플릿
***
- https://laravel.kr/docs/8.x/blade#extending-a-layout 에 쓰여있는 레이아웃 템플릿 예시를 약간 변형한 코드
```
<!-- Stored in resources/views/child.blade.php -->
<!-- resources/views/layouts/app.blade.php -->
@extends('layouts.app') 

@section('title', 'Page Title')

@section('sidebar')
    @parent

    <p>This is appended to the master sidebar.</p>
@endsection

@section('sidebar')
    @parent

    <p>This is appended to the master sidebar.</p>
@stop


```
- @extends("경로")  
  확장할 부모 레이아웃을 지정한다. 경로 지정 방식은 @include와 동일하다.
- @section("이름", "내용")  
  위의 @section처럼 @yield 이름 부분에 그대로 삽입되는건 동일하다. 이 형태는 @endsection이나 @stop이 존재하지 않고
그대로 "내용" 부분을 삽입한다.
- @section("이름") @parent ~ @endsection or @stop  
  @parent를 선언하면 부모에 존재하는 같은 @section을 덮어쓰지 않고 그 뒤에 붙여서 쓰게 된다.


### 데이터 삽입
***
```
@section('content')
    <p>This is my body content.</p>
@endsection

@{{ name }}
```
- {{ $php코드 }} : \<?php echo 코드; ?>와 동일하게 사용된다.
```
Route::get('greeting', function () {
    return view('welcome', ['name' => 'Samantha']); // name이 key값 Samanatha가 value인 관계배열 1개를 넘긴다. 
});
```
은 View에서 다음과 같이 사용한다.
```
Hello, {{ $name }}.
<!-- Hello, <?php echo $name;?> = Hello, <?=$name?> -->
```
PHP 코드를 그대로 삽입할 수 있기 때문에 다음과 같은 형태도 가능하다.
```
현재 시간은 {{ time() }}. 
<!-- 현재 시간은 <?php echo time();?> = 현재 시간은 <?=time()?> -->
```

### etc
***
- @{{}} or @Blade의 지시문   
  @를 붙인 다는 것은 Blade에서 해당 구문을 처리하지 말라는 뜻입니다. vue 같은 자바스크립트 언어에서 처리를 해주게 하기 위해 사용합니다.
- @verbatime ~ @endverbatim  
  위의 @{{}} 와 사용 용도는 동일합니다. Blade에서 해당 코드는 처리하지 않습니다.
- {{-- 주석 --}}  
  Blade의 주석입니다. html로 반환되지 않기 때문에 브라우저의 개발툴로 (ex: 크롬의 devtools) 볼 수 없습니다.
- once
  