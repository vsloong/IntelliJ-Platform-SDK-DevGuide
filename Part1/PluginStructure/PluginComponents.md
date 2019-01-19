# 插件的Components
组件是插件集成的基本概念。有三种组件：
- 当IDE启动时，将创建并初始化应用程序级的组件。可以使用 ```getComponent(Class)``` 方法从[Application](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/core-api/src/com/intellij/openapi/application/Application.java)实例中获取它们。  
- 当IDE中的[Project](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/core-api/src/com/intellij/openapi/project/Project.java)打开时候会创建项目级的组件。（请注意，即使项目违背打开组件也可能会被创建。）可以使用 ```getComponent(Class)``` 方法从 ```Project``` 实例中获取它们。  
- 当IDE加载项目中[Module](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/core-api/src/com/intellij/openapi/module/Module.java)的时候将会创建模块级组件。可以使用 ```getComponent(Class)``` 方法从 ```Module``` 实例中获取它们。   

每个组件都应该在配置文件中有指定的接口和实现类。接口类将用于从其他组件中检索组件，实现类将用于组件的实例化。  

请注意，同一级别的两个组件([Application](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/core-api/src/com/intellij/openapi/application/Application.java)，[Project](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/core-api/src/com/intellij/openapi/application/Application.java)或者[Module](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/core-api/src/com/intellij/openapi/module/Module.java)])不能具有相同的接口类。可以为接口和实现指定相同的类。  

每个组件都有一个唯一的名称，用于外部化和其他内部需求。组件的名称由其 ```getComponentName()``` 方法返回。   

## 组件命名表示法
建议将组件命名在 ```<plugin_name>.<Component_name>``` 表单中。  

## 应用层组件  
通常，应用层组件的实现类可以实现[ApplicationComponent](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/core-api/src/com/intellij/openapi/components/ApplicationComponent.java)接口。  

没有依赖项的应用层组件应该有一个用于实例化的无参构造函数。如果应用层组件依赖于其他应用层组件，则可以将这些组件指定为构造函数参数。IntelliJ平台将确保以正确的顺序实例化组件，以满足其依赖关系。  

应用层级组件必须在plugin.xml文件的```<application-components>```部分中注册（请参考[Plugin Configuration File)]()）。  

## 项目层组件  
通常，项目级组件的实现类可以实现[ProjectComponent](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/core-api/src/com/intellij/openapi/components/ProjectComponent.java)接口。  

项目层组件的如果需要实例化的话那么它的构造函数需要有一个[Project](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/core-api/src/com/intellij/openapi/project/Project.java)类型的参数。如果依赖其他应用层或项目层组件，它还可以将这些组件指定为参数。  

项目层组件必须在plugin.xml文件的 ```<project-components>``` 部分中注册（请参考[Plugin Configuration File]()）。  

## 模块层组件
通常，模块层组件的实现类需要实现[ModuleComponent](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/projectModel-api/src/com/intellij/openapi/module/ModuleComponent.java)接口。  

如果模块层组件需要实例化的话那么它的构造函数需要有一个模块类型的参数。如果它依赖于其他应用层、项目层或模块层组件的话，那么它还可以指定这些组件作为参数。  

项目层组件必须在plugin.xml文件的 ```<module-components>``` 部分中注册（请参考[Plugin Configuration File]()）。   

## 组件状态的持久化
如果组件的类实现了[JDOMExternalizabl](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/util/src/com/intellij/openapi/util/JDOMExternalizable.java)(已废弃)或[ PersistentStateComponent](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/projectModel-api/src/com/intellij/openapi/components/PersistentStateComponent.java)接口，则每个组件的状态将自动保存和加载。  

当组件的类实现[PersistentStateComponent](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/projectModel-api/src/com/intellij/openapi/components/PersistentStateComponent.java)接口时，组件状态将保存在一个XML文件中，您可以在Java代码中使用[@State](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/projectModel-api/src/com/intellij/openapi/components/State.java)和[@Storage](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/projectModel-api/src/com/intellij/openapi/components/Storage.java)注释来调用。  

当组件的类实现[JDOMExternalizabl](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/util/src/com/intellij/openapi/util/JDOMExternalizable.java)接口时，组件将其状态保存在以下文件中：  

- 项目层组件将其状态保存到项目的( ```.ipr``` )文件中。   
但是，如果 ```plugin.xm``` 文件中的工作区选项设置为 ```true``` ，则组件将其配置保存到工作区( ```.iws``` )文件中。  

- 模块层组件将其状态保存到模块( ```.iml``` )文件中。  

有关更多信息和示例，请参见[Persisting State of Components]()。


## 默认值
默认值(组件的预定义设置)应该放在```<component_name>.xml```文件中。将该文件放置在对应于默认包的插件的classpath中。  

如果组件具有默认值，则会调用两次 ```readExext()``` 方法：  

- 第一次默认执行
- 第二次保存配置  

## 插件组件的生命周期
组件按以下顺序加载：
- Creation - 构造函数被调用。  
- Initialization - ```initComponent```方法被调用（如果组件实现了[ApplicationComponent](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/core-api/src/com/intellij/openapi/components/ApplicationComponent.java)接口）。  
- Configuration - ```readExternal``` 方法被调用（如果组件实现了[ JDOMExternalizabl](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/util/src/com/intellij/openapi/util/JDOMExternalizable.java)接口），或者```loadState```方法被调用（如果组件实现了[PersistentStateComponent](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/projectModel-api/src/com/intellij/openapi/components/PersistentStateComponent.java)接口，并且没有默认的持久化状态）。  
- 对于模块层组件，调用[ModuleComponent](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/projectModel-api/src/com/intellij/openapi/module/ModuleComponent.java)接口的```moduleAdded```方法来通知向项目添加了一个模块。  
- 对于项目层组件，将调用[ProjectComponent](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/core-api/src/com/intellij/openapi/components/ProjectComponent.java)接口的```projectOped```方法来通知已加载项目。  

组件将按以下顺序卸载：  
- 保存配置 - 调用 ```writeExternal``` 方法(如果组件实现了[JDOMExternalable](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/util/src/com/intellij/openapi/util/JDOMExternalizable.java)接口)，或者调用 ```getState``` 方法(如果组件实现了[PersistentStateComponent](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/projectModel-api/src/com/intellij/openapi/components/PersistentStateComponent.java)接口)。  

- 结束 - ```disposeComponent``` 方法被调用。  

注意，您不能在组件的构造函数中使用 ```getComponent()``` 方法请求任何其他组件，否则您将得到一个断言。初始化组件时如果需要访问其他组件，可以将它们指定为构造函数参数或在 ```initComponent``` 方法中访问它们。
