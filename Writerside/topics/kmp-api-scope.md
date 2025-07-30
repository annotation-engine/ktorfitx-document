# @ApiScope

> 定义生成接口的作用域，控制扩展方法中泛型的类型以规定此服务作用于指定 ktorfitx 配置

## 代码示例

- 在这里使用 @ApiScope 注解，并传递 自定义的 UserScope 作用域
- 作用域类型可以是任意非空类型，建议使用 `sealed interface` 以防止不必要的错误实现此类，它只是用于区分需要为谁生成 Api 接口的扩展属性

```kotlin
@ApiScope(scopes = [UserScope::class])
@Api(url = "user")
interface UserApi

sealed interface UserScope
```

## 生成实现

- 注意，这里生成的 `userApi` 是 `Ktorfitx<UserScope>` 的扩展属性，而不是再是 `Ktorfitx<DefaultApiScope>` 的了

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

## 注意事项 ⚠️

- 除 `@Api` 和 `@ApiScope` 文档，其余文档将不展示接口和类的代码，以方便阅读查看