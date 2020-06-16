---
title: 'tomcat https 적용'
description: 'tomcat https 적용'
date: 2019-06-11
---

## SSL인증서 발급

개인 인증서를 발급받아서 HTTPS통신을 하고자 한다.

```bash
# 개인키 생성
openssl genrsa -des3 -out server.key 2048

# 인증요청서 생성. (자신의 정보 입력)
openssl req -new -key server.key -out server.csr

# (선택) 개인키에서 패스워드 제거
cp server.key server.key.origin
openssl rsa -in server.key.origin -out server.key

# 인증서 생성
openssl x509 -req -days 3650 -in server.csr -signkey server.key -out server.crt
# 여기까지 마치면 서버 인증서인 server.crt파일이 생성된다.
```

## SSl인증서를 발급받았으면 이제 톰캣에 맞는 형식으로 바꾸어 준다

server.crt가 준비된 후 실행

```bash
# 인증서이름(server.crt), 인증서키이름(server.key)를 톰캣에 맞는 형식으로(.keystore) 바꿔줌.
openssl pkcs12 -export -in server.crt -inkey server.key -out .keystore -name tomcat
```

## 톰캣 인증서를 생성했으면 server.xml에서 설정해줘야 한다

```xml
<Connector port="443" protocol="org.apache.coyote.http11.Http11NioProtocol"
    maxthreads="150" sslenabled="true" scheme="https" secure="true"
    clientauth="false" sslprotocol="TLS"
    keystorepass="인증서 비밀번호" keystorefile="인증서 위치"/>
```

## http로 접속 시 https로 자동 리다이렉트 설정

```xml
<!-- web.xml에 이 부분을 넣으면 된다는 데 제대로 되지 않아서 좀 더 알아봐야 겠다. -->

<security-constraint>
    <web-resource-collection>
        <web-resource-name>SSL Forward</web-resource-name>
        <url-pattern>/*</url-pattern>
    </web-resource-collection>
    <user-data-constraint>
        <transport-guarantee>CONFIDENTIAL</transport-guarantee>
    </user-data-constraint>
</security-constraint>
```

참고 :

- <https://zetawiki.com/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4_%EC%9E%90%EC%B2%B4%EC%84%9C%EB%AA%85_SSL_%EC%9D%B8%EC%A6%9D%EC%84%9C_%EC%83%9D%EC%84%B1>
- <https://namjackson.tistory.com/25>
- <https://offbyone.tistory.com/262>

@TODO 각각의 명령어가 무슨역할을 하는지 알아보자

@TODO openssl패키지를 이용하여 개인키를 생성했는데 keytool로도 해보자

@TODO http로 접속 시 https로 리다이렉트 되게 하는 법 알아보기
