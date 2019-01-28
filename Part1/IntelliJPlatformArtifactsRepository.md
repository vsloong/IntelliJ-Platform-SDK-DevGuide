# IntelliJ Platform 仓库
JetBrains维护一个托管与IntelliJ平台相关的文件公共仓库，例如二进制文件和源代码。这个仓库使得插件作者更容易访问到这些文件。

IntelliJ平台仓库包含：
- 发布版本和EAP版本的[仓库](https://www.jetbrains.com/intellij-repository/releases/)。  
- 用于EAP候选和每个分支最新EAP的快照[仓库](https://www.jetbrains.com/intellij-repository/snapshots/)。  

来自IntelliJ平台各个模块的依赖托管在bintray仓库中。但是如果使用IntelliJ平台仓库中的各个模块，则应该将指向该仓库的链接添加到 pom.xml/build.gradle 文件中，而不能直接使用这些构件。
