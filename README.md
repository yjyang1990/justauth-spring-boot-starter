# justauth-spring-boot-starter

> Spring Boot 集成 JustAuth 的最佳实践~
>
> JustAuth 脚手架

![Maven Central](https://img.shields.io/maven-central/v/com.xkcoding/justauth-spring-boot-starter.svg?color=brightgreen&label=Maven%20Central)
![Travis (.com)](https://img.shields.io/travis/com/xkcoding/justauth-spring-boot-starter.svg?label=Build%20Status)
![GitHub](https://img.shields.io/github/license/xkcoding/justauth-spring-boot-starter.svg)

## Demo

懒得看文档的，可以直接看demo

https://github.com/xkcoding/justauth-spring-boot-starter-demo

## 快速开始

- 引用依赖

```xml
<dependency>
  <groupId>com.xkcoding</groupId>
  <artifactId>justauth-spring-boot-starter</artifactId>
  <version>0.0.2</version>
</dependency>
```

- 添加配置，在 `application.yml` 中添加配置配置信息

```yaml
justauth:
  enabled: true
  type:
    QQ:
      client-id: 10**********6
      client-secret: 1f7d08**********5b7**********29e
      redirect-uri: http://oauth.xkcoding.com/demo/oauth/qq/callback
```

- 然后就开始玩耍吧~

```java
@Slf4j
@RestController
@RequestMapping("/oauth")
@RequiredArgsConstructor(onConstructor_ = @Autowired)
public class TestController {
    private final AuthRequestFactory factory;

    @GetMapping("/login/qq")
    public void login(HttpServletResponse response) throws IOException {
        AuthRequest authRequest = factory.get(AuthSource.QQ);
        response.sendRedirect(authRequest.authorize(AuthStateUtils.createState()));
    }

    @RequestMapping("/qq/callback")
    public AuthResponse login(AuthCallback callback) {
        AuthRequest authRequest2 = factory.get(AuthSource.QQ);
        AuthResponse response = authRequest2.login(callback);
        log.info("【response】= {}", JSONUtil.toJsonStr(response));
        return response;
    }
}
```

## 附录

`justauth` 配置列表

| 属性名             | 类型                                                         | 默认值 | 可选项     | 描述              |
| ------------------ | ------------------------------------------------------------ | ------ | ---------- | ----------------- |
| `justauth.enabled` | `boolean`                                                    | true   | true/false | 是否启用 JustAuth |
| `justauth.type`    | `java.util.Map<me.zhyd.oauth.config.AuthSource,me.zhyd.oauth.config.AuthConfig>` | 无     |            | JustAuth 配置     |

`justauth.type` 配置列表

| 属性名                      | 描述                                                         |
| --------------------------- | ------------------------------------------------------------ |
| `justauth.type.keys`        | `justauth.type` 是 `Map` 格式的，key 的取值请参考 [`AuthSource`](https://github.com/zhangyd-c/JustAuth/blob/master/src/main/java/me/zhyd/oauth/config/AuthSource.java) |
| `justauth.type.keys.values` | `justauth.type` 是 `Map` 格式的，value 的取值请参考 [`AuthConfig`](https://github.com/zhangyd-c/JustAuth/blob/master/src/main/java/me/zhyd/oauth/config/AuthConfig.java) |

