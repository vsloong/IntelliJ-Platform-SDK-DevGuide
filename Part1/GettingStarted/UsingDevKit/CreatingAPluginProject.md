# 创建插件工程

本节将说明如何使用New Project向导从头创建一个新的插件项目。通常，您可以选择导入现有项目或导入外部模块项目。您还可以向现有的 *IntelliJ Platform* 项目添加一个新的插件模块。有关详细信息，请参阅[IntelliJ IDEA Web Help](https://www.jetbrains.com/idea/help/new-project-wizard.html)。  

## 创建IntelliJ平台插件项目

- 在主菜单上，选择 **File | New | Project** ，然后 *New Project* 向导将会启动。

![](http://www.jetbrains.org/intellij/sdk/docs/basics/getting_started/img/new_project_wizard.png)  

- 设置 * IntelliJ Platform Plugin* 项目类型  
- 单击 **Next**  
- 设置所需的项目名称  
- 单击 **Finish** 生成项目结构文件  
- 如果需要自定义项目设置，请打开 **File | Project Structure**


## 创建IntelliJ平台插件模块

- 选择 **File | New | Module** ，然后选择 *IntelliJ Platform Plugin* 模块类型。  

![](http://www.jetbrains.org/intellij/sdk/docs/basics/getting_started/img/intellij_platform_plugin_module.png)  

- 输入您想要的插件名。  
- 转到 **File | Project Structure** ，选择新创建的 *IntelliJ Platform SDK* 作为插件模块的默认SDK：  

![](http://www.jetbrains.org/intellij/sdk/docs/basics/getting_started/img/set_plugin_module_sdk.png)  
