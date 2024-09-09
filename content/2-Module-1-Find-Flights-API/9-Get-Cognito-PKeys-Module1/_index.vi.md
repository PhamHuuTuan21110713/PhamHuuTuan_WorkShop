---
title : "Lấy Cognito public keys"
date :  "`r Sys.Date()`" 
weight : 9
chapter : false
pre : " <b> 2.9 </b> "
---

- Bây giờ chúng ta sẽ xây dựng lớp RSA Key provider để lấy Cognito public key. Điều này sẽ được sử dụng để xác minh JWT Token  sau này.
- Mở **AwsCognitoRSAKeyProvider.java** trong thư mục **src/main/java/com.airlines.catalog/controller** và thêm các import sau

![image.png](/images/module_1/cognito_keys/image.png)

- Dưới đây là prompt của  Amazon Q Developer để tạo lớp.

{{< callout type="info" title="Prompt" >}}

Create AwsCognitoRSAKeyProvider class to implement methods for public key verification.
URL is provided as input
Add other mandatory methods to implement the interface . Handle all the exceptions

{{< /callout >}}

![image.png](/images/module_1/cognito_keys/image_1.png)

- Code hoàn chỉnh cho lớp **AwsCognitoRSAKeyProvider**

```java
package com.airlines.catalog.controller;

import com.auth0.jwk.JwkException;
import com.auth0.jwk.JwkProvider;
import com.auth0.jwk.JwkProviderBuilder;
import com.auth0.jwt.interfaces.RSAKeyProvider;
import java.net.MalformedURLException;
import java.net.URL;
import java.security.interfaces.RSAPrivateKey;
import java.security.interfaces.RSAPublicKey;

/* create a AwsCognitoRSAKeyProvider class to implement methods for public key verification.
URL is provided as input
Add other mandatory methods to implement the interface. Handle all the exceptions.
*/
public class AwsCognitoRSAKeyProvider implements RSAKeyProvider {

    private final JwkProvider provider;

    public AwsCognitoRSAKeyProvider(String url) throws MalformedURLException {
        provider = new JwkProviderBuilder(new URL(url)).build();
    }

    @Override
    public RSAPublicKey getPublicKeyById(String kid) {
        try {
            return (RSAPublicKey) provider.get(kid).getPublicKey();
        } catch (JwkException e) {
            e.printStackTrace();
        }
        return null;
    }

    @Override
    public RSAPrivateKey getPrivateKey() {
        return null;
    }

    @Override
    public String getPrivateKeyId() {
        return null;
    }
}
```