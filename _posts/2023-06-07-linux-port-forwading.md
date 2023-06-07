---
sidebar:
  nav: "docs"
title : 포트포워딩
---
VMWare (Ubuntu) 네트워크 포트포워딩 설정

***

**Ubuntu 설치시 네트워크 설정**    
네트워크 설정을 Default 설정으로 선택   
DHCP를 이용해서 동적으로 인터넷 설정   
apt를 이용하여 필요한 패키지들을 다운 받기 위해서   


**<i class="fa fa-info-circle" aria-hidden="true"></i> 정보**
Ctrl + Alt + F1 ~ F6 : CLI <=> GUI 변경
{: .notice--info}

---

**고정 IP 할당**

```s
sudo apt update
```

```s
sudo apt install net-tools
```

네트워크 정보 확인(서버 Linux)
```s
iconfig
```

네트워크 정보 확인 (로컬 Windows cmd)
```s
ipconfig
```

`Edit`>`Virtual Network Editor` 클릭   
`Chnage Settings` 클릭   
`VMnet8` 선택   
`NAT Settings` 클릭    
Gateway IP 확인 > 뒤로가기    
`DHCP Settings` 클릭   
DHCP 범위 확인
ex) 
Starting IP address : 192.168.137.128   
Ending IP address : 192.168.137.254      


Ubuntu 네트워크 정보 확인 결과
```s
IPv4 : *.*.*.*
Subnet Mask : *.*.*.*
브로드 캐스트 : *.*.*.255
네트워크 어댑터 인터페이스 : ens33
```

```s
/etc/netplan
```

```s
nano 01-network-manager-all.yaml
```

```s
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    ens33: --네트워크 어댑터 인터페이스 명
      dhcp4: no
      addresses:
       - 192.168.137.128/24  --고정할 IP 및 서브마스크
      routes:
       - to: default
         via: 192.168.137.2  --게이트웨이
      nameservers:
        addresses: [192.168.137.2]  --게이트웨이를 적자
```

네트워크 설정 반영
```s
sudo netplan apply
```

ip 변경 됐는지 확인
```s
ifconfig
```

---