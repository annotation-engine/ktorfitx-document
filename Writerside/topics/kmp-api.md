# @Api

定义服务接口类型，作为生成 REST 风格 Ktor 请求代码的标识

## 代码示例

```kotlin
/**
 * 在这里使用 @Api 注解
 */
@Api(url = "user")
interface UserApi {
	
	@POST(url = "login")
	suspend fun login(
		@Field username: String,
		@Field password: String
	): LoginDTO
}

@Serializable
data class LoginDTO(
	val token: String
)
```

## 生成实现

以下代码将会自动生成，您只需要通过 `Ktorfitx<DefaultApiScope>` 的扩展属性 `userApi` 获取实例即可

```kotlin
private class UserApiImpl private constructor(
	private val config: KtorfitxConfig,
) : UserApi {
	
	override suspend fun login(username: String, password: String): LoginDTO {
		val response = this.config.httpClient.post {
			this.url("user/login")
			this.contentType(ContentType.Application.FormUrlEncoded)
			this.setBody(listOf("username" to username, "password" to password).formUrlEncode())
		}
		return response.body()
	}
	
	@OptIn(InternalAPI::class)
	companion object {
		
		private var defaultApiScopeInstance: UserApi? = null
		
		private val defaultApiScopeSynchronizedObject: SynchronizedObject = SynchronizedObject()
		
		fun getInstanceByDefaultApiScope(ktorfitx: Ktorfitx<DefaultApiScope>): UserApi = defaultApiScopeInstance ?: synchronized(defaultApiScopeSynchronizedObject) {
			defaultApiScopeInstance ?: UserApiImpl(ktorfitx.config).also { defaultApiScopeInstance = it }
		}
	}
}

val Ktorfitx<DefaultApiScope>.userApi: UserApi
	@JvmName("userApiByDefaultApiScope")
	get() = UserApiImpl.getInstanceByDefaultApiScope(this)

```