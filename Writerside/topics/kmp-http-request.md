# HTTP 请求 @GET, @POST...

标记方法对应 HTTP 请求类型及路径

## 请求类型

| 注解         | 请求类型    | 说明                                  |
|------------|---------|-------------------------------------|
| `@GET`     | GET     | 用于获取资源或数据，不应包含请求体。常用于查询或读取操作        |
| `@POST`    | POST    | 用于创建资源或提交数据。支持请求体（如 JSON、表单等）       |
| `@PUT`     | PUT     | 用于整体更新资源，需提供完整的资源内容                 |
| `@DELETE`  | DELETE  | 用于删除指定资源，常用于数据删除操作                  |
| `@PATCH`   | PATCH   | 用于部分更新资源，适合修改资源中的部分字段               |
| `@HEAD`    | HEAD    | 类似 GET 请求，但不返回响应体。用于获取资源的元信息（如是否存在） |
| `@OPTIONS` | OPTIONS | 获取服务器支持的通信选项。常用于 CORS 预检请求          |

## 代码示例

在这里使用 `@GET` 注解并设置 `url`，生成的 url 会自动拼接上 `@Api` 上提供的 `url`

```kotlin
@BearerAuth
@GET(url = "userDetail/query")
suspend fun queryUserDetail(): UserDetailDTO
```

你也可以使用 `http://` 或 `https://` 前缀的 url

```kotlin
@BearerAuth
@POST(url = "https://ktorfitx.cn:8080/api/userDetail/update")
suspend fun updateUserDetail(
	@Field key: String,
	@Field value: String?
): Boolean
```

## 生成实现

```kotlin
override suspend fun queryUserDetail(): UserDetailDTO {
	val token = this.config.token?.invoke()
	val response = this.config.httpClient.get {
		this.url("query")
		if (token != null) {
			this.bearerAuth(token)
		}
	}
	return response.body()
}
```

```kotlin
override suspend fun updateUserDetail(key: String, value: String?): Boolean {
	val token = this.config.token?.invoke()
	val response = this.config.httpClient.post {
		this.url("userDetail/update")
		if (token != null) {
			this.bearerAuth(token)
		}
		this.contentType(ContentType.Application.FormUrlEncoded)
		this.setBody(listOf("key" to key, "value" to value).formUrlEncode())
	}
	return response.body()
}
```