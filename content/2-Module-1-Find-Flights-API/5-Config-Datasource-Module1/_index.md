---
title : "Data Source configuration"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 2.5 </b> "
---
- You will build a Configuration Class to dynamically configure database credentials from AWS Secrets Manager.
- Open the DataSourceConfig.java under the src/main/java/com.airlines.catalog/config folder and add the following import statements.

![image.png](/images/module_1/datasource/image.png)

- Use the following prompt to create DataSourceConfig class and get the configurations required from application.properties file. Delete any methods created inside the class by Amazon Q Developer.

{{< callout type="info" title="Prompt" >}}
*Create public class DataSourceConfig and 2 member variables awsRegion and secretName. Autowire these variables with aws.region and secretmanager.key from application.properties file.*
{{< /callout >}}

![image.png](/images/module_1/datasource/image_1.png)

-Use the following prompt to create getSecret method that will get the database connection string from AWS Secrets Manager.

{{< callout type="info" title="Prompt" >}}
*Create a private getSecret method that connects to AWS Secrets Manager and gets the secret string using member variables awsRegion and secretName.*

{{< /callout >}}

![image.png](/images/module_1/datasource/image_2.png)

- Use the prompt below create getDataSource method that will call the getSecret method and build the data source object.

{{< callout type="info" title="Prompt" >}}
*Create a method getDataSource to build the datasource object.*<br>

*Call the getSecret function. Parse  the returned JSON string to extract host, port, db, username and password. Then configure the mysql url, username and password of the datasource object.*<br>

*Return the datasource object. Throw any Json Processing exception.*<br>

{{< /callout >}}

![image.png](/images/module_1/datasource/image_3.png)

- Complete code for DataSourceConfig class

```java
package com.airlines.catalog.config;

import com.fasterxml.jackson.core.JsonProcessingException;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import software.amazon.awssdk.regions.Region;
import software.amazon.awssdk.services.secretsmanager.SecretsManagerClient;
import software.amazon.awssdk.services.secretsmanager.model.GetSecretValueRequest;
import software.amazon.awssdk.services.secretsmanager.model.GetSecretValueResponse;
import software.amazon.awssdk.services.secretsmanager.model.SecretsManagerException;

import org.springframework.boot.jdbc.DataSourceBuilder;
import com.fasterxml.jackson.core.JsonParser;
import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;
import javax.sql.DataSource;

/*Create public class DataSourceConfig and 2 member variables awsRegion and secretName.
Autowire these variables with aws.region and secretmanager.key from application.properties file.
*/
@Configuration
public class DataSourceConfig {

    @Value("${aws.region}")
    private String awsRegion;

    @Value("${secretmanager.key}")
    private String secretName;

    /* Create a private getSecret method that connects to AWS Secrets Manager and gets the secret string using
    member variables awsRegion and secretName. Catch and throw Secret Manager exceptions  */
    private String getSecret() {
        Region region = Region.of(awsRegion);
        SecretsManagerClient client = SecretsManagerClient.builder()
                .region(region)
                .build();

        GetSecretValueRequest valueRequest = GetSecretValueRequest.builder()
                .secretId(secretName)
                .build();

        GetSecretValueResponse valueResponse = client.getSecretValue(valueRequest);
        return valueResponse.secretString();
    }

    /* Create a method getDataSource to build the datasource object.
   Call the getSecret function. Parse  the returned JSON string to extract host, port, db,
   username and password. Then configure the mysql url, username and password of the datasource object.
   Return the datasource object. Throw any Json Processing exception.
   */
    @Bean
    public DataSource getDataSource() throws JsonProcessingException {
        DataSourceBuilder<?> dataSourceBuilder = DataSourceBuilder.create();
        String secret = getSecret();
        ObjectMapper objectMapper = new ObjectMapper();
        objectMapper.configure(JsonParser.Feature.ALLOW_COMMENTS, true);
        JsonNode jsonNode = objectMapper.readTree(secret);
        String host = jsonNode.get("host").asText();
        int port = jsonNode.get("port").asInt();
        String db = jsonNode.get("db").asText();
        String username = jsonNode.get("username").asText();
        String password = jsonNode.get("password").asText();
        dataSourceBuilder.url("jdbc:mysql://" + host + ":" + port + "/" + db);
        dataSourceBuilder.username(username);
        dataSourceBuilder.password(password);
        return dataSourceBuilder.build();
    }

}
```