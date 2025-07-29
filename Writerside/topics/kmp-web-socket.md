# @WebSocket

将接口函数定义为 WebSocket 通道处理入口

## 代码示例

在这里使用 `@WebSocket` 注解，并设置 `WebSocketSessionHandler` 类型，他是 `suspend DefaultClientWebSocketSession.() -> Unit` 类型的别名

```kotlin
@BearerAuth
@WebSocket(url = "friendMessageSocket")
suspend fun friendMessageSocket(handler: WebSocketSessionHandler)
```

你也可以使用 `ws://` 或者 `wss://` 前缀的 url，当然你也可以直接使用 `suspend DefaultClientWebSocketSession.() -> Unit` 类型

```kotlin
@WebSocket("ws://ktorfitx.cn:8080/api/keepAlive")
suspend fun keepAlive(handler: suspend DefaultClientWebSocketSession.() -> Unit)
```

## 生成实现

```kotlin
override suspend fun friendMessageSocket(handler: WebSocketSessionHandler) {
	val token = this.config.token?.invoke()
	this.config.httpClient.webSocket(
		urlString = "friendMessageSocket",
		request = {
			if (token != null) {
				this.bearerAuth(token)
			}
		},
		block = handler
	)
}
```

```kotlin
override suspend fun keepAlive(handler: suspend DefaultClientWebSocketSession.() -> Unit) {
	this.config.httpClient.webSocket(
		urlString = "ws://ktorfitx.cn:8080/api/keepAlive",
		block = handler
	)
}
```