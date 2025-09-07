# 快速开始

- 在这里会介绍如何在 Kotlin Multiplatform 项目中启用 Ktorfit X，从而在项目中使用 Ktorfit X 的全部能力，从而加速开发 App

## 配置构建

- 在模块级的 build.gradle.kts 中添加以下代码

```kotlin
plugins {
	// 省略其他...
	// 添加 cn.ktorfitx.multiplatform 插件
	id("cn.ktorfitx.multiplatform") version "<latest>"
}

/**
 * 使用 插件提供的 ktorfitx DSL 配置
 */
ktorfitx {
	websockets {
		enabled = true  // 启用 WebSockets 功能，默认关闭
	}
	
	mock {
		enabled = true  // 启用 Mock 功能，默认关闭
	}
	
	ksp {
		kspGeneratedSrcDir = "<custom>" // 默认："build/generated/ksp/metadata/commonMain/kotlin"
	}
    
    // 配置语言，默认：ENGLISH，支持 ENGLISH 和 CHINESE
    language = KtorfitxLanguage.CHINESE
}
```