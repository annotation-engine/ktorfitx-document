# @Query

> 将参数映射为 URL 查询参数（?key=value）

## 示例代码

- 在这里使用 `@Query` 注解，此注解目的是为了将参数映射成 URL 查询参数，生成 Ktor 提供的 `this.parameter()` 函数
- 它有个 `name` 参数，用来设置查询参数的键，默认情况下会使用变量名称

```kotlin
@BearerAuth
@GET(url = "friend/infos")
suspend fun queryFriendInfos(
	@Query pageSize: Int,
	@Query(name = "custom") pageNumber: Int
): List<FriendInfo>
```

## 生成实现

- 这里可以看见 `pageNumber` 参数设置了 `"custom"` 参数，在生成的实现中使用了它

```kotlin
override suspend fun queryFriendInfos(pageSize: Int, pageNumber: Int): List<FriendInfo> {
	val token = this.config.token?.invoke()
	val response = this.config.httpClient.`get` {
		this.url("friend/infos")
		if (token != null) {
			this.bearerAuth(token)
		}
		this.parameter("pageSize", pageSize)
		this.parameter("custom", pageNumber)
	}
	return response.body()
}
```
