# 使用Gradle开始开发插件  

将Gradle构建支持功能添加到IntelliJ平台需要使用到最近发布的Gradle构建系统和IntelliJ IDEA(Community或者Ultimate版)。

## 1.0 下载安装IntelliJ IDEA
下载并安装IntelliJ IDEA Ultimate版或者IntelliJ IDEA Community Edition版。

## 1.1 确保启用“Gradle”插件和“Plugin DevKit”插件  
您可以点击 Settings | Plugins 来验证插件是否启用。  

![插件](http://www.jetbrains.org/intellij/sdk/docs/tutorials/build_system/img/step0_gradle_enabled.png)  

## 1.2 创建插件项目  
IntelliJ IDEA支持使用Gradle自动创建新的插件项目，并自动执行build.gradle中所有必要的设置。这也可以用于将现有插件转换为Gradle，如果Gradle在本例中无法转换现有项目，您需要将源码复制到新项目中。  

要做到这一点，需要在IntelliJ IDEA中创建一个新项目，打开 **File | New… | Project**, 并在弹出的对话框中选择Gradle。并在“Additional Libraries and Frameworks”一栏中，勾选 “IntelliJ Platform Plugin”。

![](http://www.jetbrains.org/intellij/sdk/docs/tutorials/build_system/img/step1_new_gradle_project.png)  

项目创建向导现在将指导您一步步完成项目创建的过程。您需要指定组ID、伪ID和版本：  

![](http://www.jetbrains.org/intellij/sdk/docs/tutorials/build_system/img/step2_group_artifact_version.png)  

建议选择```Use default gradle wrapper```选项，这样IntelliJ IDEA将自动安装运行Gradle任务所需的一切东西。  

最后，还需要指定一个将要使用的JVM Gradle，它可以是Project JDK。当项目创建后您还可以通过点击 **Settings | Build, Execution, Deployment | Build Tools | Gradle** 来配置该路径。

![](http://www.jetbrains.org/intellij/sdk/docs/tutorials/build_system/img/step3_gradle_config.png)  

## 1.3 配置Gradle插件项目  
IntelliJ平台提供了对基于Gradle插件项目的支持```gradle-intellij-plugin```。有关更多信息，请参考[Gradle plugin README](https://github.com/JetBrains/gradle-intellij-plugin/blob/master/README.md#gradle)。例如，要配置 **Sandbox Home** 目录的位置，在项目的 ```build.gradle``` 文件中包含以下内容：  

```
intellij {
  sandboxDirectory = "$project.buildDir/myCustom-sandbox"
}
```
有关Sandbox Home默认主目录位置和内容的详细信息，请参阅[IDE开发实例]()。

## 1.4 在现有插件中添加Gradle支持
要将Gradle支持添加到现有的插件项目中，请在根目录下创建 ```build.gradle``` 文件，其中至少包含以下内容：

```gradle
buildscript {
    repositories {
        mavenCentral()
    }
}

plugins {
    id "org.jetbrains.intellij" version "0.3.0"
}

apply plugin: 'idea'
apply plugin: 'org.jetbrains.intellij'
apply plugin: 'java'

intellij {
    version 'IC-2016.3' //IntelliJ IDEA 2016.3 dependency; for a full list of IntelliJ IDEA releases please see https://www.jetbrains.com/intellij-repository/releases
    plugins 'coverage' //Bundled plugin dependencies
    pluginName 'plugin_name_goes_here'
}

group 'org.jetbrains'
version '1.2' // Plugin version
```

然后，使用系统路径上配置的Gradle可执行文件，在命令行窗口执行以下命令：

```
gradle cleanIdea idea
```

这将清理任何现有的IntelliJ IDEA配置文件，并生成一个由IntelliJ IDEA识别的新的Gradle构建配置文件。刷新您的项目，然后点击 **View | Tool Windows | Gradle** 查看到Gradle工具窗口。可以看到IntelliJ IDEA已经识别到了Gradle。  

## 1.5 运行一个简单的插件
现在将一个新的 ```HelloAction``` 类添加到Java文件夹中，并在 ```META-INF``` 文件夹中添加 ```plugin.xml``` 和 ```pluginIcon.svg``` 文件。有关 ```pluginIcon.svg``` 文件的更多信息，请参见[Plugin Icon]()页面。  

![](http://www.jetbrains.org/intellij/sdk/docs/tutorials/build_system/img/gradle_directory_structure.png)  

```java
import com.intellij.openapi.actionSystem.*;
import com.intellij.openapi.project.Project;
import com.intellij.openapi.ui.Messages;

public class HelloAction extends AnAction {
  public HelloAction() {
    super("Hello");
  }

  public void actionPerformed(AnActionEvent event) {
    Project project = event.getProject();
    Messages.showMessageDialog(project, "Hello world!", "Greeting", Messages.getInformationIcon());
  }
}
```  

```xml
<idea-plugin>
  <id>org.jetbrains</id>
  <name>Hello Action Project</name>
  <version>0.0.1</version>
  <vendor email="dummy" url="dummy">dummy</vendor>

  <depends>com.intellij.modules.lang</depends>

  <extensions defaultExtensionNs="com.intellij">
  </extensions>

  <actions>
    <group id="MyPlugin.SampleMenu" text="Greeting" description="Greeting menu">
      <add-to-group group-id="MainMenu" anchor="last"/>
      <action id="Myplugin.Textboxes" class="HelloAction" text="Hello" description="Says hello"/>
    </group>
  </actions>

</idea-plugin>
```

打开Gradle工具窗口并搜索 ```runIde``` 任务。如果列表中没有该选项，请点击顶部的刷新按钮，双击来运行它。  

![](http://www.jetbrains.org/intellij/sdk/docs/tutorials/build_system/img/gradle_tasks_in_tool_window.png)  

或者添加一个新的Gradle运行配置，配置如下：  

![](http://www.jetbrains.org/intellij/sdk/docs/tutorials/build_system/img/gradle_run_config.png)  

启动新的Gradle运行配置。在运行窗口中，可以看到类似下面的输出信息。  

![](http://www.jetbrains.org/intellij/sdk/docs/tutorials/build_system/img/launched.png)  

最后，当IDE启动时，帮助菜单的右边应该有一个新的菜单。您的插件现在已经通过Gradle配置好了。
