# 搭建开发环境
## 步骤
使用下面的步骤列表来确保您已经准备好开发您的自定义插件。

- 在您的计算机上下载IntelliJ IDEA CE源代码。IntelliJ IDEA CE源码并不是插件开发的必要条件，但是使用它可以使的插件的调试更加容易。有关详细说明，请参阅 *Getting IntelliJ IDEA Community Edition Source Code*  部分的[Check Out And Build Community Edition](https://github.com/JetBrains/intellij-community/blob/master/README.md)。请注意，下载源码这一步并不是开发插件所必需的。  
- 必须在IntelliJ IDEA中启用DevKit插件。  
- 必须为您的IDEA配置IntelliJ Platform SDK。有关更多信息，请参见下面。

## 配置IntelliJ平台SDK
为配置插件开发环境，请执行以下操作：  
- 单击 **File | Project Structure** ，在打开的窗口下创建新的 * IntelliJ Platform SDK* ：
![](http://www.jetbrains.org/intellij/sdk/docs/basics/getting_started/img/create_intellij_idea_sdk.png)  

- 指定 *IntelliJ IDEA Community Edition* 的安装文件夹作为主目录。
> 警告： 您可以使用 IntelliJ IDEA Ultimate 作为替代方案，但只能用Community Edition版本调试核心代码。

![](http://www.jetbrains.org/intellij/sdk/docs/basics/getting_started/img/set_home_directory.png)  

选择1.8作为默认的Java SDK版本。有关创建1.8版本的JSDK说明，请参阅IntelliJ构建配置的 [Check Out And Build Community Edition](```Check Out And Build Community Edition``) 部分。
![](http://www.jetbrains.org/intellij/sdk/docs/basics/getting_started/img/set_java_sdk.png)

- 在SDK设置的Sourcepath选项中，单击*Add*按钮：
![](http://www.jetbrains.org/intellij/sdk/docs/basics/getting_started/img/add_sourcepath.png)

指定IntelliJ IDEA Community Edition版的源代码目录：
![](http://www.jetbrains.org/intellij/sdk/docs/basics/getting_started/img/community_sources_directory.png)

- 指定SandboxHome目录。
Sandbox Home目录存储了IDE开发实例的设置。下面显示的是MacOSX上用户默认的 **Sandbox Home** 目录。任何目录都可以作为 *Sandbox Home* 的路径。可以使用省略号按钮(如下所示)自定义位置。  

有关默认 *Sandbox Home* 主目录位置和内容的详细信息，请参阅[IDE Development Instances]()。

![](http://www.jetbrains.org/intellij/sdk/docs/basics/getting_started/img/plugins-sandbox.png)
