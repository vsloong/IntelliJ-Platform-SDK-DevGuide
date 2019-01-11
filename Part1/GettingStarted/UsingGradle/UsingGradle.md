# 使用Gradle构建插件
该[gradle-intellij-plugin](https://github.com/JetBrains/gradle-intellij-plugin)Gradle插件是构建IntelliJ插件的推荐解决方案。该插件可以处理插件项目的依赖关系-包括基本的IDE和可能依赖其他插件的插件。它还提供了在IDE中运行插件和将插件发布到JetBrains插件仓库的功能。为了确保您的插件不受平台主要版本间可能发生的API更改情况的影响，您可以使用Gradle轻松地针对多个版本的基本IDE构建插件。  

下面的教程引用了包含在[gradle_plugin_](https://github.com/JetBrains/intellij-sdk-docs/tree/master/code_samples/gradle_plugin_demo)演示项目中的资料。  

- [开始开发](GettingStartedWithGradle.md)  
- [部署插件](PublishingYourPlugin.md)
