# 项目简介

Ktorfit X 是一个基于 Kotlin Multiplatform 和 Ktor 构建的跨平台网络请求框架，旨在通过注解简化 RESTful API 的声明与实现。它支持客户端与服务端的代码自动生成，帮助开发者减少样板代码，提高开发效率。

## 依赖以及平台版本

| 平台或依赖库 | 版本           |
|--------|--------------|
| Kotlin | 2.2.10       |
| Ktor   | 3.2.3        |
| KSP    | 2.2.10-2.0.2 |
| JVM    | 21           |

## 核心特点

- **多平台支持**：支持 Android、iOS、watchOS、tvOS、Js、WasmJs、 macOS、Linux、Windows 等平台。
- **注解驱动**：通过注解定义 API 接口与服务端路由，使用 KSP 自动生成实现类与 Ktor 路由注册代码。
- **支持 WebSocket、Mock、Timeout、Header/Cookie/Path 等常见参数类型**
- **服务端授权支持**：内置 `@Authentication` 注解支持路由授权处理。

## 模块组成

### Kotlin Multiplatform 客户端模块

| 模块名                         | 功能           |
|-----------------------------|--------------|
| multiplatform-core          | 客户端核心功能      |
| multiplatform-annotation    | 注解声明         |
| multiplatform-websockets    | WebSocket 支持 |
| multiplatform-mock          | Mock 支持      |
| multiplatform-ksp           | 客户端 KSP 生成器  |
| multiplatform-gradle-plugin | Gradle 插件    |

### Ktor Server 服务端模块

| 模块名                  | 功能           |
|----------------------|--------------|
| server-core          | 服务端核心支持      |
| server-annotation    | 注解声明         |
| server-websockets    | WebSocket 支持 |
| server-auth          | 授权支持         |
| server-ksp           | 服务端 KSP 生成器  |
| server-gradle-plugin | Gradle 插件    |

### 通用模块

| 模块名             | 功能        |
|-----------------|-----------|
| common-ksp-util | 通用符号处理器工具 |

## 全面拥抱 3.x，从 2.x 迁移到 3.x

- 原 `cn.vividcode.multiplatform` 包名已迁移为 `cn.ktorfitx`，旧的依赖包已停止维护❗️
- 新增 Ktor Server 目标平台
- 支持更多的 KMP 平台

## 编译期校验

**Ktorfit X** 提供编译期间的错误校验支持，能够在使用错误的注解或结构时给出明确提示，帮助用户快速定位问题。

## 异常处理

- 当函数返回类型为 `Result<T>` 时，Ktorfit X 会自动捕获并封装网络异常；如果返回值为普通类型，则需自行处理异常逻辑。