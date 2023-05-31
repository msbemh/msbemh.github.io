---
sidebar:
  nav: "docs"
title : 아파치 수동 설치
---

아파치(Apache)는 오픈 소스 웹 서버 소프트웨어입니다.    
아파치를 수동으로 설치하면 **소스 코드를 직접 다운로드**하여 **컴파일**하고 **설정 파일을 구성**해야 합니다.    
이 방법은 보다 세부적인 제어와 구성이 필요한 경우에 유용합니다.   

---

**설치방법**   
[아파치 설치 가이드 홈페이지](https://httpd.apache.org/docs/2.4/en/install.html) 를 참고하세요.

---

**C 컴파일러 gcc 설치**
```
sudo apt-get install build-essential
```

---

**libexpat-dev 설치**   
libexpat-dev는 C 언어로 작성된 XML 파서 라이브러리인 Expat의 개발용 헤더 파일과 정적 라이브러리를 포함하는 패키지입니다.
```
sudo apt-get install libexpat-dev
```

---

**APR (Apache Portable Runtime)**   
Apache 웹 서버와 다른 프로그램들이 사용하는 라이브러리   
APR을 설치하면 Apache 웹 서버와 연동되는 다양한 프로그램을 더욱 쉽게 개발할 수 있습니다.    

```
wget https://dlcdn.apache.org//apr/apr-1.7.4.tar.gz
```
```
gzip -d apr-1.7.4.tar.gz
```

`apr-1.7.4.tar.gz` => `apr-1.7.4.tar` 로 변함

```
tar xvf apr-1.7.4.tar
```
```
cd apr-1.7.4
```
```
./configure --prefix=/usr/local/apr
```
```
make
```
```
make install
```

---

**PCRE(Perl Compatible Regular Expressions)**  
   
Perl 정규 표현식(Regular Expression)을 기반으로 하는 정규 표현식 엔진입니다
다양한 문자열 처리 작업에서 유용하게 사용됩니다. 
아파치 웹 서버에서 URL 리다이렉션, 모디파이드 리퀘스트, 경로 매칭 등에 사용됩니다.   
PCRE은 다양한 프로그래밍 언어에서도 사용 가능하며, C 언어로 작성되어 있습니다.   

```
wget https://github.com/PCRE2Project/pcre2/releases/download/pcre2-10.42/pcre2-10.42.tar.gz
```
```
gzip -d pcre2-10.42.tar.gz
```
```
tar xvf pcre2-10.42.tar
```
```
cd pcre2-10.42
```
```
./configure --prefix=/usr/local/pcre
```
```
make
```
```
make install
```

---

**APR-util (Apache Portable Runtime - util)**    
Apache Portable Runtime (APR) 라이브러리를 보완하기 위해 개발된 라이브러리입니다.         
APR-util은 다양한 데이터베이스 시스템 (Oracle, MySQL, PostgreSQL 등)과의 상호 작용을 가능하게 해주는    
데이터베이스 라이브러리, XML 파서, URI 파서 등의 기능을 제공합니다.   
또한, 인증과 암호화 기능을 포함한 여러 유틸리티 함수를제공합니다.   
이러한 기능들은 아파치 HTTP 서버와 같은 웹 서버에서 사용됩니다.   
APR-util은 C 언어로 작성되었으며, 아파치HTTP 서버와 함께 사용될 때 높은 성능을 제공합니다.     

```
wget https://dlcdn.apache.org//apr/apr-util-1.6.3.tar.gz
```
```
gzip -d apr-util-1.6.3.tar.gz
```
```
tar xvf apr-util-1.6.3.tar
```
```
cd apr-util-1.6.3
```
```
./configure --prefix=/usr/local/apr-util --with-apr=/usr/local/apr
```
```
make
```
```
make install
```

---

**Apache 설치**

```
wget https://dlcdn.apache.org/httpd/httpd-2.4.57.tar.gz
```

```
gzip -d httpd-2.4.57.tar.gz
```

```
tar xvf httpd-2.4.57.tar   
```

```
cd httpd-2.4.57
```

```
./configure --prefix=/usr/local/apache2 --with-apr=/usr/local/apr --with-apr-util=/usr/local/apr-util --with-pcre=/usr/local/pcre/bin/pcre2-config
```

아파치에서 `--prefix`를 지정하지 않으면 기본값으로 /usr/local/apache2 를 사용합니다.   

```
make
```
```
make install
```

--- 

**Apache 실행 & 중지**    
이동

```
cd /usr/local/apache2/bin
```

시작
```
./apachectl start
```

중지
```
./apachectl stop
```

--- 

**아파치 환경변수 등록**     

 + 현재 세션에서만 유효함   

```
export PATH=$PATH:/usr/local/apache2/bin
```   
   
 + 영구적으로 등록하기   

```
sudo nano ~/.bashrc
```
위 명령어로 `bashrc` 파일을 편집할 수 있습니다.   
`export PATH=$PATH:/usr/local/apache2/bin` 명령어를 맨 아래줄에 추가 합니다

```
source ~/.bashrc
```
위 명령어로 환경변수를 적용합니다.

이제 어느곳에서도 `apachectrl` 명령어를 실행 시킬 수 있습니다.

---

**아파치 설정 변경**

```
vi /usr/local/apache2/conf/httpd.conf
```
아파치 PORT 번호를 변경 한다던가 설정을 변경할 수 있습니다.










