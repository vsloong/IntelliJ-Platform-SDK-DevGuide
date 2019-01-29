# IntelliJ Platform 构件仓库
JetBrains维护一个托管与IntelliJ平台相关的构件公共仓库，例如二进制文件和源代码。这个仓库使得插件作者更容易访问到这些构件。

IntelliJ平台构件仓库包含：
- 发布版本和EAP版本的[仓库](https://www.jetbrains.com/intellij-repository/releases/)。  
- 用于EAP候选和每个分支最新EAP的快照[仓库](https://www.jetbrains.com/intellij-repository/snapshots/)。  

来自IntelliJ平台各个模块的依赖托管在bintray仓库中。但是如果使用IntelliJ平台仓库中的各个模块，则应该将指向该仓库的链接添加到 pom.xml/build.gradle 文件中，而不能直接使用这些构件。

## 如何使用IntelliJ平台构件仓库
仓库中的构件是通过向项目的build.gradle文件中添加信息来使用的。有关Gradle支持的更多信息，请参见[gradle-intellij-plugin]()。  

如果要手动设置依赖项，请看下面两种使用仓库的信息：  

- 指定构件相应的仓库URL。  
- 指定构件的Maven坐标。

## 添加仓库URL
将所需构件相应的URL添加到Maven或Gradle脚本中：  
- 对于发行版或EAP版本，请使用 https://www.jetbrains.com/intellij-repository/releases  
- 对于EAP候选快照，请使用 https://www.jetbrains.com/intellij-repository/snapshots  
- 对于IntelliJ平台中各个模块的依赖关系，请使用 https://jetbrains.bintray.com/intellij-third-party-dependencies  

## 指定构件
描述所需构件可以使用Maven坐标的方法：  
- IntelliJ平台构件的跨平台zip发行版：  
  - groupId = com.jetbrains.intellij.idea  
  - artifactId = ideaIC  
  - classifier = “”  
  - packaging = .zip  
- IntelliJ IDEA社区版：
  - groupId = com.jetbrains.intellij.idea  
  - artifactId = ideaIC  
  - classifier = sources  
  - packaging = jar  
- 来自IntelliJ平台的各个模块及其依赖项。例如，要指定 jps-model-serialization模块：  
  - groupId = com.jetbrains.intellij.platform
  - artifactId = jps-model-serialization
  - classifier = “”
  - packaging = jar

仓库URL中的每个工件都有多个可用版本。可以通过以下几种方式中的一种指定版本：  
- 构建分支指定为 ***BRANCH.BUILD[.FIX]*** 。例如，构建分支(如```141.233```)，或使用修复的构建分支(如```139.555.1```)  
- Release号指定为 ***MAJOR[.MINOR][.FIX]*** 。例如，14，或 ```14.1``` ，或```14.1.1```
- 将从其中生成下一个EAP/release 的构建分支的快照指定为 ***BRANCH-EAP-SNAPSHOT*** 。例如 ```141-EAP-SNAPSHOT```

## 示例
本节提供了几个使用Gradle脚本将仓库合并到build.gradle文件中的示例。每个示例都有为构件声明URL、Maven坐标和版本的说明。每个示例有两个部分：仓库和依赖部分。

### IntelliJ IDEA Edition 版的Gradle示例
这些片段演示了将IntelliJ快照构建合并到build.gradle文件中。

#### Repository 部分
这个片段通过URL为构件选择快照仓库。  
```
repositories {
    maven { url "https://www.jetbrains.com/intellij-repository/snapshots" }
}
```  

#### Dependencies 部分
仓库部分选择了快照后，依赖项部分指定所需的构件。
```
dependencies {
    idea_dep "com.jetbrains.intellij.idea:ideaIC:141-SNAPSHOT@zip"
}
```

其中 ```idea_dep``` 为这个自定义配置的命名。  

这里是经常用到的[build.gradle](https://github.com/shalupov/idea-cloudformation/blob/9007023afa187a1fb8b45c3ca66d5a51f86b795c/build.gradle)文件示例。

### IntelliJ IDEA项目中单个模块的Gradle示例

这些代码片段说明了如何将Intellij模块合并到build.gradle文件中。这个示例下所需的模块是 ```jps-model-serialization```。  

#### Repositories 部分
此代码段片段第一个URL是发布仓库，第二个URL是Intellij IDEA依赖仓库。因为这个示例选择了单个模块，而不是完整构建，所以第二个URL是必须的。
```
repositories {
	maven { url "https://www.jetbrains.com/intellij-repository/releases" }
	maven { url "https://jetbrains.bintray.com/intellij-third-party-dependencies" }
}
```
#### Dependencies 部分
此依赖项部分指定了所需的模块构件。
```
dependencies {
	compile "com.jetbrains.intellij.platform:jps-model-serialization:182.2949.4"
	compile "com.jetbrains.intellij.platform:jps-model-impl:182.2949.4"
}
```  
注意：  
- 这两个语句中的构件版本（182.2949.4）必须匹配。  
- 在本例中，```jps-model-serialization``` 声明了API，```jps-model-impl```提供了实现，因此两者都是必需的依赖项。
