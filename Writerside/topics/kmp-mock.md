# @Mock

定义 Mock 行为逻辑，用于测试或演示 Mock 数据

## 示例代码

这里使用 `@Mock` 注解，并制定 provider 参数

```kotlin
@POST(url = "friend/{friendId}")
@Mock(provider = FriendMockProvider::class)
suspend fun fetchFriend(
	@Path friendId: Int,
): FriendDTO
```

定义 `FriendMockProvider` Mock 提供者，必须是 `object` 类型

`MockProvider<R>` 的泛型必须和请求函数的返回类型一致，如果返回类型是 `Result<T>` 则需要和类型 `T` 保持一致

```kotlin
object FriendMockProvider : MockProvider<FriendDTO> {
	
	override fun provide(): FriendDTO {
		return TODO()
	}
}
```

## 代码生成

```kotlin
override suspend fun fetchFriend(friendId: Int): FriendDTO = this.config.mockClient.request(
	method = HttpMethod.Post,
	mockProvider = FriendMockProvider,
) {
	this.url("friend/${friendId}")
	this.paths {
		this.append("friendId", friendId)
	}
}
```

## 注意事项 ⚠️

目前暂不支持在 `provide()` 函数中获取请求参数，计划在未来版本中支持，但目前还未设计如何实现此功能