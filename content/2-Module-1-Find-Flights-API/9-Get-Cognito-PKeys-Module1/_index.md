---
title : "Get Cognito public keys"
date :  "`r Sys.Date()`" 
weight : 9
chapter : false
pre : " <b> 2.9 </b> "
---

- You will now build the RSA Key provider class to get the Cognito public key. This will be used to verify the JWT Token later.
- Open AwsCognitoRSAKeyProvider.java under src/main/java/com.airlines.catalog/controller folder and add the following imports

![image.png](/images/module_1/cognito_keys/image.png)

- Below is the Amazon Q Developer prompts to generate the class. Resolve errors in the generated code by working with in-line recommendations.

{{< callout type="info" title="Prompt" >}}

Create AwsCognitoRSAKeyProvider class to implement methods for public key verification.
URL is provided as input
Add other mandatory methods to implement the interface . Handle all the exceptions

{{< /callout >}}

![image.png](/images/module_1/cognito_keys/image_1.png)

- Complete code for AwsCognitoRSAKeyProvider class

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