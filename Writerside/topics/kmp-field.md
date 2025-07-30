# @Field

> 以 x-www-form-urlencoded 格式提交表单字段

## 示例代码

- 在这里使用 `@Field` 注解，它是以 x-www-form-urlencoded 格式提交表单字段
- 当使用非 `String` 类型时，会首先执行 `.toString()` 方法转换为 `String` 类型
- 它同样有个 `name` 参数，用来设置表单参数名，默认情况下会使用变量名称

```kotlin
@POST(url = "user/register")
suspend fun register(
	@Field username: String,
	@Field password: String,
	@Field(name = "captcha") code: Int
): Boolean
```

## 生成实现

- 这里可以看到 `code` 参数为表单参数名设置了 `"captcha"` 值，在生成的实现中可以看到确实使用了它
- 并且可以发现它的类型是 `Int` 的，在生成的实现中将它 `.toString()` 了

```kotlin
override suspend fun register(
	username: String,
	password: String,
	code: Int,
): Boolean {
	val response = this.config.httpClient.post {
		this.url("user/register")
		this.contentType(ContentType.Application.FormUrlEncoded)
		this.setBody(listOf("username" to username, "password" to password, "captcha" to code.toString()).formUrlEncode())
	}
	return response.body()
}
```