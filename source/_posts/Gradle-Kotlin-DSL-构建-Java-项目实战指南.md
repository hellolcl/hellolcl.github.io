---
title: Gradle Kotlin DSL 构建 Java 项目实战指南
date: 2026-04-03 10:29:16
tags:
  - Java
  - Gradle
  - Kotlin DSL
  - 构建工具
categories:
  - Java
---

## 为什么选择 Gradle Kotlin DSL

传统的 Gradle 构建脚本使用 Groovy 编写（`build.gradle`），虽然灵活，但缺少 IDE 的类型检查和代码补全支持。Gradle Kotlin DSL（`build.gradle.kts`）用 Kotlin 语言替代 Groovy，带来以下优势：

- 类型安全：编译期就能发现配置错误
- IDE 支持：IntelliJ IDEA 对 Kotlin DSL 提供完整的代码补全和跳转
- 更好的重构能力：重命名、提取变量等操作一应俱全

如果你是 Java 开发者，不需要会 Kotlin，掌握几个基础语法就能上手。

## 创建项目

### 方式一：命令行初始化

```bash
mkdir my-project && cd my-project
gradle init
```

选择 `Application` → `Java` → `Kotlin` 作为 DSL 语言，会自动生成基础项目结构。

### 方式二：Spring Initializr

访问 https://start.spring.io ，选择 Gradle - Kotlin 作为构建工具，生成项目即可。

## 项目结构

```
my-project/
├── build.gradle.kts       # 构建脚本
├── settings.gradle.kts    # 项目设置
├── gradle.properties      # Gradle 属性配置
└── src/
    ├── main/java/         # Java 源码
    └── test/java/         # 测试代码
```

## 核心配置详解

### 基础的 build.gradle.kts

```kotlin
plugins {
    java
    application
}

group = "com.example"
version = "1.0.0"

java {
    sourceCompatibility = JavaVersion.VERSION_17
    targetCompatibility = JavaVersion.VERSION_17
}

repositories {
    mavenCentral()
}

dependencies {
    // 编译时依赖
    implementation("org.springframework.boot:spring-boot-starter-web:3.2.0")
    // 测试依赖
    testImplementation("org.junit.jupiter:junit-jupiter:5.10.0")
}

tasks.test {
    useJUnitPlatform()
}

application {
    mainClass.set("com.example.Main")
}
```

### 从 Groovy 迁移的关键差异

**字符串插值**

```kotlin
// Groovy
version = "${project.version}"

// Kotlin DSL
version = "${project.version}"
// 注意：Kotlin 中 $project 也可以，但 ${} 更安全
```

**赋值操作**

```kotlin
// Groovy
sourceCompatibility = '17'

// Kotlin DSL
java {
    sourceCompatibility = JavaVersion.VERSION_17
}
```

**task 配置**

```kotlin
// Groovy
test {
    useJUnitPlatform()
}

// Kotlin DSL
tasks.test {
    useJUnitPlatform()
}
```

注意 Kotlin DSL 中访问 task 需要 `tasks.xxx` 的语法。

## 依赖管理

### 声明依赖

```kotlin
dependencies {
    // 核心依赖：编译和运行时都需要
    implementation("com.google.guava:guava:33.0.0-jre")

    // 编译期依赖：不会被打包
    compileOnly("org.projectlombok:lombok:1.18.30")

    // 注解处理器
    annotationProcessor("org.projectlombok:lombok:1.18.30")

    // 运行时依赖：编译时不需要
    runtimeOnly("com.mysql:mysql-connector-j:8.2.0")

    // 测试依赖
    testImplementation("org.springframework.boot:spring-boot-starter-test:3.2.0")
}
```

### 使用版本目录（Version Catalog）

在 `gradle/libs.versions.toml` 中统一管理版本：

```toml
[versions]
spring-boot = "3.2.0"
mybatis-flex = "1.7.5"

[libraries]
spring-boot-starter-web = { module = "org.springframework.boot:spring-boot-starter-web", version.ref = "spring-boot" }
mybatis-flex-spring-boot = { module = "com.mybatis-flex:mybatis-flex-spring-boot-starter", version.ref = "mybatis-flex" }

[plugins]
spring-boot = { id = "org.springframework.boot", version.ref = "spring-boot" }
```

在 `build.gradle.kts` 中引用：

```kotlin
dependencies {
    implementation(libs.spring.boot.starter.web)
    implementation(libs.mybatis.flex.spring.boot)
}
```

## 多模块项目

### settings.gradle.kts

```kotlin
rootProject.name = "my-project"

include(
    "app",
    "common",
    "service-user",
    "service-order"
)
```

### 父项目的 build.gradle.kts

```kotlin
plugins {
    java
    id("org.springframework.boot") version "3.2.0" apply false
    id("io.spring.dependency-management") version "1.1.4" apply false
}

subprojects {
    apply(plugin = "java")
    apply(plugin = "io.spring.dependency-management")

    group = "com.example"
    version = "1.0.0"

    java {
        sourceCompatibility = JavaVersion.VERSION_17
    }

    repositories {
        mavenCentral()
    }

    dependencyManagement {
        imports {
            mavenBom(org.springframework.boot.gradle.plugin.SpringBootPlugin.BOM_COORDINATES)
        }
    }
}
```

### 子模块的 build.gradle.kts

```kotlin
dependencies {
    implementation(project(":common"))
    implementation("org.springframework.boot:spring-boot-starter-web")
}
```

## 自定义 Task

```kotlin
tasks.register("printProjectInfo") {
    group = "help"
    description = "打印项目信息"
    doLast {
        println("项目名称: ${project.name}")
        println("项目版本: ${project.version}")
        println("Java 版本: ${project.java.sourceCompatibility}")
    }
}
```

## 常用构建命令

```bash
# 编译项目
./gradlew build

# 跳过测试构建
./gradlew build -x test

# 运行项目
./gradlew bootRun

# 查看依赖树
./gradlew dependencies

# 清理构建产物
./gradlew clean

# 刷新依赖（解决缓存问题）
./gradlew build --refresh-dependencies
```

## 常见问题

### 1. IDE 同步失败

Kotlin DSL 的 IDE 同步比 Groovy 慢，这是正常的。如果频繁失败，尝试：

```bash
# 关闭 Gradle Daemon 后重新构建
./gradlew --stop
./gradlew build
```

### 2. 插件不兼容

部分老旧插件对 Kotlin DSL 的支持不完善。遇到这种情况，可以在 `settings.gradle.kts` 中通过 pluginManagement 指定插件仓库：

```kotlin
pluginManagement {
    repositories {
        gradlePluginPortal()
        mavenCentral()
    }
}
```

### 3. 构建速度慢

在 `gradle.properties` 中开启构建缓存：

```properties
org.gradle.caching=true
org.gradle.parallel=true
org.gradle.jvmargs=-Xmx1024m
```

## 总结

Gradle Kotlin DSL 是 Java 项目构建工具的现代化选择。它带来的类型安全和 IDE 支持在日常开发中能省下不少时间。对于新项目，建议直接使用 Kotlin DSL。老项目如果没有特殊需求，也不必强行迁移，能跑就别动。
