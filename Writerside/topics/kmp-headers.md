# @Headers

> 声明静态请求头，生成时自动注入多个 headers。

## 示例代码

- 在这里使用 `@Headers` 注解，参数是一个字符串数组，字符串中需要满足 `"<key>:<value>"` 的格式
- 字符串中符号 `:` 左右两侧的空格会在解析后被去除

```kotlin
@Headers(
	"X-App-Name: KtorfitxDocument",
	"X-Locale: zh-CN"
)
@GET(url = "info")
suspend fun fetchInfo(): Info
```

## 生成实现

- 这里可以看到正确的设置了在 `@Headers` 注解中设置的请求头

```kotlin
override suspend fun fetchInfo(): Info {
	val response = this.config.httpClient.`get` {
		this.url("info")
		this.headers {
			this.append("X-App-Name", "KtorfitxDocument")
			this.append("X-Locale", "zh-CN")
		}
	}
	return response.body()
}
```