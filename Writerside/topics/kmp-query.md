# @Query

将参数映射为 URL 查询参数（?key=value）

## 示例代码

在这里使用 `@Query` 注解，此注解目的是为了将参数映射成 URL 查询参数，生成 Ktor 提供的 `this.parameter()` 函数

它有个 `name` 参数，用来设置查询参数的键，默认为变量名，混淆不会影响结果，因为这是编译时处理的

```kotlin
@BearerAuth
@GET(url = "friend/infos")
suspend fun queryFriendInfos(
	@Query pageSize: Int,
	@Query(name = "custom") pageNumber: Int
): List<FriendInfo>
```

## 生成实现

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
