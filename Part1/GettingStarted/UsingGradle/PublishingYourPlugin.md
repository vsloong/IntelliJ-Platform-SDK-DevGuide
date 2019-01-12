# 使用Gradle发布插件
只要配置了Gradle支持，您就可以自动构建插件并将其部署到[JetBrains Plugin Repository](http://plugins.jetbrains.com/)。要做到这一点，需要您先将插件发布到插件仓库。有关详细信息，请参阅[publishing a plugin](http://www.jetbrains.org/intellij/sdk/docs/basics/getting_started/publishing_plugin.html)。

## 2.0添加您的账户凭证
为了将插件提交到插件仓库，首先需要您提供JetBrains帐户凭证。这些凭证信息通常存储在[Gradle properties](https://docs.gradle.org/current/userguide/build_environment.html#sec:gradle_configuration_properties)。所以千万不要将这些凭证添加到源代码提交信息中。  

将以下信息放在项目根目录下名为 ```gradle.properties``` 的文件中，或者放在 ```GRADLE_HOME/gradle.properties``` 中。

```gradle
intellijPublishUsername="YOUR_USERNAME_HERE"
intellijPublishPassword="YOUR_PASSWORD_HERE"
```

然后在 ```build.gradle``` 文件中的 ```publishPlugin``` 任务中引用这些值：  
```
publishPlugin {
  username intellijPublishUsername
  password intellijPublishPassword
}
```

如果将 ```gradle.properties``` 文件放置在项目的根目录中，请确保版本控制工具忽略此文件。例如在Git中，可以将以下行添加到 ```.gitignore``` 文件中：
```
gradle.properties
```

或者，您可以创建一个默认的 ```gradle.properties``` 模板，并默认 ```git``` 忽略将来对该文件的任何更改。为此，请检查默认模板并执行以下命令：
```
git update-index --assume-unchanged gradle.properties
```

如果您的项目已经有一个 ```gradle.properties``` 文件，您可以创建一个自定义 ```*.properties``` 文件，并手动加载它。例如：
```
apply from: "/path/to/custom.properties"
```

## 2.1配置插件
Gradle-IntelliJ-plugin为如何使用Gradle构建您的插件提供了许多[configuration options](configuration options)(配置选项)。其中一个最重要的是 ```version（版本）``` 。 默认情况下，如果在构建脚本中修改 ```version``` ，Gradle插件将自动更新 ```plugin.xml``` 文件中的 ```<version>``` 。

Gradle插件还将更新 ```plugin.xml``` 文件中的 ```<idea-version since-build=.../>``` ，以与 ```intellij.version``` 匹配，直到当前主版本中的最后一个版本为止。当然，您也可以通过将 ```intellij.updateSinceUntilBuild``` 选项设置为 ```false``` 来禁用此功能。

```gradle
apply plugin: 'org.jetbrains.intellij'

intellij {
    version '15.0.1'
    pluginName 'idear'
    intellij.updateSinceUntilBuild false //Disables updating since-build attribute in plugin.xml
}

group 'com.jetbrains'
version '1.2' // Update me!
```

当您使用包含上述代码段的构建脚本运行 ```Gradle runIdea``` 时，Gradle将从[Snapshot](https://www.jetbrains.com/intellij-repository/snapshots)(基于时间的)或[Release](https://www.jetbrains.com/idea/help/managing-plugins.html)(基于版本)的仓库下载IntelliJ IDEA的适当版本，配置插件沙箱，安装插件，并启动新的IDE。这个任务可以直接从命令行运行，而不需要任何其他工具帮助。  

为了获得最稳定的效果，插件开发人员应该使用固定的版本，而不是 ```LATEST-TRUNK-SNAPSHOT（最新版本）``` 。有关IntelliJ平台的可用版本信息，您可以参考以下URL获取最新的更新：  
- Releases: https://www.jetbrains.com/intellij-repository/releases  

- Snapshots: https://www.jetbrains.com/intellij-repository/snapshots
