[Spring Authorization Server入门 (二) Spring Boot整合Spring Authorization Server](https://juejin.cn/post/7239953874950815804)

oauth2 接口查看 http://localhost:8080/.well-known/openid-configuration

### 流程

- 获取授权码

```shell
open http://127.0.0.1:8080/oauth2/authorize?client_id=messaging-client&response_type=code&scope=message.read&redirect_uri=https%3A%2F%2Fwww.baidu.com
```

- 使用 `admin` `123456` 登录并获取 code
- 换取 accessToken

```http request
@code = 获取到 code

POST http://localhost:8080/oauth2/token
Content-Type: application/x-www-form-urlencoded
# 注意这里填的是 clientId 和 clientSecret, 授权方式基于 `.clientAuthenticationMethod(ClientAuthenticationMethod.CLIENT_SECRET_BASIC)` 配置
Authorization: Basic messaging-client 123456

grant_type = authorization_code &
code = {{code}} &
redirect_uri = https://www.baidu.com

<> 2025-03-14T205211.200.json
```

结果示意:

```json
{
    "access_token": "eyJraWQiOiJhNzQwOGMyYy1lYmJhLTRkYmYtYTNhZC05M2JlZDAzNzRjYmYiLCJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJhZG1pbiIsImF1ZCI6Im1lc3NhZ2luZy1jbGllbnQiLCJuYmYiOjE3NDE5NTY3MzEsInNjb3BlIjpbIm1lc3NhZ2UucmVhZCJdLCJpc3MiOiJodHRwOi8vbG9jYWxob3N0OjgwODAiLCJleHAiOjE3NDE5NjM5MzEsImlhdCI6MTc0MTk1NjczMSwianRpIjoiY2M2YmRhMzgtNGZkNC00YmYyLWJhOWEtMjlhMWU4N2ZhYTlmIn0.gMufxe-PY1VJaUawA45PSwqntA5s-mMKiO-qebrhKiWAb9EKz3VDlbXy5DoXGHdtbEaZp6Wrb14M6SIsUM4sAP2vvCLT62OZS5xdIVz6gjxbWnDJ86vn0gARvHXJf2xZ_RmxXrnShGoMHCqHP7jvNj9lMrJqGFCJU-KINVAOQaINZ0A1-ahoIXouOTZVyYFFzigJ-Ylb3YOtxYcTYZ03elMCCFFIRCeftp3gpuV7k0rw_ZQ9fFB2iMB9I62nSi8lakxy5r5GYcfP0LWnx1abpdmrA4dpdfyMx30H3qpXLMHfAlTAxvN7xClfoF_SvSrVPmcVfRKpzegKGwJko4MpHA",
    "refresh_token": "aGxDvMW9BAPBf8Hfzs8XHANR2OWg8r13xH0E8w4MZWS1AWTLvNy-kJhIVLpvwOFx3cRxLeAiXTUBXE8fcNuREV1Ryfs1nBZbQGYsuOOhnNSZOBZwH4nJ1lIsVUlqy9w0",
    "scope": "message.read",
    "token_type": "Bearer",
    "expires_in": 7199
}
```
