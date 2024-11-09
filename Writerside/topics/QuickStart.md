# 快速开始

## 创建项目

本项目介绍建议使用 Kotlin Multiplatform 项目创建，单独使用也是没问题的，只要是 Gradle 项目且支持 Ktor

<procedure title="项目创建" id="kotlin-multiplatform-create">
    <step> 
        <a href="https://kmp.jetbrains.com">点击前往 Kotlin Multiplatform Wizard</a>
    </step>
    <step>
        根据需求填写 项目名 以及 包名 并选择需要使用的平台，IOS 推荐使用 Share UI
        <img src="kotlin_multiplatform_wizard.png" alt="Kotlin Multiplatform Wizard"/>
    </step>
    <step>
        使用 Fleet 或 IDEA 打开项目，目前推荐使用 Fleet 打开，可以运行所有平台
    </step>
</procedure>

## Gradle 构建配置

> 本项目采用 Kotlin Multiplatform 项目为例，所以教程中只有需要添加的配置而不是覆盖

- 第一步：libs.versions.toml 中添加以下依赖，注：不是覆盖哦

``` text
[versions]
kotlin = "2.0.21"
ksp = "2.0.21-1.0.26"
ktofitx = "3.0.1-2.0.0"

[libraries]
ktorfitx-ksp = { group = "cn.vividcode.multiplatform", name = "ktorfitx-ksp", version.ref = "ktofitx" }
ktorfitx-api = { group = "cn.vividcode.multiplatform", name = "ktorfitx-api", version.ref = "ktofitx" }
ktorfitx-annotation = { group = "cn.vividcode.multiplatform", name = "ktorfitx-annotation", version.ref = "ktofitx" }

[plugins]
kotlinx-serialization = { id = "org.jetbrains.kotlin.plugin.serialization", version.ref = "kotlin" }
ksp = { id = "com.google.devtools.ksp", version.ref = "ksp" }
```

- 第二步：在项目级 build.gradle.kts 中添加以下配置，注：kotlin 项目推荐使用 kts 作为 gradle 脚本语言

``` kotlin
plugins {
    alias(libs.plugins.kotlinx.serialization) apply false
    alias(libs.plugins.ksp) apply false
}
```

- 第三步：在模块级 build.gradle.kts 中添加以下配置，注：只要需要代码生成的模块均需要配置

``` kotlin
plugins {
    alias(libs.plugins.kotlinx.serialization)
    alias(libs.plugins.ksp)
}

kotlin {
    sourceSets {
        commonMain.dependencies {
            // 这里可以使用 api 来向上传递依赖，这样就不需要重复定义此处代码
            // 只需要在父模块中使用 implementation(projects.<module_name>) 依赖子模块
            implementation(libs.ktorfitx.api)
            implementation(libs.ktorfitx.annotation)
        }
        commonMain {
            // 配置生成代码目录
            kotlin.srcDir("build/generated/ksp/metadata/commonMain/kotlin") 
        }
    }
}

// 为 commonMain 源集的元数据添加依赖
dependencies {
    kspCommonMainMetadata(libs.ktorfitx.ksp)
}

// 确保所有 KotlinCompilationTask 的编译任务正确依赖于 kspCommonMainKotlinMetadata
// 避免在 KMP 项目中编译任务顺序问题
tasks.withType<KotlinCompilationTask<*>>().all {
    "kspCommonMainKotlinMetadata".also {
        if (this.name != it) dependsOn(it)
    }
}
```

- 配置完成好之后只需要同步项目即可

## 开始使用

项目具有作用在接口上，作用在方法上，作用在参数上 的注解，请查看详细文档