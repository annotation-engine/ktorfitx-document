# @Timeout

> 配置请求超时时间，用于控制方法的最长执行时长

## 示例代码

- 在这里使用 `@Timeout` 注解，配置：请求超时时间、连接超时时间 和 套接字超时时间

```kotlin
@Timeout(
	requestTimeoutMillis = 10_000L,
	connectTimeoutMillis = 10_000L,
	socketTimeoutMillis = 10_000L
)
@BearerAuth
@GET(url = "user/info")
suspend fun getUserInfo(): UserInfo
```

## 生成实现

- 在 `@Timeout` 中配置的参数会在实现中生成 `this.timeout { }`

```kotlin
override suspend fun getUserInfo(): UserInfo {
	val token = this.config.token?.invoke()
	val response = this.config.httpClient.`get` {
		this.url("user/info")
		this.timeout {
			this.requestTimeoutMillis = 10_000L
			this.connectTimeoutMillis = 10_000L
			this.socketTimeoutMillis = 10_000L
		}
		if (token != null) {
			this.bearerAuth(token)
		}
	}
	return response.body()
}
```