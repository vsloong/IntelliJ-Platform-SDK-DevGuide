# 部署插件

在使用自定义插件之前，需要先通过：构建、安装，然后使用插件管理器启用来部署。  

部署插件：  
- 通过调用 **Build | Build Project** 或 **Build | Build Module <module name>** 来创建您的项目。  
- 准备部署插件。在主菜单中选择 **Build | Prepare Plugin Module <module name> for Deployment** 。  

![](http://www.jetbrains.org/intellij/sdk/docs/basics/getting_started/deploying_plugin/img/prepare_plugin_for_deployment.png)  

- 如果该插件模块不依赖于任何第三方库，那么将创建一个.jar的插件包。否则，将创建一个.zip的压缩文件，它包括项目设置中指定的所有插件库。  

![](http://www.jetbrains.org/intellij/sdk/docs/basics/getting_started/deploying_plugin/img/jar_saved_notification.png)  

- 从磁盘安装新创建的压缩文件或者jar文件。下文的 ```editor_basics``` 代码示例将压缩文件/jar文件的插件导入到 ```editor_basics``` 项目文件夹中：  

![](http://www.jetbrains.org/intellij/sdk/docs/basics/getting_started/deploying_plugin/img/jar_location.png)  

- 重新启动IDE，使得插件生效。
