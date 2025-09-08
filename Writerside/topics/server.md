# Ktor Server

## 全部注解

### 目标 `ANNOTATION_CLASS`

- `@HttpMethod` 自定义 HttpMethod

### 目标 `CLASS`

- `@Controller` 控制器，支持提供 `path` 参数，支持嵌套使用，需要在 `object` 类上使用

### 目标 `FUNCTION`

- `@GET` GET 请求
- `@POST` POST 请求
- `@PUT` PUT 请求
- `@DELETE` DELETE 请求
- `@PATCH` PATCH 请求
- `@OPTIONS` OPTIONS 请求
- `@HEAD` HEAD 请求
- `@Authentication` 路由授权
- `@WebSocket` WebSocket
- `@WebSocketRaw` WebSocketRaw
- `@Regex` 正则匹配 path
- `@Timeout` 超时时间

#### 参数

- `@Query` 查询参数
- `@Body` 请求体参数
- `@Field` x-www-form-urlencoded 字段
- `@PartForm` form-data 参数
- `@PartFile` form-data 文件
- `@PartBinary` form-data 二进制参数
- `@PartBinaryChannel` form-data 数据流
- `@Path` path 参数，支持正则表达式
- `@Header` 请求头参数
- `@Attribute` attribute 参数
- `@Cookie` cookie 参数

#### 注意事项 ⚠️

- `@Body`、`@Field`、`@Part*` 这三种类型的注解不能在同一个请求中同时使用