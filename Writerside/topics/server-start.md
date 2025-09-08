# 快速开始

- 在这里会介绍如何在 Kotlin Server 项目中启用 Ktorfit X，从而在项目中使用 Ktorfit X 的全部能力，从而加速开发 App

## 配置构建

- 在模块级的 build.gradle.kts 中添加以下代码

```kotlin
plugins {
	// 省略其他...
	// 添加 cn.ktorfitx.server 插件
	id("cn.ktorfitx.server") version "<latest>"
}

/**
 * 使用 插件提供的 ktorfitx DSL 配置
 */
ktorfitx {
	// 将所有提示文本改为中文，默认：ENGLISH，支持：ENGLISH, CHINESE
	language = KtorfitxLanguage.CHINESE
	
	websockets {
		enabled = true  // 启用 WebSockets 功能，默认：false
	}
	
	auth {
		enabled = true  // 启用授权功能，默认：false
	}
	
	generate {
		this.packageName = "<package name>" // 生成文件目录，默认：当前模块包 + .generated
		this.funName = "<function name>"    // 生成方法名，默认：generateRoutes
		this.fileName = "<filename>"        // 生成文件名，默认：GenerateRoutes，可以不加 .kt 后缀
	}
}
```