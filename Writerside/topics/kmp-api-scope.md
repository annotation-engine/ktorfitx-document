# @ApiScope

定义服务接口类型，作为生成 REST 风格 Ktor 请求代码的标识

## 示例代码

```kotlin
@ApiScope(scopes = [UserScope::class])
@Api(url = "user")
interface UserApi

sealed interface UserScope
```

## 生成代码

注意，这里生成的 `userApi` 是 `Ktorfitx<UserScope>` 的扩展属性，而不是默认的 `Ktorfitx<DefaultApiScope>` 的了

```kotlin
private class UserApiImpl private constructor(
	private val config: KtorfitxConfig,
) : UserApi {
	
	@OptIn(InternalAPI::class)
	companion object {
		private var userScopeInstance: UserApi? = null
		
		private val userScopeSynchronizedObject: SynchronizedObject = SynchronizedObject()
		
		fun getInstanceByUserScope(ktorfitx: Ktorfitx<UserScope>): UserApi = userScopeInstance ?: synchronized(userScopeSynchronizedObject) {
			userScopeInstance ?: UserApiImpl(ktorfitx.config).also { userScopeInstance = it }
		}
	}
}

val Ktorfitx<UserScope>.userApi: UserApi
@JvmName("userApiByUserScope")
get() = UserApiImpl.getInstanceByUserScope(this)
```

> [!NOTE]
>
> 测试