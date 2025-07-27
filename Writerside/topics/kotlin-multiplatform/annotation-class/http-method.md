# @HttpMethod

支持自定义 HTTP 方法类型，用于非标准请求方法，需标注在自定义注解上

## 示例代码

```kotlin
@Retention(AnnotationRetention.SOURCE)
@Target(AnnotationTarget.FUNCTION)
@HttpMethod(method = "CUSTOM")
annotation class CUSTOM(
	val url: String
)
```

## 注意事项

- 必须添加 `@Retention` 为 `AnnotationRetention.SOURCE`
- 必须添加 `@Target` 为 `AnnotationTarget.FUNCTION`
- 参数必须按照示例代码中的形式
- `@HttpMethod` 的 `method` 默认值为自定义注解名称