---
sidebar:
  nav: "docs"
title : PHP Remote 디버그 (xdebug)
---
Xdebug는 PHP용 확장 기능으로, <br/> 'PHP' 개발 환경을 개선하기 위한 다양한 기능을 제공합니다.

***

**기능: Step Debugging**    
스크립트가 실행되는 동안 IDE 또는 편집기에서 코드를 단계별로 살펴볼 수 있는 방법입니다.
Step Debugging 기능에서 IDE와 ***DBGp 프로토콜***로 통신 합니다.

**<i class="fa fa-info-circle" aria-hidden="true"></i> Info:**
DBGp 디버깅 프로토콜(Debugging Protocol)중 하나.
디버깅을 위한 표준 프로토콜
{: .notice--info}

---
**DBGp 통신 그림**

<img src="{{ site.baseurl }}/assets/images/xdebug 통신 이미지.png">
---

**php 구성정보 확인** 

phpinfo() 함수를 실행시켜 현재 PHP 구성정보를 확인합니다.

```php
<?php
    phpinfo();
?>
```
    
`   

PHP 구성정보 화면이 나타나면, 전체 화면을 복사 합니다.
<img src="{{ site.baseurl }}/assets/images/phpinfo 이미지.png">

---
**Xdebug Wizard 사이트**

<a href="https://xdebug.org/wizard">https://xdebug.org/wizard</a> 사이트로 이동합니다.


복사한 phpinfo 정보를 양식에 붙여넣고, "Analyes my phpinfo() output" 버튼을 클릭하면
Xdebug 설치 방법을 알려줍니다.

해당 사이트에서 알려준 방법대로 진행 하면 Server에 xdebug를 설치할 수 있습니다.

+ xdebug-3.x.x.tgz 다운로드 (wizard 사이트에서 다운)
+ PHP Extension 컴파일 하기 위한 요구사항들을 설치

```
{% raw %}apt-get install php-dev autoconf automake{% endraw %}
```

**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**   
**php-dev** : PHP 개발에 필요한 다양한 라이브러리와 도구들을 제공     
**autoconf** : 소프트웨어 빌드 시켜주는 스크립트를 자동 생성해주는 도구.   
configure 라는 스크립트를 생성시켜 줍니다.    
`   
**automake** : 소프트웨어 빌드 프로세스를 자동화하기 위한 툴입니다.  
소스코드를 컴파일 가능한 상태로 만들기 위해 Makefile 생성시켜 줍니다.    
`    
**Makefile** : 소스 코드 파일과 다른 자원들을 컴파일 가능한 상태로 만드는 방법을 정의합니다.
{: .notice--info}

+ 압축풀기   

```
{% raw %}tar -xvzf xdebug-3.2.1.tgz{% endraw %}
```

**<i class="fa fa-info-circle" aria-hidden="true"></i> 옵션의미**   
**-x** : 아카이브 파일을 해제(decompress)합니다.   
**-v** : 처리되는 파일들의 이름을 자세히(verbose) 출력합니다.   
**-z** : gzip으로 압축된 아카이브 파일을 해제합니다.     
**-f** : 파일 이름을 지정합니다. 이 옵션 다음에는 해제할 아카이브 파일의 이름이 나와야 합니다.   
{: .notice--info}

**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**   
**tar** : 여러 파일을 하나의 아카이브 파일로 묶는 프로그램   
**gzip** : 압축을 수행하는 프로그램   
**tgz** : tar로 묶은 파일을 gzip으로 압축한 형식        
{: .notice--info}

+ 이동   

```
{% raw %}cd xdebug-3.2.1{% endraw %}
```
    
`    

+ phpize 실행  

```
{% raw %}phpize{% endraw %}
```

**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**   
**phpize** : configure, make, make install 등의 명령어를 실행하기 전에 필요한 파일들을 생성합니다.
{: .notice--info}

+ ./configure 실행

```
{% raw %}./configure{% endraw %}
```

**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**   
**configure** : 소스 코드를 컴파일하고 설치하기 위한 설정을 구성합니다.
{: .notice--info}

+ make 실행   

```
{% raw %}./make{% endraw %}
```

**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**   
**make** : 소스 코드를 컴파일합니다.
{: .notice--info}

+ 라이브러리 복사

```
{% raw %}cp modules/xdebug.so /usr/lib/php/20210902{% endraw %}
```

**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**   
**cp** : 파일이나 디렉토리를 복사하는 명령어   
cp [옵션] 소스파일 대상위치
{: .notice--info}

+ php.ini 파일 찾기

```
{% raw %}find / -name 'php.ini'{% endraw %}
```

검색 결과 2개의 파일이 나옴  
/etc/php/8.1/apache2/php.ini   
/etc/php/8.1/cli/php.ini   

+ 2개의 php.ini 파일에 code 추가

```ini
{% raw %}
//For Xdebug v3.x.x:
[XDEBUG]
zend_extension = xdebug

xdebug.mode = debug
xdebug.start_with_request = yes{% endraw %}

//For Xdebug v2.x.x:
[XDEBUG]
xdebug.remote_enable = 1
xdebug.remote_autostart = 1
xdebug.remote_port = 9000
```

저는 v3.2.1를 다운받았기 때문에, 위쪽 code만 추가 했습니다.

**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**   
버전이 v2 => v3로 바뀌면서, 기본 포트도 9000 => 9003 으로 변경 됐습니다.
{: .notice--info}

**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**   
**start_with_request** : 웹 서버가 클라이언트의 요청을 받을 때마다 Xdebug가 자동으로 활성화됩니다. 
디버깅이 필요한 경우에만 디버깅을 할 수 있습니다. 이를 통해 디버깅을 위한 오버헤드를 줄일 수 있습니다.
{: .notice--info}

+ PHP 재시작 (서버 재시작)

```
{% raw %}sudo /etc/init.d/apache2 restart{% endraw %}
```

**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**   
**/etc 폴더** : etc(et cetera) 라틴어로 "그 밖에 더 많은 것" 의미   
시스템 설정 파일들이 저장되는 폴더.   
예를 들어 네트워크 설정, 사용자 계정, 서비스 설정 등이 포함됩니다.    
`   
**/etc/init.d 폴더** : 리눅스 시스템에서 서비스를 시작하고 중지하는 데 사용되는 스크립트 파일을 포함하는 곳.   
이 디렉토리의 스크립트 파일은 시스템 부팅시 자동으로 실행된다.
수동으로 실행할 수도 있다.
{: .notice--info}

---
**Server Xdebug 적용 확인**

```php
{% raw %}<?php 
    xdebug_info();
?>{% endraw %}
```

<img src="{{ site.baseurl }}/assets/images/xdebug_info 이미지.png">

이렇게 잘 나오면 Server 쪽 xdebug는 적용이 잘 된겁니다.

---

**Client Xdebug 설치**

<img src="{{ site.baseurl }}/assets/images/vscode xdebug plugin.png">

---

**VSCode launch파일 생성**

<img src="{{ site.baseurl }}/assets/images/xdebug vscode launch 파일.png">

port는 기본(9003)으로 설정했고,    
pathMappings으로 server와 local의 파일경로를 매핑 시켰습니다.

---
**디버깅 동작 모습**

<img src="{{ site.baseurl }}/assets/images/xdebug 디버깅 모습.png">

잘된다~ 오예~


