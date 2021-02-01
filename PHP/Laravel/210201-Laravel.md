# 라라벨 8 설치
- 참고 - https://laravel.kr/docs/8.x/installation

## 기본 요구사항
- PHP >= 7.3
- BCMath PHP Extension (PHP를 컴파일 할 때 옵션으로 enable 해야함. 윈도우 버전은 추가 작업 필요 없음.)
- Ctype PHP Extension (PHP를 컴파일 할 때 기본으로 들어감. 윈도우 버전은 추가 작업 필요 없음.)
- Fileinfo PHP extension (extension=fileinfo)
- JSON PHP Extension (PHP 5.2.0 이상 버전에선 자동으로 bundle 되어 있음)
- Mbstring PHP Extension (extension=mbstring)
- OpenSSL PHP Extension (extension=openssl)
- PDO PHP Extension (php 5.3 이상에선 설정할 필요 없음)
- Tokenizer PHP Extension (PHP를 컴파일 할 때 기본으로 들어감. 윈도우 버전은 추가 작업 필요 없음.)
- XML PHP Extension (PHP를 컴파일 할 때 기본으로 들어감. 윈도우 버전은 추가 작업 필요 없음.)

php.ini 에서 fileinfo, mbstring, openssl을 활성화 시키자.

## 컴포저로 설치하기
- Composer - https://getcomposer.org/download/

#### 라라벨 인스롤러로 설치    
```
composer global require laravel/installer
```

- macOS: $HOME/.composer/vendor/bin
- Windows: %USERPROFILE%\AppData\Roaming\Composer\vendor\bin
- GNU / Linux Distributions: $HOME/.config/composer/vendor/bin or $HOME/.composer/vendor/bin

해당 위치에 설치되었는지 확인.

프로젝트 생성
```
laravel new 프로젝트이름
```

#### Composer Create-Project로 설치
```
composer create-project --prefer-dist laravel/laravel 프로젝트이름
```

