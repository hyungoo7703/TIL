# SSL(Secure Sockets Layer)
인터넷 상에서 데이터를 안전하게 전송하기 위한 암호화 프로토콜

### 주요특징

+ <b>데이터 암호화</b>: 웹 서버와 브라우저 사이의 데이터를 암호화하여 개인정보와 민감한 데이터를 보호
+ <b>인증</b>: 웹사이트의 신원을 확인하여 사용자가 의도한 정당한 웹사이트와 통신하고 있음을 보장
+ <b>데이터 무결성</b>: 전송 중 데이터가 변조되지 않았음을 확인
+ <b>HTTPS</b>: SSL/TLS를 사용하는 웹사이트는 URL에 "HTTPS"를 표시
+ <b>인증서 사용</b>: SSL 인증서는 웹사이트의 신원을 확인하고 공개키를 포함하여 암호화된 연결을 설정
+ <b>다양한 인증 수준</b>: 도메인 검증(DV), 조직 검증(OV), 확장 검증(EV) 등 다양한 수준의 인증서

## SSL인증서 재설치 [실무에서 경험한 CASE]
SSL 인증서는 만료일 기준 90일 이내부터 갱신 신청이 가능

### Case 1. Apache에서만 SSL 처리하는 경우
Apache에 SSL 인증서를 설치한 뒤, <br>
SSL 가상호스트를 설정하고 JkMount 지시자로 Tomcat으로 요청을 전달하는 방법이다. <br>
Tomcat에는 AJP 커넥터만 설정하면 된다. <br>
만약 Node.js 애플리케이션 기반 웹이라면, <br>
Tomcat 요청 전달방법 대신 Apache의 프록시 모듈을 사용하여 Node.js 애플리케이션으로 요청을 전달하는 방식으로 처리하면 된다.

일반적으로 /etc/apache2/sites-available/ 또는 /etc/httpd/conf.d/ 디렉토리에 위치한 Apache 설정파일을 수정하면 된다. <br>
SSL을 사용할 도메인의 Virtual Host 설정을 수정 해주어 처리한다.
```
# Tomcat 버전
<VirtualHost *:443>
    ServerName www.yourdomain.com
    DocumentRoot /var/www/yourdomain
    
    SSLEngine on
    SSLCertificateFile /path/to/your_domain.crt #서버 인증서
    SSLCertificateKeyFile /path/to/your_domain.key #개인 키 파일
    SSLCertificateChainFile /path/to/intermediate.crt #중간 인증서
    
    # 기타 필요한 설정들
</VirtualHost>

# Node.js 애플리케이션 버전
<VirtualHost *:443>
  ServerName www.yourdomain.com
  SSLEngine on
  SSLCertificateFile /path/to/your_domain.crt
  SSLCertificateKeyFile /path/to/your_domain.key
  
  ProxyRequests Off
  ProxyPreserveHost On
  ProxyVia Full
  <Proxy *>
    Require all granted
  </Proxy>

  ProxyPass / http://localhost:3000/
  ProxyPassReverse / http://localhost:3000/
</VirtualHost>
```
Apache를 재시작 하여 SSL 처리가 완료된다.

### Case 2. Tomcat에서만 SSL 처리하는 경우
Tomcat이 SSL 연결을 직접 처리하고 있음을 의미. Tomcat이 SSL 종단점(endpoint)역할 <br>
보통 이런 케이스의 경우 Tomcat이 독립 실행형 웹 서버로 작동하는 경우가 대부분 이기에 SSL인증서 재설치가 매우 간편한 편이다. <br>
<b>Apache가 프록시 서버로 사용되는 경우에도, Tomcat이 SSL을 처리하므로 Apache에는 별도의 SSL 설정이 필요하지 않는다.</b>

![image](https://github.com/user-attachments/assets/4a450786-6cb3-4e9f-a763-4549f047a205)
> 이미지 출처: [한국전자인증-SSL설치메뉴얼](https://cert.crosscert.com/tomcat-ssl%EC%9D%B8%EC%A6%9D%EC%84%9C-%EC%84%A4%EC%B9%98-%EB%A9%94%EB%89%B4%EC%96%BC/)

### Case 3. Apache와 Tomcat 둘 다 SSL 처리하는 경우
보안을 위해 Apache와 Tomcat 간 통신도 SSL로 암호화 하는 경우가 이에 해당한다. <br>
대부분 단일 SSL 종단점을 사용하지만,(앞서도 별도 필요하다 말하지 않았다.) <br>
Apache가 프록시 서버로 사용되고 Tomcat이 SSL을 처리하는 경우에도, Apache에는 클라이언트와의 통신을 위한 SSL 설정이 필요할 수 있고,  <br>
또한, Apache에서 Tomcat으로의 요청을 HTTPS로 전달하기 위한 프록시 설정도 필요할 수 있다. <br>
즉 선택사항으로써 처리할 수 있다. <br>
두 서버 모두에 SSL을 설치하면 구성이 더 복잡해지기에 권장되지는 않는다.(실제로 매우 고생한 경험...)

