---
sidebar:
  nav: "docs"
title : MySQL 수동 설치
---

MySQL 수동 설치 해보자

---

**설치방법**   
[Installing MYSQL Using a Standard Source Distribution](https://dev.mysql.com/doc/refman/8.0/en/installing-source-distribution.html) 를 참고하세요.

[CMake User Guide](https://cmake.org/cmake/help/latest/guide/user-interaction/index.html#guide:User%20Interaction%20Guide) 를 참고하세요.

---

**사전준비**

+ boost 설치    

```s
wget https://boostorg.jfrog.io/artifactory/main/release/1.77.0/source/boost_1_77_0.tar.bz2
```

```s
tar -xvjf 파일이름.boost_1_77_0.tar.bz2
```

```s
tar -xvjf boost_1_77_0.tar.bz2
```

**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**   
**-j** : 파일이 `.bz2` 형식이라는 것을 나타냅니다.
{: .notice--info}

`cmake `, `make`, `build-essential`, `openssl`, `libboost-all-dev`, `libncurses6-dev`, `libncursesw6-dev` 필요

아래 명령어를 통해 패키지가 존재하는지 확인   
`dpkg -l | grep [이름]`   

**<i class="fa fa-info-circle" aria-hidden="true"></i> 요구사항 패키지 확인**   
```s
cmake : apt-get install cmake

make : apt-get install make (존재)

Linux: GCC 7.1 or Clang 5 : sudo apt-get install build-essential (gcc, g++ 존재)

SSL library : sudo apt-get install openssl (존재)

Boost C++ libraries : sudo apt-get install libboost-all-dev

ncurses library : sudo apt-get install libncurses5-dev libncursesw5-dev (존재)

Perl : apt-get install perl (존재)
```

+ cmake 설치

`cmake`만 없기 때문에 이것만 설치   
`Boost`는 위에서 수동 설치 했음

```s
cmake : apt-get install cmake
```

---

**MySQL설치**   

```s
wget https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-8.0.33.tar.gz
```

```s
gzip -d mysql-8.0.33.tar.gz
```

```s
tar xvf mysql-8.0.33.tar
```

```s
cd mysql-8.0.33
```

```s
mkdir build
cd build
```

```s
cmake \
.. \
-DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
-DMYSQL_DATADIR=/usr/local/mysql/data \
-DMYSQL_UNIX_ADDR=/usr/local/mysql/mysql.sock \
-DMYSQL_TCP_PORT=3306 \
-DDEFAULT_CHARSET=utf8 \
-DDEFAULT_COLLATION=utf8_general_ci \
-DSYSCONFDIR=/etc \
-DWITH_EXTRA_CHARSETS=all \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_ARCHIVE_STORAGE_ENGINE=1 \
-DWITH_BLACKHOLE_STORAGE_ENGINE=1 \
-DDOWNLOAD_BOOST=1 \
-DWITH_SSL=system \
-DWITH_BOOST=/home/song_ubuntu_22_04/boost_1_77_0
```

**<i class="fa fa-info-circle" aria-hidden="true"></i> CMake MySQL 옵션 정보**   
```s
-DCMAKE_INSTALL_PREFIX=path: MySQL이 설치될 경로를 지정합니다.

-DMYSQL_DATADIR=path: MySQL 데이터 디렉토리의 경로를 지정합니다.

-DSYSCONFDIR=path: MySQL 설정 파일의 경로를 지정합니다.   

-DMYSQL_UNIX_ADDR=path: MySQL 서버가 실행 중인 Unix 소켓 파일의 경로를 지정합니다.   

-DMYSQL_TCP_PORT=<port_number>: MySQL 서버와 통신하기 위해 사용할 TCP 포트를 설정하는 데 사용됩니다.   

-DDEFAULT_CHARSET=<문자집합>: CMake 프로젝트에서 사용할 기본 문자 인코딩을 설정하는 데 사용됩니다. 

-DDEFAULT_COLLATION=<문자정렬>: 데이터베이스에서 문자열 비교를 수행할 때 사용되는 기본 정렬 규칙을 지정합니다.   

-DWITH_EXTRA_CHARSETS=<문자집합>: MySQL 데이터베이스 서버를 빌드할 때 추가 문자 집합을 사용할 수 있도록 합니다.   

-DOWNLOAD_BOOST=1: CMake가 Boost 라이브러리를 다운로드하도록 지시하는 옵션입니다. 

-DWITH_BOOST=path: CMake가 Boost 라이브러리를 빌드할 때 해당 라이브러리를 사용할 수 있도록 지정할 수 있습니다

-DWITH_INNOBASE_STORAGE_ENGINE=1: InnoDB 스토리지 엔진을 빌드합니다.

-DWITH_MYISAM_STORAGE_ENGINE=1: MyISAM 스토리지 엔진을 빌드합니다.

-DWITH_ARCHIVE_STORAGE_ENGINE=1: Archive 스토리지 엔진을 빌드합니다.

-DWITH_BLACKHOLE_STORAGE_ENGINE=1: Blackhole 스토리지 엔진을 빌드합니다.

-DWITH_PARTITION_STORAGE_ENGINE=1: Partition 스토리지 엔진을 빌드합니다.

-DWITH_SSL=system: 시스템 SSL 라이브러리를 사용합니다.

-DWITH_ZLIB=bundled: MySQL에 내장된 zlib 라이브러리를 사용합니다.

-DWITH_LIBWRAP=0: TCP Wrappers를 사용하지 않습니다.

-DENABLED_LOCAL_INFILE=1: 로컬 파일의 로딩을 허용합니다.
```

**<i class="fa fa-info-circle" aria-hidden="true"></i> Unix 소켓 파일 정보**   
```s
Unix 소켓 파일은 Unix 계열 운영체제에서 사용되는 통신 방식 중 하나입니다. 이 방식은 프로세스간 통신(IPC)을 위해 사용됩니다.    
소켓 파일은 파일 시스템에 존재하지만, 일반 파일과는 다릅니다.    
일반 파일은 파일 내용을 읽고 쓸 수 있지만, 소켓 파일은 소켓을 통해 데이터를 주고 받을 수 있습니다.    

소켓 파일은 서버와 클라이언트 간의 통신에 사용됩니다.    
서버는 소켓 파일을 생성하고, 클라이언트는 이 파일을 통해 서버에 접속합니다.    
이를 통해 두 프로세스 간의 데이터 전송이 가능해집니다.   
```

```s
make
```

```s
make install
```

---

**MYSQL 사용자 추가**    

사용자 그룹 및 사용자 추가

```s
groupadd mysql
useradd -r -g mysql -s /bin/false mysql
```

`useradd [OPTIONS] USERNAME`   

**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**   
-r : system account   
-g : 그룹 지정   
-s: user의 로그인 shell   
/bin/false : 로 해당 user로 로그인하는 것을 막는다.   
{: .notice--info}   

생성된 사용자 확인    
```
cat /etc/passwd 
```

---

**MySQL 설정 및 실행**

```s
cd /usr/local/mysql
```

```s
mkdir mysql-files
```

```s
chown mysql:mysql mysql-files
```

```s
chmod 750 mysql-files
```

MySQL 데이터베이스 서버를 초기화
```s
bin/mysqld --initialize --user=mysql
```
**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**   
--initialize : 데이터베이스 디렉토리를 초기화   
--user=mysql : MySQL 서버 프로세스가 mysql 사용자로 실행되도록 지정
{: .notice--info}
   
```s
bin/mysql_ssl_rsa_setup
```
**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**   
MySQL 서버와 클라이언트 간의 통신이 암호화되고 보안이 강화됩니다.    
{: .notice--info}

MySQL 서버 실행   
`sudo /etc/init.d/mysqld start`    
`sudo /etc/init.d/mysqld status`    
`sudo /etc/init.d/mysqld restart`  

---

**MYSQL 서버 실행 중인지 확인**   

```
ps -ef | grep mysqld
```

---

**MYSQL 서버 실행이 안될때**    
`/usr/local/mysql/data` 경로에서 `*****.err` 파일을 봐보자.   
이곳에서 로그가 `mysqld.sock`을 읽다가 중단 되는 느낌이였음.  
MYSQL이 실행 될때 생성 되는 `mysql.sock`이 현재 봐라보고 있는 곳과 다른곳에서 생성 되고 있었음.    

CMake 설정에 `-DMYSQL_UNIX_ADDR=/usr/local/mysql/mysql.sock` 로 했기 때문에    
`mysql.sock`이 `/usr/local/mysql` 에 생성이 됨. (못찾아서 헤맸음 😂)  

MYSQL 설정파일 `/etc/mysql/conf.d/mysql.cnf` 에서
`socket=/usr/local/mysql/mysql.sock` 로 socket 경로를 바꿔주자   

그럼 이제 잘 실행이 될거다 👍

---   

**사용자 패스워드 변경**   
`mysql -u root -p` 로 로그인   
처음 설치 했을 때 임시 비밀번호를 이용한다.

`alter user 'root'@'localhost' identified by '원하는 비밀번호';` 로 비밀번호 변경   

---

**부팅 시 자동으로 실행 설정**   

```s
cp support-files/mysql.server /etc/init.d/mysql.server
```
**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**   
해당 명령어는 support-files 디렉토리 안에 위치한 mysql.server 파일을 
/etc/init.d/ 디렉토리로 복사하는 것을 의미합니다.    
이 명령어는 MySQL 데이터베이스 서버를 설치한 후에 서버를 실행하기 위해 사용됩니다.   
/etc/init.d/ 디렉토리에 위치한 스크립트 파일은 시스템 부팅 시 자동으로 실행되는데,    
이 경우 /etc/init.d/mysql.server 파일은 MySQL 서버를 자동으로 시작하거나 중지하는 데 사용됩니다. 
{: .notice--info}

---

**php mysql 연동**   

```s
sudo apt-get install php-mysql
```

php.ini 파일에 아래 2줄의 주석을 풀어줍니다.   
```s
extension=mysqli
extension=pdo_mysql
```

그리고 Apache 서버 재시작   
```s
apachectl restart
```

**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**   
`extension=pdo_mysql`는 PHP Data Objects(PDO) 인터페이스를 사용하여 MySQL 데이터베이스와 상호 작용하는 데 사용됩니다.  
이 확장(extension)은 일반적으로 MySQL 데이터베이스를 사용하는 PHP 애플리케이션에서 데이터베이스 연결과 쿼리 실행에 사용됩니다.    
PDO는 여러 종류의 데이터베이스에 대한 일관된 인터페이스를 제공하기 때문에, MySQL 외에도 다른 데이터베이스 시스템과 상호 작용하는 데 사용할 수 있습니다.   
`    
반면에 `extension=mysqli`는 MySQL 데이터베이스와 직접 상호 작용하기 위한 MySQLi 확장(extension)입니다.    
이 확장(extension)은 이전 버전의 MySQL 확장(extension)보다 개선된 기능을 제공하며, 객체 지향 프로그래밍 방식으로 작성되었습니다.    
`     
이 확장(extension)은 MySQL 데이터베이스에 대한 더 나은 보안 및 성능을 제공합니다.   
`    
따라서, `extension=pdo_mysql`은 일반적인 데이터베이스 인터페이스를 제공하고,    
`extension=mysqli`는 MySQL 데이터베이스와 직접 상호 작용하기 위한 인터페이스를 제공합니다.    
어떤 확장(extension)을 사용할지는 애플리케이션의 요구 사항과 개발자의 선호도에 따라 다를 수 있습니다.
{: .notice--info}


PHP 연동이 잘 되는지 확인해 봅시다.
```php
<?php
    echo "MySQL 연결 테스트<br>";

    error_reporting(E_ALL);
    
    ini_set("display_errors", 1);

    if (!function_exists('mysqli_init') && !extension_loaded('mysqli')) {
        echo 'MySQLi is not installed!';
    } else {
        echo 'MySQLi is installed!';
    }


    $conn = mysqli_connect('127.0.0.1', 'root', 'AlshalshSjrnf92@');
    if (!$conn) {
        die('Could not connect to MySQL: ' . mysqli_connect_error());
    }
    echo 'Connected successfully';
    mysqli_close($conn);
?>
```
를 이용해서 확인!😎

---     

