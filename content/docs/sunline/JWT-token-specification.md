---
title: 'Jwt token'
weight: 2
---

# JWT token

## Public endpoints configuration

为了增加系统的耐操性，接口白名单配置需要支持 默认+增量 的配置方式。防止漏配或者配错。

### Detailed Design

配置类 `PublicEndpointsCfg.java`

### Example

DBS 项目的默认白名单配置。其余项目 API 不一定一致，但是需要保证这些功能一定要放行。

```java
package cn.sunline.euc.bcb.config;

import jakarta.annotation.PostConstruct;
import java.util.ArrayList;
import java.util.List;
import lombok.Getter;
import lombok.Setter;
import org.springframework.boot.autoconfigure.condition.ConditionalOnProperty;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

@Getter
@Setter
@Component
@ConfigurationProperties(prefix = "twsp.security.authentication.public")
@ConditionalOnProperty(
    prefix = "spring.security",
    name = "enable",
    havingValue = "true",
    matchIfMissing = false
)
public class PublicEndpointsCfg {

    private List<String> endpoints;

    @PostConstruct
    public void init() {
        if (null == endpoints) {
            endpoints = new ArrayList<>();
        }
        endpoints.add("/twsp/eu8500"); // query branch
        endpoints.add("/twsp/eu8502"); // logout
        endpoints.add("/twsp/eu8503"); // login
        endpoints.add("/twsp/eu8504"); // refresh_token
        endpoints.add("/twsp/expression");  // ui
        endpoints.add("/twsp/et*"); // easy task
        endpoints.add("/twsp/ef*"); // easy flow
        endpoints.add("/ET/*"); // way task
        endpoints.add("/EF/*"); // easy flow
        endpoints.add("/aps-service");  // governor ui
        endpoints.add("/apsService");   // governor client
    }
}
```
