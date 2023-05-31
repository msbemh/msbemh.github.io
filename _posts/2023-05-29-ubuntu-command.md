---
sidebar:
  nav: "docs"
title : Linux(Ubuntu) 사용법 및 명렁어
---

Ubuntu는 데비안 리눅스 기반의 오픈 소스 운영 체제입니다.    
우분투는 무료로 다운로드하고 사용할 수 있으며, 사용자 친화적인 인터페이스와 함께 안정적이고 신뢰성이 높은 운영 체제입니다.   
또한 개발자들이 많이 사용하는 운영 체제로, 다양한 개발 도구와 프로그래밍 언어를 지원합니다.    
우분투는 데스크톱, 서버, 클라우드, IoT 등 다양한 플랫폼에서 사용할 수 있습니다.    

---

**wget**   
웹 서버로부터 콘텐츠를 가져오는 컴퓨터 프로그램으로, GNU 프로젝트의 일부이다.   
World wide web + get   
HTTP, HTTPS, FTP 프로토콜을 통해 내려받기를 지원한다.

```
wget [옵션] [URL]
```

`ex) wget http://www.example.com`   

**<i class="fa fa-info-circle" aria-hidden="true"></i> 옵션의미**   
**-o** : 다운로드한 파일의 이름을 지정합니다.  
**-q** : 실행 중에 출력되는 메시지를 표시하지 않습니다.   
**-r** : 재귀적으로 다운로드합니다.   
**-np** : 부모 디렉토리를 탐색하지 않습니다.   
{: .notice--info}

**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**    
**tar (Tape Archive)** : 여러 파일을 하나의 아카이브 파일로 묶는 프로그램       
**gzip** : 압축을 수행하는 프로그램    
**tar.gz** : 여러 파일을 하나의 아카이브 파일로 묶고, gzip으로 압축한 형식. 일반적으로 UNIX 및 Linux 시스템에서 사용    
**tgz** : tar로 묶은 파일을 gzip으로 압축한 형식. 일반적으로 Windows에서 사용        
{: .notice--info}


---

**lynx**   
텍스트 모드 웹 브라우저입니다.
CLI에서 작동하며, HTML 문서, 이미지, 비디오 및 기타 미디어를 볼 수 있습니다. 

---

**gzip**   
gzip은 파일 압축을 위한 명령어입니다.   
주로 리눅스나 유닉스 기반의 운영체제에서 사용됩니다.   
gzip 명령어를 사용하면 파일을 압축하거나, 압축 해제할 수 있습니다.   


+ 파일 압축    

```
gzip [옵션] [파일명]
```

`ex) gzip test.txt`   


+ 파일 압축 풀기   

```
gzip -d [파일명]
```

`ex) gzip -d test.txt.gz`

---

**tar (Tape ARchive)**    
파일 압축과 아카이브(archive) 생성을 위해 사용됩니다.   
아카이브에서 파일을 추출에도 사용됩니다.   

아카이브는 압축이 아니라 단순히 파일과 디렉토리를 하나로 모으는 것

**<i class="fa fa-info-circle" aria-hidden="true"></i> 옵션의미**      
**-c** : 새 아카이브를 만듭니다.   
**-x** : 아카이브를 해제합니다.   
**-v** : 아카이브에 포함된 파일 및 디렉토리 목록을 출력합니다.     
**-z** : gzip으로 압축된 아카이브를 만듭니다.   
**-f** : 아카이브 파일 이름을 지정합니다. 이 옵션 다음에는 해제할 아카이브 파일의 이름이 나와야 합니다.      
{: .notice--info}

아카이브 파일 생성 : `tar -cvf 아카이브파일명.tar 파일1 파일2 디렉토리1 디렉토리2`   
아카이브 파일 압축(gzip) : `tar -czvf 아카이브파일명.tar.gz 파일1 파일2 디렉토리1 디렉토리2`   
아카이브 파일에서 파일 추출 : `tar -xvf 아카이브파일명.tar`   
아카이브 파일 압축 해제 : `tar -xzvf 아카이브파일명.tar.gz`   
현재 디렉토리의 모든 파일과 디렉토리를 아카이브로 묶기 : `tar -cvf archive.tar *`   

---

**configure**   
소스 코드를 빌드하기 전에 환경을 구성하는 명령어입니다.   
이 명령어는 보통 오픈 소스 소프트웨어를 빌드할 때 사용됩니다.      

```
./configure [OPTIONS]
```

```
./configure --prefix=/usr/local
```
**<i class="fa fa-info-circle" aria-hidden="true"></i> 옵션의미**      
**--prefix** : 소프트웨어를 설치할 위치를 지정하는 옵션입니다.   
{: .notice--info}

```
./configure --prefix=/usr/local/apache2 --with-apr=/usr/local/apr/bin/apr-1-config
```

**<i class="fa fa-info-circle" aria-hidden="true"></i> 옵션의미**      
**--with-apr** : Apache HTTP Server를 빌드할 때, `apr` 을 설치한 경로를 지정   
`--with-apr=<설치된 경로>`
{: .notice--info}


```
./configure --prefix=/usr/local/apache2 --with-apr=/usr/local/apr --with-apr-util=/usr/local/apr-util
```

**<i class="fa fa-info-circle" aria-hidden="true"></i> 옵션의미**      
**--with-apr-util** : Apache HTTP Server를 빌드할 때, `apr-util` 을 설치한 경로를 지정   
`--with-apr-util=<설치된 경로>`
{: .notice--info}

**<i class="fa fa-info-circle" aria-hidden="true"></i> 옵션의미**      
**--with-included-apr** : Apache HTTP Server를 빌드할 때 시스템에 설치된 APR 라이브러리가 아닌    
Apache HTTP Server 소스 코드에 포함된 APR 라이브러리를 사용하게 됩니다.     
난 안쓸거임😊
{: .notice--info}

---

**make**   
소스 코드 파일을 컴파일하고 실행 파일을 생성할 수 있습니다.     
Makefile이라는 특별한 파일에 작성된 규칙을 기반으로 작동합니다.   

```
make [options] [target]
```

예시)   
```s
[Makefile]
hello: helloc
    gcc -o hello hello.c
```

make 명령어 실행   
```
make hello
```
이렇게 실행하면, `hello.c` 파일이 컴파일 되고 `hello` 실행 파일이 생성됩니다.

---
**make install**   
소프트웨어 빌드 과정에서 생성된 실행 파일과 라이브러리, 설정 파일 등을 시스템에 설치하는 명령어입니다. 
일반적으로 소스 코드를 다운로드하고 컴파일하여 실행 파일을 생성한 후, make install 명령어를 사용하여 해당 파일을 시스템에 설치합니다.
소스 코드를 컴파일하여 설치하는 과정을 자동화하는 Makefile을 사용합니다. Makefile은 각 파일의 의존성과 명령어를 정의하여 빌드 과정을 자동화하는 스크립트 파일입니다.

---

**파일 및 디렉토리 소유자 및 권한 변경**   

+ **chown**   
파일이나 디렉토리의 소유자를 변경하는 데 사용됩니다.    

```
chown [옵션] [소유자:그룹] [파일명]
```

**<i class="fa fa-info-circle" aria-hidden="true"></i> 옵션의미**      
**-R** : 디렉토리 내의 모든 파일과 디렉토리의 권한을 재귀적으로 변경합니다.   
**-v** : 변경된 파일의 권한을 출력합니다.   
**-c** : 경된 파일의 권한이 출력될 때, 변경되지 않은 파일은 출력하지 않습니다.   
{: .notice--info}

예시) `chown root:root file.txt` 는 해당 파일의 소유자를 `root`로 그룹을 `root`로 변경합니다.

+ **chmod**   
파일이나 디렉토리의 권한을 변경하는 데 사용됩니다.   
읽기, 쓰기 및 실행 권한으로 구성됩니다.    
소유자, 그룹 및 기타 사용자에 대해 각각 설정됩니다.    

```
chmod [옵션] [모드] [파일명]
```

**<i class="fa fa-info-circle" aria-hidden="true"></i> 옵션의미**      
**-R** : 디렉토리 내의 모든 파일과 디렉토리의 권한을 재귀적으로 변경합니다.   
**-v** : 변경된 파일의 권한을 출력합니다.   
**-c** : 경된 파일의 권한이 출력될 때, 변경되지 않은 파일은 출력하지 않습니다.   
{: .notice--info}

**<i class="fa fa-info-circle" aria-hidden="true"></i> 숫자모드 의미**      
3자리 숫자로 권한을 표시합니다. 각 자리는 읽기, 쓰기, 실행 권한을 나타냅니다.    
예를 들어 `chmod 755 file.txt` 는 해당 파일에 소유자는 읽기, 쓰기, 실행 권한을 가지고,    
그룹과 다른 사용자는 읽기와 실행 권한을 가지게 합니다.   
{: .notice--info}

**<i class="fa fa-info-circle" aria-hidden="true"></i> 문자모드 의미**      
`u` : 소유자   
`g` : 그룹   
`o` : 다른 사용자   
`a` : 모든 사용자
`+` : 권한 추가
`-` : 권한 제거
`=` : 권한 설정
예를들어 `chmod u+x file.txt` 는 해당 파일에 소유자의 실행 권한을 추가합니다.
{: .notice--info}

---

**cmake**   
[CMake User Guide](https://cmake.org/cmake/help/latest/guide/user-interaction/index.html#guide:User%20Interaction%20Guide) 를 참고하세요.

크로스 플랫폼 빌드 시스템으로, C, C++, Java 및 Python과 같은 다양한 언어를 지원합니다.   
간단한 스크립트 언어를 사용하여 빌드 프로세스를 정의하며, 이를 통해 빌드 프로세스를 자동화할 수 있습니다.      

CMake를 사용하기 위해서는 먼저 CMakeLists.txt 파일을 작성해야 합니다. 이 파일은 빌드 프로세스를 정의하는 스크립트 파일로, CMakeLists.txt 파일을 작성한 후에는 CMake를 사용하여 빌드 프로세스를 자동화할 수 있습니다.   


CMake 프로젝트를 빌드하려면 먼저 해당 프로젝트의 소스 코드를 다운로드하고 압축을 해제해야 합니다.   

압축을 해제한 디렉토리에서 빌드 디렉토리를 생성합니다.   

이유 : 소스와 별도의 디렉터리에서 빌드하는 것이 좋습니다. 이렇게 하면 소스 디렉터리를 깨끗하게 유지하고, 여러 툴체인으로 단일 소스를 빌드할 수 있으며, 빌드 디렉터리를 삭제하기만 하면 빌드 아티팩트를 쉽게 지울 수 있습니다.

```
mkdir build
cd build
```

CMake를 사용하여 Makefile을 생성합니다.   

```
cmake ..
```

Makefile을 사용하여 프로젝트를 빌드합니다.



```
make
```

CMake 프로젝트를 빌드한 후에는 해당 프로젝트를 시스템에 설치할 수 있습니다. 이를 위해 다음 명령어를 사용합니다.

```
make install
```

---

**&**
&는 해당 명령어를 백그라운드에서 실행하도록 합니다. 

---



---