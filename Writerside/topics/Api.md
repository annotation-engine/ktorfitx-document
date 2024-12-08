# @Api 注解

## @Api 代码

```kotlin
@Target(AnnotationTarget.CLASS)
@Retention(AnnotationRetention.SOURCE)
annotation class Api(
    
    // 接口前缀
    val url: String = "",
    
    // 接口作用域 
    val apiScope: KClass<out ApiScope> = DefaultApiScope::class
    
    // 多个接口作用域
    val apiScopes: Array<KClass<out ApiScope>> = [],
)
```

## 使用方法

```Kotlin
@Api(url = "/test")
interface TestApi {
    
    @GET(url = "/data")
    suspend fun getData(): String
}
```

需标记在 interface 上，并且不支持 sealed interface，这一点要注意，因为生成后的实现文件不是在同一包下的

## 参数介绍

- **url**：用于定义接口的前缀
- **apiScope**：接口作用域，用于配置接口文件可以被哪些作用域访问