# @Headers

声明静态请求头，生成时自动注入多个 headers。

## 示例代码

```kotlin
@Headers(
	"X-App-Name: KtorfitxDocument",
	"X-Locale: zh-CN"
)
@GET(url = "info")
suspend fun fetchInfo(): Info
```

## 生成代码

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