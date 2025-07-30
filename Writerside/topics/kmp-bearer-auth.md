# @BearerAuth

> 启用 Bearer token 授权方案，自动加入 Authorization 请求头

## 配置 token

- 函数 `ktorfitx { }` 是用来创建 `Ktorfitx<T>` 对象的
- 具体如何使用 `ktorfitx { }` 函数，请前往 [](kmp-ktorfitx.md) 查看教程

```kotlin
val ktorfitx = ktorfitx {
	/**
	 * 在这里配置获取 token
	 */
	token { "<token>" }
	
	// 省略其他代码
}
```

## 示例代码

- 在这里使用 `@BearerAuth` 注解

```kotlin
@BearerAuth
@GET(url = "query")
suspend fun query(): UserDetailDTO
```

## 生成实现

- 在生成的代码中会首先生成获取 token 的代码，然后判空后调用 ktor 提供的 `this.bearerAuth(token)` 方法

- 这里的 `this.config` 是通过生成类的构造参数获取，具体代码可以前往 [`@Api`](kmp-api.md) 查看教程

```kotlin
override suspend fun query(): UserDetailDTO {
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