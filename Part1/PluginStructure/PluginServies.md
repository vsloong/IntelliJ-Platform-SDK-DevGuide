# 插件的Services
*IntelliJ Platform* 提供了 *services* 的概念。  

当您的插件调用[ServiceManager]()类的 ```getService``` 方法时，*service* 插件组件就会按需加载。  

即使服务被多次调用，*IntelliJ Platform* 也会确保只加载一个服务实例。service必须具有用于service实例化的实现类。service还可以具有接口类，该接口类用于获取service实例并提供service的API。该接口和实现类在 ```plugin.xml``` 文件中。  

*IntelliJ Platform* 提供三种类型的服务：应用层（*application level*）服务、项目层（*project level*）服务和模块层（*module level*）服务。

## 如何声明Service？
要声明service，可以在IDEA核心版中使用以下扩展点：  
- ```applicationService```：用于声明应用层级别的服务。  
- ```projectService```：用于声明项目层级别的服务。  
- ```moduleService```：用于声明模块层级别的服务。  

### 声明service
- 在项目中，打开目标包的上下文菜单，然后单击 *New* （或者按下```Alt``` + ```Insert``` 快捷键）。  
- 在 *New* 菜单中，选择 *Plugin DevKit* ，并根据需要使用的服务类型，单击 *Application Service* 、*Project Service* 或 *Module Service*。  
- 在打开的对话框中，可以指定服务接口和实现，如果取消选中 *Separate interface from implementation* 复选框，则只会指定服务类。  

IDE将生成新的Java接口和类(如果未选中 *Separate interface from implementation* 复选框，则只生成类)，并将新服务注册到 ```plugin.xml``` 文件中。  

> 注意： 要通过 *New* 上下文菜单声明servie，IDE版本需高于2017.3。  

为了了解service声明过程，请参考```plugin.xml```文件的以下片段：
```xml
<extensions defaultExtensionNs="com.intellij">
  <!-- Declare the application level service -->
  <applicationService serviceInterface="Mypackage.MyApplicationService" serviceImplementation="Mypackage.MyApplicationServiceImpl" />

  <!-- Declare the project level service -->
  <projectService serviceInterface="Mypackage.MyProjectService" serviceImplementation="Mypackage.MyProjectServiceImpl" />
</extensions>
```
如果没有指定 ```serviceInterface``` ，那么它应该具有与 ```serviceImplementation``` 相同的值。  

## 获取Service
要实例化服务，在Java代码中使用以下语法：
```java
MyApplicationService applicationService = ServiceManager.getService(MyApplicationService.class);

MyProjectService projectService = ServiceManager.getService(project, MyProjectService.class);

MyModuleService moduleService = ModuleServiceManager.getService(module, MyModuleService.class);
```  

### 示例插件
本节将演示如何创建和使用插件服务，然后您可以下载并安装一个示例插件。该插件有一个项目组件，它实现了一个用于计算IDE中当前打开项目数量的service。当前打开的项目数量大于最大值的时候，该插件将会返回一个错误信息并关闭最近打开的项目。
### 安装并运行示例插件
- 在[这里](https://github.com/JetBrains/intellij-sdk-docs/tree/master/code_samples/max_opened_projects)下载包含了示例插件的项目。  
- 启动 *Intellij IDEA* ，在打开的页面中，单击 *Open Project* ，然后使用 *Open Project* 对话框打开 *max_opened_projects* 项目。  
- 在主菜单上，选择 *Run | Run* 或者按下 ```Shift``` + ```F10``` 快捷键。  
-  如果需要的话，更改[Run/Debug Configurations](http://www.jetbrains.com/help/idea/run-debug-configuration-plugin.html)。
