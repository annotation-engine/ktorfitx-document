# HTTP 请求 @GET, @POST...

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

```kotlin
@Api(url = "userDetail")
interface UserDetailApi {
	
	@BearerAuth
	@GET(url = "query")
	suspend fun query(): UserDetailDTO
	
	@BearerAuth
	@POST(url = "update")
	suspend fun update(
		@Field type: String,
		@Field value: String?
	): Boolean
}

@Serializable
data class UserDetailDTO(
	val username: String,
	val nickname: String,
	val avatarUrl: String,
)
```

## 生成实现

```kotlin
private class UserDetailApiImpl private constructor(
	private val config: KtorfitxConfig,
) : UserDetailApi {
	
	override suspend fun query(): UserDetailDTO {
		val token = this.config.token?.invoke()
		val response = this.config.httpClient.get {
			this.url("userDetail/query")
			if (token != null) {
				this.bearerAuth(token)
			}
		}
		return response.body()
	}
	
	override suspend fun update(type: String, value: String?): Boolean {
		val token = this.config.token?.invoke()
		val response = this.config.httpClient.post {
			this.url("userDetail/update")
			if (token != null) {
				this.bearerAuth(token)
			}
			this.contentType(ContentType.Application.FormUrlEncoded)
			this.setBody(listOf("type" to type, "value" to value).formUrlEncode())
		}
		return response.body()
	}
	
	@OptIn(InternalAPI::class)
	companion object {
		
		private var defaultApiScopeInstance: UserDetailApi? = null
		
		private val defaultApiScopeSynchronizedObject: SynchronizedObject = SynchronizedObject()
		
		fun getInstanceByDefaultApiScope(ktorfitx: Ktorfitx<DefaultApiScope>): UserDetailApi = defaultApiScopeInstance ?: synchronized(defaultApiScopeSynchronizedObject) {
			defaultApiScopeInstance ?: UserDetailApiImpl(ktorfitx.config).also { defaultApiScopeInstance = it }
		}
	}
}

val Ktorfitx<DefaultApiScope>.userDetailApi: UserDetailApi
    @JvmName("userDetailApiByDefaultApiScope")
    get() = UserDetailApiImpl.getInstanceByDefaultApiScope(this)
```