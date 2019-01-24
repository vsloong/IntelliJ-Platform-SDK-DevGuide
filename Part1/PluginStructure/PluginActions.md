# 插件的Actions
*IntelliJ Platform* 提供了 *actions* 的概念。Action是从[AnAction](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/editor-ui-api/src/com/intellij/openapi/actionSystem/AnAction.java)类派生的类，在选择菜单项或工具栏按钮时会调用其 ```actionPerformed``` 方法。  

这一些列的actions允许插件添加他们自己的项目到IDEA的菜单项和工具栏。action可以被组织成组，而这些组又可以包含其他组。一组action可以形成工具栏或菜单。组的子组可以形成菜单的子菜单。您可以在[IntelliJ Platform Action System](http://www.jetbrains.org/intellij/sdk/docs/basics/action_system.html)中找到有关如何创建和注册action的详细信息。
