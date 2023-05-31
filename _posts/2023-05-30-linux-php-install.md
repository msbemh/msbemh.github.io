---
sidebar:
  nav: "docs"
title : PHP 수동 설치 (Linux)
---

Linux (Ubuntu) 환경에서 PHP 수동 설치를 해보자 😎

---

**설치방법**   
[아파치 설치 가이드 홈페이지](https://httpd.apache.org/docs/2.4/en/install.html) 를 참고하세요.

---

**필요한 종속성을 먼저 설치**
```
sudo apt-get install build-essential libxml2-dev libssl-dev libbz2-dev libjpeg-dev libpng-dev libmcrypt-dev libxslt1-dev
```

**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**    
**build-essential** : 리눅스 환경에서 소프트웨어를 컴파일(Compile) 하기 위해 필요한 기본적인 빌드 도구 패키지입니다.    
이 패키지는 C/C++ 컴파일러, 빌드 도구(gcc, g++, make 등) 및 관련 라이브러리 등을 포함합니다.     
**libxml2-dev** :   XML 파서 라이브러리인 libxml2를 사용하여 XML 문서를 처리하는 C/C++ 프로그램을 개발할 때 필요한 개발용 라이브러리입니다.   
**libssl-dev** : OpenSSL 라이브러리를 사용하여 SSL 및 TLS 프로토콜을 구현하는 개발자를 위한 라이브러리입니다.    
**libbz2-dev** : Bzip2 압축 라이브러리를 사용하여 데이터를 압축하거나 압축을 해제할 수 있는 프로그램을 개발할 때 필요한 개발 라이브러리입니다.     
**libjpeg-dev** : JPEG 이미지 파일을 생성하고 처리하기 위한 C/C++ 라이브러리인 libjpeg의 개발 버전입니다.    
**libpng-dev** :  PNG 이미지 파일 형식을 처리하기 위한 라이브러리인 libpng의 개발용 헤더 파일과 라이브러리를 포함하는 패키지입니다.     
**libmcrypt-dev** : 암호화 라이브러리인 libmcrypt를 사용하여 애플리케이션을 개발하기 위한 개발 헤더 파일과 라이브러리를 포함하는 패키지입니다.    
**libxslt1-dev** : XSLT (eXtensible Stylesheet Language Transformations)를 사용하여 XML 문서를 변환하는 C 라이브러리인 libxslt의 개발 버전입니다.    
{: .notice--info}


```
sudo apt-get install pkg-config
```

```
sudo apt-get install sqlite3 libsqlite3-dev
```

```
sudo apt-get install libcurl4-openssl-dev
```

```
sudo apt-get install libreadline-dev
```



---

**PHP설치**   

```
wget https://www.php.net/distributions/php-8.2.6.tar.gz
```

```
gzip -d php-8.2.6.tar.gz
```

```
tar xvf php-8.2.6.tar
```

```
cd php-8.2.6
```

```
./configure --prefix=/usr/local/php --with-apxs2=/usr/local/apache2/bin/apxs --with-config-file-path=/etc/php --with-config-file-scan-dir=/etc/php/conf.d --with-curl --with-openssl --with-zlib --with-gd --with-jpeg --with-png --with-mcrypt --with-readline --with-libxml --with-mysqli --with-pdo-mysql
```
**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**    
**--prefix** : PHP를 설치할 디렉토리를 지정합니다.   
이 패키지는 C/C++ 컴파일러, 빌드 도구(gcc, g++, make 등) 및 관련 라이브러리 등을 포함합니다.     
**--with-apxs2** : Apache 웹 서버와 PHP를 연동하기 위한 모듈을 지정합니다.   
**--with-config-file-path** : PHP 설정 파일을 저장할 디렉토리를 지정합니다.   
**--with-config-file-scan-dir** : 추가적인 PHP 설정 파일을 저장할 디렉토리를 지정합니다.   
**`--with-curl`, `--with-openssl`, `--with-zlib`, `--with-gd`, `--with-jpeg`, `--with-png`, `--with-mcrypt`, `--with-readline`, `--with-libxml`, `--with-mysqli`, `--with-pdo-mysql`** :  PHP에서 사용할 라이브러리와 모듈을 지정합니다.   
{: .notice--info}


```
make
```

```
make install
```

---

**PHP설치 확인**   

```
cd /usr/local/apache2/modules
ls -al
```

libphp.so 가 설치된 것을 확인

```s
-rwxrwxrwx  1 root root    17306 May 30 14:21 httpd.exp
-rwxrwxrwx  1 root root 42016320 May 30 17:46 libphp.so
-rwxrwxrwx  1 root root    39816 May 30 14:26 mod_access_compat.so
-rwxrwxrwx  1 root root    37656 May 30 14:26 mod_actions.so
```

---
**PHP 와 Apache 연동**  

```
vi /usr/local/apache2/conf/httpd.conf
```

Apache 설정 내부에

```s
LoadModule php_module         modules/libphp.so
```

이렇게 아까 확인한 `libphp.so` 모듈을 로드하고 있는 것을 확인할 수 있음.   

+ Apache에서 읽어들일 확장자를 지정합니다.
AddType이 있는 라인을 찾아가서   
`AddType application/x-httpd-php .php .html` 를 추가    

```s
AddType application/x-compress .Z
AddType application/x-gzip .gz .tgz
AddType application/x-httpd-php .php .html
```

Apache Document Root 경로(`/usr/local/apache2/htdocs`)로 이동하고    
index.php파일을 생성하고   
아래와 같이 작성합니다.   

```php
<?php 
  phpinfo();
?>
```

브라우저 url에 `localhost:80/phpindex.php` 입력해서 php 정보 화면이 뜨는지 확인     

저는 wget로 다운 받고 압축을 푼 장소가 `/home/사용자명` 입니다.   
php.ini 파일 인식경로(`Configuration File (php.ini) Path`) 는 `/etc/php` 입니다.   

```
cp  /home/사용자명/php-8.2.6/php.ini-production /etc/php/php.ini
```
위 명령어로 `php.ini` 파일을 `/etc/php/php.ini` 경로로 복사해 줬습니다.

테스트 삼아서 php.ini 설정을 바꾸고 phpinfo()에 반영 되는지 테스트 해보세요 😊


그러면 끝입니다 😊👍


