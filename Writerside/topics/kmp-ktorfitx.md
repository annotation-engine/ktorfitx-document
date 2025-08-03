# 创建 Ktorfitx

- 在为每个接口生成的实现类下方，会自动生成一个扩展函数。该扩展函数用于将该 API 接口绑定到特定的作用域（Scope），以便在调用时区分其所属的上下文环境
- 扩展函数的泛型参数 `AS` 表示接口绑定的作用域类型。该类型通过接口上的 `@ApiScope` 注解进行配置，默认泛型值为 `DefaultApiScope`。你可以在 `@ApiScope`
  中声明多个作用域类型，生成器会为每个作用域自动生成一个对应的扩展函数，从而限制只在制定的作用域上使用此实例
- 生成的 API 实现类的构造函数中，会自动注入一个 `KtorfitxConfig` 配置对象。该配置对象承载了通过 `ktorfitx {}` DSL 中配置的参数，包括 `baseUrl`、`token`、`HttpClient` 等参数，确保生成的
  API 实例在运行时获取到对应的实例

## DSL

- 在项目中使用 `ktorfitx {}` DSL 来创建实例，这里的作用域是默认的 `DefaultApiScope`

```kotlin
val ktorfitx = ktorfitx {
	baseUrl = "<base-url>"  // 配置网络请求公共前缀
	token { "<token>" }     // 获取 Token，例如：从本地存储中获取
	httpClient(CIO) {       // 配置 HttpClient，CIO 是网络请求引擎
		// ...
		// 具体如何配置请查阅 Ktor 的官方文档
	}
	
	// 需要开启 Mock 功能，在 build.gradle.kts 中配置
	mockClient {
		// 这里配置 MockClient
	}
}
```

- 自定义作用域，建议使用 `sealed interface` 来修饰自定义的类型

```kotlin
val ktorfitx = ktorfitx<CustomApiScope> {
	// ...
}

sealed interface CustomApiScope
```

## 开启 Mock 功能

- 首先你需要在 build.gradle.kts 中进行以下配置，前提是你添加了 `cn.ktorfitx.multiplatform` Gradle 插件

```kotlin
ktorfitx {
	mock.enabled = true
}
```

- 然后在 `ktorfitx` DSL 中配置 MockClient

```kotlin
val ktorfitx = ktorfitx {
	// 省略其他配置
	
	// 在这里配置
	mockClient {
		log {
			level = LogLevel.INFO   // 配置日志级，默认值为 LogLevel.HEADERS
			logger = ::println      // 配置日志输出方式，默认使用 println 函数输出
		}
		format = Json               // 配置序列化器，默认使用 .toString() 格式化为 String 类型输出
	}
}
```