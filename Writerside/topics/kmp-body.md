# @Body

设置请求体内容，支持配置多种序列化类型

## 示例代码

`@Body` 注解可用于设置请求体的序列化方式。通过 `format` 参数，可以指定使用的 `SerializationFormat`，从而生成对应的 `ContentType` 类型的请求体

可选的 `SerializationFormat` 类型及对应的 `ContentType` 如下：

- `SerializationFormat.JSON` 对应 `ContentType.Application.Json` 默认值
- `SerializationFormat.XML` 对应 `ContentType.Application.Xml`
- `SerializationFormat.CBOR` 对应 `ContentType.Application.Cbor`
- `SerializationFormat.PROTO_BUF` 对应 `ContentType.Application.ProtoBuf`

在这里使用 `@Body` 注解，这里为 `format` 设置了 `SerializationFormat.XML` 值

```kotlin
@BearerAuth
@POST(url = "user/detail/update")
suspend fun updateUserDetail(
	@Body(format = SerializationFormat.XML) data: UserDetail
): Boolean
```

## 生成的实现

在实现中这里为 `contentType()` 函数设置了 `ContentType.Application.Xml` 值，对应注解中设置的 `SerializationFormat.XML`

```kotlin
override suspend fun updateUserDetail(`data`: UserDetail): Boolean {
	val token = this.config.token?.invoke()
	val response = this.config.httpClient.post {
		this.url("user/detail/update")
		if (token != null) {
			this.bearerAuth(token)
		}
		this.contentType(ContentType.Application.Xml)
		this.setBody(`data`)
	}
	return response.body()
}
```