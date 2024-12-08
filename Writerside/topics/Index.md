# 项目介绍

## 最新版本

当前最新版本是：3.0.2-2.1.0

项目版本规则采用 ktor版本 + ktorfitx版本 规则，能够方便用户直接对比出使用的 ktor 版本

## 项目介绍

本项目是一个专为 Ktor Client 提供代码生成功能的 KSP 插件，并且适配了 Kotlin Multiplatform 项目，
同时也支持单独的 Kotlin JVM Desktop 和 Android 项目

通过在接口和方法上标注注解来自动生成对应的 Ktor 代码，
旨在减少手动编写样板代码的工作量，加速开发进程。该插件可以识别和解析指定的注解，根据注解内容生成符合 Ktor 应用程序架构的代码片段，
从而帮助开发者更高效地进行网络请求的实现

## 项目依赖

需要同时使用，否则项目将无法正常运行

`ktorfitx-ksp` 提供代码扫描检查以及代码生成的服务

> cn.vividcode.multiplatform:ktorfitx-ksp:$version

`ktorfitx-api` 提供 ktorfitx 的基础服务

> cn.vividcode.multiplatform:ktorfitx-api:$version

`ktorfitx-annotation` 提供注解能力

> cn.vividcode.multiplatform:ktorfitx-annotation:$version

## 项目技术

项目采用 **KotlinPoet** 代码生成 + **KSP** 符号处理器 + **Ktor** 网络请求框架 技术，实现接口实现代码的生成

## 项目特点

1. **提升工作效率**：减少项目模版代码的编写，项目中经常会有大量接口的请求，而不需要写 Ktor 大量的请求模版代码
2. **不影响程序运行性能**：Ktorfitx 是构建期通过代码生成的方式而不是运行时，所以不会影响程序运行性能
3. **多平台支持 (Kotlin Multiplatform)**：以下简称：KMP，现在支持 Android, IOS, Desktop (JVM), WasmJs 平台，如果想要单独使用同样能够满足你，
   不过我还是推荐你尝试一下 KMP 项目
4. **自定义Mock**：当后端为进行开发完成时，使用自定义Mock机制，可以模拟网络请求行为并进行数据的返回
5. **接口作用域配置**：配置不同作用域，可以访问不同的服务端配置
6. **编译期错误检查**：在项目构建阶段，借助 ksp 的 kspLogger 进行编译期错误提示，告诉用户错误点以及如何纠正

## 下一步

接下来请查看 **[](快速开始.md)** 文档来创建项目并使用它吧