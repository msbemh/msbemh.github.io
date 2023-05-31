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

```s
bin/mysqld --initialize --user=mysql
```

```s
bin/mysql_ssl_rsa_setup
```

```s
bin/mysqld_safe --user=mysql &
```

```s
cp support-files/mysql.server /etc/init.d/mysql.server
```

**<i class="fa fa-info-circle" aria-hidden="true"></i> MySQL 실행 명령어 정보**   
```s
bin/mysqld --initialize --user=song  
MySQL 데이터베이스 서버를 초기화하는 명령어입니다.     
--initialize 옵션은 데이터베이스 디렉토리를 초기화하고     
--user=mysql 옵션은 MySQL 서버 프로세스가 mysql 사용자로 실행되도록 지정합니다.     
        
bin/mysql_ssl_rsa_setup     
MySQL 데이터베이스에서 SSL/TLS 인증서를 생성하는 스크립트입니다.      
이 스크립트는 OpenSSL을 사용하여 RSA 키 쌍과 X.509 인증서를 생성하고,      
MySQL 서버에 필요한 파일을 생성합니다.      
이를 통해 MySQL 서버와 클라이언트 간의 통신이 암호화되고 보안이 강화됩니다.     
        
bin/mysqld_safe --user=song &     
MySQL 서버 실행     
mysql 사용자로 MySQL 서버를 실행합니다.     
&는 해당 명령어를 백그라운드에서 실행하도록 합니다.     
   
cp support-files/mysql.server /etc/init.d/mysql.server   
해당 명령어는 support-files 디렉토리 안에 위치한 mysql.server 파일을 
/etc/init.d/ 디렉토리로 복사하는 것을 의미합니다.    
이 명령어는 MySQL 데이터베이스 서버를 설치한 후에 서버를 실행하기 위해 사용됩니다.   
/etc/init.d/ 디렉토리에 위치한 스크립트 파일은 시스템 부팅 시 자동으로 실행되는데,    
이 경우 /etc/init.d/mysql.server 파일은 MySQL 서버를 자동으로 시작하거나 중지하는 데 사용됩니다.   
```
     
---     