# 插件开发者如何使用Kotlin

## 1.为什么使用Kotlin ？
使用Kotlin为IntelliJ平台编写插件与用Java编写插件非常相似。现有插件开发者可以通过使用绑定到IntelliJ平台(版本143.+)的J2K编译器将样板Java类转换为与它们等效的Kotlin类，开发者可以轻松地将Kotlin类与他们现有的Java代码匹配和混合。  

除了[null safety](https://kotlinlang.org/docs/reference/null-safety.html)和[type-safe builders](https://kotlinlang.org/docs/reference/type-safe-builders.html)之外，Kotlin语言还为插件开发提供了许多方便的特性，使插件更易于阅读和维护。与[Kotlin for Android](https://kotlinlang.org/docs/tutorials/kotlin-android.html)非常相似，IntelliJ平台广泛使用回调，这在Kotlin中很容易使用lambdas来解决。

同样，在IntelliJ IDEA中使用[扩展](https://kotlinlang.org/docs/reference/extensions.html)很容易定制内部类的行为。例如，通常保护日志记录的语句，以避免参数构造的成本，从而导致在使用日志时会使用以下方式：

```java
if(logger.isDebugEnabled()) {
    logger.debug("...");
}
```  

通过声明以下扩展方法，我们可以在Kotlin中更简洁地获得相同的结果：

```java
inline fun Logger.debug(lazyMessage: () -> String) {
  if (isDebugEnabled) {
    debug(lazyMessage())
  }
}
```  
现在，我们可以直接编写 ```logger.debug { "..." }``` 来打印轻量级日志记录，而不需要冗长的内容。通过实践，您将能够了解到IntelliJ平台中许多可以用Kotlin简化的语法。要了解有关使用Kotlin构建IntelliJ平台插件的更多信息，本教程将帮助您入门。

## 2.添加Kotlin支持
针对IntelliJ平台143及以上版本的插件很容易迁移：只需开始编写Kotlin。必需的Kotlin插件和库已经与IDE捆绑在一起，不需要进一步配置。对于142或更低版本，您需要安装和配置Kotlin运行时的依赖项(除了安装Kotlin插件本身，还需获得代码辅助和工具支持)。有关详细说明，请参考[Kotlin documentation](https://kotlinlang.org/docs/tutorials/getting-started.html)。   

## 3.Kotlin Gradle插件
对于已经使用了Gradle构建系统的插件，或者那些需要对Kotlin构建过程进行精确控制的插件，我们建议使用[kotlin-gradle-plugin](https://kotlinlang.org/docs/reference/using-gradle.html#configuring-dependencies)。这个[Gradle插件](https://mvnrepository.com/artifact/org.jetbrains.kotlin/kotlin-gradle-plugin-core)使得以可控制和可复制的方式构建Kotlin项目极大地简化了。  

你的 ```build.gradle``` 文件可能看起来像下面这个样子：
```java
buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
    }
}

apply plugin: 'org.jetbrains.intellij'
apply plugin: 'kotlin'

intellij {
    version 'LATEST-TRUNK-SNAPSHOT'
    pluginName 'Example'
}

group 'com.example'
version '0.0.1'

repositories {
    mavenCentral()
}
```
请注意，您不应该在插件中包含```kotlin-runtime```和```kotlin-stdlib``` jar包，因为kotlin保证了向后和向前的兼容性。  

### 3.1使用Kotlin编写gradle脚本
在gradle4.4版本后，gradle开始支持```build.gradle.kts```，这是用kotlin编写的```build.gradle```的替代方案。  

有许多好的使用Kotlin脚本来为IntelliJ插件编写构建脚本的源代码，例如 [intellij-rust](https://github.com/intellij-rust/intellij-rust/blob/master/build.gradle.kts), [julia-intellij](https://github.com/ice1000/julia-intellij/blob/master/build.gradle.kts), [covscript-intellij](https://github.com/covscript/covscript-intellij/blob/master/build.gradle.kts) 或者 [zig-intellij](https://github.com/ice1000/zig-intellij/blob/master/build.gradle.kts)。  

脚本 ```build.gradle.kts``` 基本上类似这样：

```java
group = "com.your.company.name"
version = "0.1-SNAPSHOT"

buildscript {
    repositories { mavenCentral() }
    dependencies { classpath(kotlin("gradle-plugin", "1.2.30")) }
}

plugins {
	id("org.jetbrains.intellij") version "0.2.18"
	kotlin("jvm") version "1.2.30"
}

intellij {
    updateSinceUntilBuild = false
    instrumentCode = true
    version = "2017.3"
}
```  

## 4.Kotlin的UI
使用Kotlin创建用户界面的最佳方法是使用类型安全的DSL来构建表单，而不是GUI设计器。IntelliJ平台中使用的DSL位于```com.intellij.ui.layout```包中。[参考文档](https://github.com/JetBrains/intellij-community/blob/master/platform/platform-impl/src/com/intellij/ui/layout/readme.md)。  

## 5.处理Kotlin代码
如果需要编写处理Kotlin代码的插件，则需要添加对Kotlin插件的依赖。有关如何做到这一点的信息，请参考插件依赖章节。

## 6.示例
有许多建立在IntelliJ平台上的开源Kotlin项目。关于使用Kotlin语言为IntelliJ平台构建开发工具的最新示例和应用程序，开发人员可以从这些项目中寻找灵感：  

- [intellij-rust](https://github.com/intellij-rust/intellij-rust)
- [haskell-idea-plugin](https://github.com/atsky/haskell-idea-plugin)
- [intellij-markdown](https://github.com/valich/intellij-markdown)
- [IntelliJ-presentation-assistant](https://github.com/chashnikov/IntelliJ-presentation-assistant)
- [leanpub-plugin](https://github.com/hhariri/leanpub-plugin)
