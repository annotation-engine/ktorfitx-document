# @BearerAuth

启用 Bearer token 授权方案，自动加入 Authorization 请求头

## 配置 token

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

```kotlin
@Api(url = "userDetail")
interface UserDetailApi {
	
	/**
	 * 在这里使用 @BearerAuth 注解
	 */
	@BearerAuth
	@GET(url = "query")
	suspend fun query(): UserDetailDTO
}

@Serializable
data class UserDetailDTO(
	val username: String,
	val nickname: String
)
```

## 生成代码

在生成的代码中会首先生成获取 token 的代码，然后判空后调用 ktor 提供的 `this.bearerAuth(token)` 方法

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