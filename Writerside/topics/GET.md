# @GET 注解

## @GET 代码

```kotlin
@Target(AnnotationTarget.FUNCTION)
@Retention(AnnotationRetention.SOURCE)
annotation class GET(
    
    // 接口路径
    val url: String
)
```

## 使用方法

```kotlin
@Api(url = "/test")
interface TestApi {

    @GET("/get")
    suspend fun get(
        @Query id: Int
        @Query name: String
    ): String
}
```

> 当您需要使用参数的时候需要使用 `@Query` 注解