# 插件的依赖

您的插件可能依赖于其他插件的类，无论是绑定的、第三方的还是您自己的。为了了解这种依赖关系(例如Kotlin)，您可以执行以下步骤：
- 如果插件没有绑定，运行目标IDE的沙箱实例并安装插件。  
- 如果使用Gradle与Kotlin脚本一起构建插件，请在 ```intellij``` 块中使用 ```setPlugins()``` ，例如：  
```
intellij {
    plugins 'org.jetbrains.kotlin:1.3.11-release-IJ2018.3-1'
}
```

- 如果使用Gradle与Groovy脚本一起构建插件，则需要将依赖项添加到build.gradle中 ```intellij``` 块的 ```plugins``` 参数中，例如：

```
intellij {
    plugins 'org.jetbrains.kotlin:1.3.11-release-IJ2018.3-1'
}
```
如果您没有使用Gradle，那么将您所依赖的插件的jar包添加到 *IntelliJ Platform SDK* 的classpath中。为此，打开Project Structure对话框，选择正在使用的SDK，在Classpath选项卡中的点击 + 按钮，然后选择插件的jar文件。  
- 对于捆绑的插件，插件jar包位于主安装目录的 ```plugins/<pluginname>``` 或 ```plugins/<pluginname>/lib``` 中。  
- 对于非捆绑的插件，插件jar包位于IntelliJPlatformPluginSDK设置中指定为“SandboxHome”的目录下的 ```config/plugins/<pluginname>``` 或```config/plugins/<pluginname>/lib```中。  

![](http://www.jetbrains.org/intellij/sdk/docs/basics/plugin_structure/img/add_plugin_dependency.png)  

> **警告** 不要将插件JAR作为库添加：这样在运行时会导致失败，因为IntelliJ平台会加载依赖插件类的两个独立副本。  

- 在plugin.xml中添加一条 ```<depends>``` 标签，将您所依赖的插件的ID添加为标签的内容。例如：  

```xml
<depends>org.jetbrains.kotlin</depends>
```
要找出您所依赖的插件的ID，请在其jar中找到```META-INF/plugin.xml```文件，并检查 ```<id>``` 标签的内容。

例如，如果您正在开发一个为Java和Kotlin文件添加额外高亮显示的插件，则可以使用以下设置。您的主plugin.xml需要为Java定义一个注释器，并指定对Kotlin插件的可选依赖：  

```xml
<idea-plugin>
   ...
   <depends optional="true" config-file="withKotlin.xml">org.jetbrains.kotlin</depends>

   <extensions defaultExtensionNs="com.intellij">
      <annotator language="JAVA" implementationClass="com.example.MyJavaAnnotator"/>
   </extensions>
</idea-plugin>
```

然后，在与主plugin.xml文件相同的目录中创建一个名为withKotlin.xml的文件。在该文件中，您需要为Kotlin定义一个注释器：  

```xml
<idea-plugin>
   <extensions defaultExtensionNs="com.intellij">
      <annotator language="kotlin" implementationClass="com.example.MyKotlinAnnotator"/>
   </extensions>
</idea-plugin>
```

- 请参阅 ```plugins``` 属性[gradle-intellij-plugin: Configuration](https://github.com/JetBrains/gradle-intellij-plugin#configuration)以获得可使用的参数值。
