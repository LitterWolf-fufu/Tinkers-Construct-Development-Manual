## 拉取 Maven 依赖到 MDK 工作区

### 1. 添加 Maven 仓库

在 `build.gradle` 中声明仓库来源:

```gradle
repositories {
	maven {
		name 'DVS1 Maven FS'
		url 'https://dvs1.progwml6.com/files/maven'
	}
}
```

---

### 2. 添加依赖项

在 `dependencies` 中引入所需库:

```gradle
dependencies {
	// Forge 环境
	implementation fg.deobf("slimeknights.mantle:Mantle:${minecraft_version}-${mantle_build}")
	implementation fg.deobf("slimeknights.tconstruct:TConstruct:${minecraft_version}-${tinkers_build}")
}
```

---

### 说明

* `fg.deobf(...)`
  用于在开发环境中引入**反混淆后的依赖**, 便于直接调用其 API

* `${minecraft_version}` / `${mantle_build}` / `${tinkers_build}`
  建议统一在 `gradle.properties` 中定义, 便于集中管理版本号

* 该仓库主要用于提供 **Mantle** 与 **Tinkers' Construct** 相关依赖

---

# [下一篇](/java/docs/定义注册Modifier.md)