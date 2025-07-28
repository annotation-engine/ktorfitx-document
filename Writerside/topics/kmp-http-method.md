# @HttpMethod

支持自定义 HTTP 方法类型，用于非标准请求方法，需标注在自定义注解上

## 示例代码

```kotlin
/**
 * 在这里使用 @HttpMethod 注解，method 参数默认会使用自定义注解的注解名称
 */
@HttpMethod(method = "CUSTOM")
@Retention(AnnotationRetention.SOURCE)
@Target(AnnotationTarget.FUNCTION)
annotation class CUSTOM(
	val url: String
)
```

## 注意事项

- 必须标记 `@Retention` 注解，并将值设置为 `AnnotationRetention.SOURCE`
- 必须标记 `@Target` 注解，并将值设置为 `AnnotationTarget.FUNCTION`
- 必须添加一个名称为 `url` 类型为 `String` 的参数
- `@HttpMethod` 的 `method` 默认值为自定义注解名称