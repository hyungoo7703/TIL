# 리눅스 활용

## 1. X 윈도
X 윈도우는 유닉스/리눅스 계열 운영체제에서 사용되는 플랫폼 독립적인 그래픽 사용자 인터페이스(GUI) 시스템

### 주요 구성 요소

+ **서버/클라이언트**
  + 로컬 통신을 위해 /tmp/.X11-unix/X0 유닉스 도메인 소켓을 사용하고, 원격 통신을 위해 TCP 포트 6000번을 사용
+ **X 프로토콜**
  + X 서버와 클라이언트 간의 통신을 위한 메시지 형식을 정의한 통신 규약 
+ **Xlib**
  + C 언어로 작성된 X 윈도우 시스템 프로토콜 클라이언트 라이브러리
  + 윈도우 생성, 키보드 처리 등과 같은 기본적인 라이브러리를 제공
  + 저수준 인터페이스이기 때문에 복잡한 GUI 구현에는 한계가 있어 Xtoolkit이 필요
+ **Xtoolkit**
  + Xlib의 효율성 문제를 해결하기 위해 고급 레벨의 GUI 생성에 사용 
  + 주요 라이브러리로는 Xt Intrinsics, Xaw, XView, Motif, Qt, GTK+ 등이 있다.

> #### X 서버
> > XFree86: 1991년에 386 PC에 X를 적용하기 위해 시작된 프로젝트로, Intel x86 계열 시스템을 위한 X 서버 <br>
> > X.Org: X.Org와 freedesktop.org 구성원들이 XFree86 코드를 포크하여 더 개방적인 개발 모델로 전환

> #### X 프로토콜의 주요 명령어들의 실제 사용 예시
> > xauth (인증 키 관리)
```
# 현재 인증 키 목록 확인
xauth list

# 인증 키 추출
xauth extract /tmp/xauth_file $DISPLAY

# 다른 사용자를 위한 인증 키 병합
xauth merge /tmp/xauth_file
```
xauth는 X 서버와 클라이언트 간의 보안 통신을 위한 인증 키를 관리
> > xhost (호스트 접근 제어)
```
# 현재 접근 허용된 호스트 목록 확인
xhost

# 특정 호스트 접근 허용
xhost +hostname

# 모든 접근 제한
xhost -
```
xhost는 X 서버에 접근할 수 있는 호스트를 관리하는 명령
> > xprop (윈도우 속성 관리)
```
# 루트 윈도우 속성 확인
xprop -root

# 특정 윈도우 속성 확인 (마우스로 선택)
xprop

# 특정 윈도우의 이름 확인
xprop WM_NAME
```
xprop는 X 윈도우의 속성을 조회하고 설정할 수 있는 도구

> #### Xtoolkit

> C 언어 기반 Xtoolkit
> > Xt Intrinsics(X 윈도우 시스템의 기본적인 위젯 툴킷, 저수준 Xlib 위에서 동작하는 기본 프레임워크 제공) <br>
Xaw (MIT에서 개발한 기본적인 위젯 세트, 간단한 GUI 요소들을 제공하는 기본 툴킷) <br>
Motif (상업용으로 개발된 고급 GUI 툴킷, 유닉스 시스템에서 널리 사용됨) <br>
GTK+ (**GNOME 데스크톱 환경의 기반**, 다양한 프로그래밍 언어 바인딩 제공) <br>
XView (Sun Microsystems에서 개발, OpenLook GUI 표준을 구현한 툴킷)

> C++ 기반 Xtoolkit
> > Qt(C++ 기반의 크로스 플랫폼 GUI 툴킷, **KDE 데스크톱 환경의 기반**, 현대적이고 강력한 GUI 기능 제공)

### X 윈도 활용

-----

> #### 데스크톱 환경별 도구

> GNOME 환경
> > eog(Eye of GNOME): GNOME 데스크톱 환경의 기본 이미지 뷰어 <br>
Nautilus: 파일 관리자 <br>
Metacity: 윈도우 매니저 (GNOME 2) <br>
Mutter: 윈도우 매니저 (GNOME 3) <br>
GDM: GNOME 데스크톱 환경용 디스플레이 매니저

> KDE 환경
> > Dolphin: 파일 관리자 <br>
Konsole: 터미널 에뮬레이터 <br>
KWrite: 텍스트 에디터 <br>
KDM: KDE 데스크톱 환경용 디스플레이 매니저

-----

> #### 리눅스 웹브라우저

> 크로스 플랫폼 브라우저 (윈도우와 공용)
> > Firefox: 모질라에서 개발한 오픈소스 브라우저로, 대부분의 리눅스 배포판의 기본 브라우저 <br>
Chrome/Chromium: 크롬은 구글의 상용 버전, Chromium은 오픈소스 버전 <br>
Opera: 탭 브라우징과 스피드 다이얼 기능을 최초로 도입한 혁신적인 브라우저 <br>
Brave: 개인정보 보호와 광고 차단 기능이 강화된 브라우저 <br>
Vivaldi: 고급 사용자 설정 옵션이 많은 크로미움 기반 브라우저

> 리눅스 전용 브라우저
> > Konqueror(퀀커러): KDE 데스크톱의 기본 웹 브라우저이자 파일 관리자 <br>
Galeon(갈레온): GNOME 기반의 웹 브라우저 <br>
Midori: 경량 리눅스 시스템을 위한 가벼운 브라우저 <br>
Falkon(구 QupZilla): Qt 기반의 경량 브라우저

-----

## 2. 인터넷 활용

### 네트워크의 개념
네트워크는 컴퓨터와 다른 디바이스들이 데이터를 주고받을 수 있도범위에 따른 분류이며, 범위에 따라서 아래처럼 분류된다.

#### 네트워크의 분류
+ LAN: 근거리 통신망으로 제한된 지역 내 네트워크
+ WAN: 광역 통신망으로 넓은 지역을 연결하는 네트워크록 연결된 구조

또 연결방식에 따라서도 아래와 같이 분류된다.
+ 유선 네트워크: 물리적 케이블을 통한 연결
+ 무선 네트워크: 전파를 이용한 연결

> #### LAN

1. **LAN 규격**

> 주요 규격 예시
> > 10 BASE-T(해석방법→ 10Mbps, BASE: 베이스밴드 방식(디지털 신호), 트위스트 페어) <br>
100 BASE-TX <br>
1000 BASE-T

위와 같은 규격은 IEEE(전기전자기술자협회)에서 제정한 규격이다.

2. **케이블 배열 규격**
트위스트 페어(Twisted Pair)는 두 개의 구리선을 서로 꼬아놓은 형태의 네트워크 케이블로 <br>
전자기 간섭(EMI)을 최소화하고 신호의 품질을 향상시키기 위해서 꼬아놓은 형태이다. <br>

트위스트 페어 케이블의 배열 순서도 두 가지 표준이 있는데, EIA/TIA(미국전자공업협회/통신산업협회)에서 제정했다.<br>
각 배열은 4쌍(8개)의 전선으로 구성된다.

> T568A 배열
> >흰녹-녹-흰주-파-흰파-주-흰갈-갈

> T568B 배열
> >흰주-주-흰녹-파-흰파-녹-흰갈-갈

3. **LAN 전송방식**

> 이더넷
> > 가장 널리 사용되는 방식 <br>
CSMA/CD 방식으로 충돌 감지 <br>
작동 방식: 데이터 전송 전 회선 사용여부 확인, 충돌 발생시 임의 시간 대기 후 재전송

> 토큰링
> > IBM이 개발한 링 형태의 네트워크 <br>
토큰을 가진 컴퓨터만 데이터 전송 가능 <br>
충돌이 없지만 토큰 대기 시간 존재

> FDDI
> > 광섬유 케이블 사용 <br>
이중 링 구조로 안정성 높음 <br>
토큰 패싱 방식 사용 <br>
고속 데이터 전송 가능(100Mbps)

#### 네트워크 장비

1계층(물리): 허브, 리피터 <br>
2계층(데이터링크): 브리지, 스위치 <br>
3계층(네트워크): 라우터 <br>
7계층(응용): 게이트웨이 <br>

-----

> #### 기본 네트워크 장비

> 라우터 (Router)
> > 마치 우체국처럼 패킷을 목적지로 전달하는 장치 <br>
여러 네트워크를 연결하고 최적의 경로 선택 <br>
예: 집에서 사용하는 공유기

> 리피터 (Repeater)
> > 마치 확성기처럼 약해진 신호를 증폭 <br>
먼 거리 통신을 가능하게 함 <br>
단순히 신호만 증폭하는 1계층 장비

> 브리지 (Bridge)
> > 마치 다리처럼 두 네트워크를 연결 <br>
LAN을 확장하고 충돌 영역을 분리 <br>
2계층에서 동작하며 MAC 주소 기반 통신

-----

> #### 추가 네트워크 장비

> 스위치 (Switch)
> > 현대 네트워크의 핵심 장비 <br>
여러 컴퓨터를 연결하여 데이터를 전달 <br>
MAC 주소를 기반으로 통신을 관리

> 허브 (Hub)
> > 단순히 여러 컴퓨터를 연결하는 장치 <br>
모든 포트로 데이터를 전송(브로드캐스트) <br>
현재는 거의 스위치로 대체됨

> 게이트웨이 (Gateway)
> > 서로 다른 프로토콜을 사용하는 네트워크 연결 <br>
프로토콜 변환 기능 제공 <br>
예: 이메일 게이트웨이

-----

### 1계층에서 7계층까지의 OSI 모델과 관련장비 및 프로토콜 정리
프로토콜의 대한 정리는 TIL의 [ETC > 프로토콜](https://github.com/hyungoo7703/TIL/blob/main/etc/%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C.md)을 참조하면 된다.

![image](https://github.com/user-attachments/assets/f3c6ea01-2f7b-4e6f-b673-f50c2158de19)

라우터는 2계층의 기능도 사용하지만, 주된 목적과 동작은 3계층에서 이루어지는 것이라 3계층으로 표현했다.

### 인터넷 서비스의 종류

> #### WWW (World Wide Web)
HTTP 프로토콜 기반의 정보 검색 시스템, 분산 클라이언트-서버 모델 사용
+ 대표적인 브라우저: Chrome, Firefox, Opera, Edge

> #### 이메일 서비스
전자 메일 송수신 서비스
+ 송신: SMTP 프로토콜
+ 수신: POP3 또는 IMAP4 프로토콜
+ MIME: 멀티미디어 메일 전송 표준

> #### FTP (File Transfer Protocol)
호스트 간 파일 전송 서비스
+ 포트: 20번(데이터), 21번(제어)
+ 통신모드: 패시브/액티브 모드

> #### DNS (Domain Name System)
도메인 이름과 IP 주소 변환 서비스

> #### 원격 접속 서비스
+ Telnet: 암호화되지 않은 원격 접속
+ SSH: 보안이 강화된 원격 접속

> #### NFS (Network File System) 
네트워크 기반 파일 시스템 공유

> #### Samba
리눅스와 윈도우 간 파일 공유 <br>
SMB/CIFS 프로토콜 사용

### 인터넷 서비스의 설정

#### 네트워크 설정 파일
1. **/etc/sysconfig/network**
> 시스템의 기본 네트워크 설정을 담당 <br>
> 네트워크 사용 여부, 호스트 이름, 기본 게이트웨이 등 시스템 전체에 적용되는 기본 설정을 담당
주요 설정 항목:
> > NETWORKING: 네트워크 활성화 여부(yes/no) <br>
FORWARD_IPV4: IP 포워딩 허용 여부 <br>
HOSTNAME: 시스템의 호스트명 <br>
GATEWAY: 기본 게이트웨이 IP 주소 <br>
GATEWAYDEV: 게이트웨이 접근용 네트워크 장치명
```
# 설정 예시
NETWORKING=yes
HOSTNAME=myserver.example.com
GATEWAY=192.168.1.1
FORWARD_IPV4=yes
GATEWAYDEV=eth0
```

2. **/etc/sysconfig/network-scripts/ifcfg-ethX**
> 각 네트워크 인터페이스별 설정 파일
> 각각의 네트워크 카드 설정(ex. IP 주소를 자동으로 받을지(DHCP), 수동으로 설정할지(Static) 결정)
주요 설정 항목:
> > DEVICE: 장치명 <br>
BOOTPROTO: IP 할당 방식(dhcp/none/static) <br>
ONBOOT: 부팅 시 자동 시작 여부 <br>
IPADDR: IP 주소 <br>
NETMASK: 넷마스크 <br>
USERCTL: 일반 사용자의 제어 허용 여부
```
# 설정 예시
# 고정 IP 설정
DEVICE=eth0
BOOTPROTO=static
ONBOOT=yes
IPADDR=192.168.1.100
NETMASK=255.255.255.0
USERCTL=no

# DHCP 설정
DEVICE=eth0
BOOTPROTO=dhcp
ONBOOT=yes
```

3. **/etc/resolv.conf**
> DNS 관련 설정 파일 <br>
주요 설정 항목:
> > nameserver: DNS 서버 IP 주소 <br>
domain: 기본 도메인명 <br>
search: 호스트명 검색 시 사용할 도메인 목록 <br>

4. **/etc/hosts**
> 호스트명과 IP 주소의 매핑 정보(IP 주소와 호스트 이름을 직접 매핑하는 파일) <br>
DNS 조회 전에 먼저 참조되는 파일 <br>
형식: IP주소 호스트명 [별칭...] <br>
주석은 # 으로 시작

5. **/etc/host.conf**
> DNS 리졸버의 동작 방식 설정 <br>
hosts 파일과 DNS 서버의 우선순위 지정 <br>
로컬 네트워크의 주요 호스트 정보 관리

#### IP 주소 설정 방법
1. 명령어를 통한 설정
> ifconfig: 네트워크 인터페이스 설정/확인, route: 라우팅 테이블 설정/확인, ip addr: IP 주소 설정
2. 설정 파일을 통한 설정
> /etc/sysconfig/network-scripts/ifcfg-ethX 파일 직접 수정(영구적인 설정 가능, 서버 재시작 후에도 설정 유지)
3. 유틸리티를 통한 설정
> system-config-network: 텍스트 기반 설정 도구, nm-connection-editor: GUI 기반 설정 도구

## 3. 응용분야
