---
title: https
date: 2017-11-01 15:31:00
tags:
---

## 创建密钥

``` bash
 keytool -genkey -alias tomcat -keyalg RSA
```

## 修改tomcat配置文件

``` xml
    <Connector port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol" maxThreads="150" SSLEnabled="true" scheme="https" secure="true" clientAuth="false" sslProtocol="TLS" keystoreFile="c:/users/79001/.keystore" keystorePass="37934bit">
        <SSLHostConfig>
            <Certificate certificateKeystoreFile="conf/localhost-rsa.jks" type="RSA" />
        </SSLHostConfig>
    </Connector>
```

## 配置web.xml
