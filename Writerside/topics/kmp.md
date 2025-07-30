# Kotlin Multiplatform

在 Kotlin Multiplatform 中，接口生成的实现类在当前包下的 `impls` 包中，
扩展名为当前类名称的小驼峰形式

## 支持平台

| 平台      | 架构 / Target 编译类型                                                                           |
|---------|--------------------------------------------------------------------------------------------|
| Android | JVM – androidTarget (JVM 21)                                                               |
| Desktop | JVM – jvm("desktop") (JVM 21)                                                              |
| iOS     | iosX64, iosArm64, iosSimulatorArm64 (Native)                                               |
| macOS   | macosX64, macosArm64 (Native)                                                              |
| watchOS | watchosX64, watchosArm32, watchosArm64, watchosSimulatorArm64, watchosDeviceArm64 (Native) |
| tvOS    | tvosX64, tvosArm64, tvosSimulatorArm64 (Native)                                            |
| Linux   | linuxX64, linuxArm64 (Native executables)                                                  |
| Windows | mingwX64 (Native executable via MinGW)                                                     |
| Js      | js(IR) – outputModuleName + nodejs()                                                       |
| WasmJs  | wasmJs (ExperimentalWasmDsl + nodejs())                                                    |

## 全部注解

### 目标 `ANNOTATION_CLASS`

- [`@HttpMethod`](kmp-http-method.md)：支持自定义 HTTP 方法类型，用于非标准请求方法，需标注在自定义注解上

### 目标 `CLASS`

- [`@Api`](kmp-api.md)：定义服务接口类型，作为生成 REST 风格 Ktor 请求代码的标识
- [`@ApiScope`](kmp-api-scope.md)：定义生成接口的作用域，控制扩展方法中泛型的类型以规定此服务作用于指定 ktorfitx 配置

### 目标 `FUNCTION`

- [`@GET、@POST、@PUT、@DELETE、@PATCH、@OPTIONS、@HEAD`](kmp-http-request.md)：HTTP 请求
- [`@BearerAuth`](kmp-bearer-auth.md)：启用 Bearer token 授权方案，自动加入 Authorization 请求头
- [`@Headers`](kmp-headers.md)：声明静态请求头，生成时自动注入多个 headers
- [`@Mock`](kmp-mock.md)：定义 Mock 行为逻辑，用于测试或演示 Mock 数据
- [`@WebSocket`](kmp-web-socket.md)：将接口函数定义为 WebSocket 通道处理入口
- [`@Timeout`](kmp-timeout.md)：配置请求的超时时间

### 目标 `VALUE_PARAMETER`

- [`@Body`](kmp-body.md)：设置请求体内容，支持配置多种序列化类型
- `@Query`：将参数映射为 URL 查询参数（?key=value）
- `@Field`：以 x-www-form-urlencoded 格式提交表单字段
- `@Part`：表示 multipart/form-data 的表单部分，用于文件上传或复杂表单
- `@Header`：动态注入 HTTP 请求头参数
- `@Path`：映射路径参数，对 URL 模板中的 `{}` 片段进行替换
- `@Cookie`：从 Cookie 中提取参数值注入接口
- `@Attribute`：用于传递请求属性（attribute），可携带元数据或上下文信息

#### 注意事项 ⚠️

- `@Body`、`@Field`、`@Part` 这三个注解不能在同一个请求中同时使用