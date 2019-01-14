# 创建Action
您的插件可以通过向菜单和工具栏添加新的选项来自定义IntelliJ平台的UI。IntelliJ平台提供了AnAction类，每次选择菜单项或单击工具栏按钮时都会调用它的 ```actionPerformed ``` 方法。  

要在 *IntelliJ Platform* 中创建自定义操作，您需要执行两个基本步骤：  
- 在您的插件中，定义一个将自己的选项添加到菜单和工具栏中的行为或一些列行为。  
- 注册您的行为。  

本主题概述上述步骤。如需详细资料及示例，请参阅[IntelliJ Platform Action System]()。  

## 定义Actions

Action是从AnAction类派生的类。要定义您的操作，在您的插件中，创建一个从 ```AnAction``` 类派生的Java类。在这个类中，当选择菜单项或工具栏按钮时会调用重写的 ```actionPerformed``` 方法。  
为了了解这个过程，请参照以下从AnAction类派生的TextBox类的代码片段：
```java
public class TextBoxes extends AnAction {
    // If you register the action from Java code, this constructor is used to set the menu item name
    // (optionally, you can specify the menu description and an icon to display next to the menu item).
    // You can omit this constructor when registering the action in the plugin.xml file.
    public TextBoxes() {
        // Set the menu item name.
        super("Text _Boxes");
        // Set the menu item name, description and icon.
        // super("Text _Boxes","Item description",IconLoader.getIcon("/Mypackage/icon.png"));
    }

    public void actionPerformed(AnActionEvent event) {
        Project project = event.getData(PlatformDataKeys.PROJECT);
        String txt= Messages.showInputDialog(project, "What is your name?", "Input your name", Messages.getQuestionIcon());
        Messages.showMessageDialog(project, "Hello, " + txt + "!\n I am glad to see you.", "Information", Messages.getInformationIcon());
    }
}
```

注意，可以随意的从AnAction类派生出一组类。在这种情况下，您的插件将定义一系列的行为操作。

## 注册Actions
一旦定义了行为操作或者行为操作系统，就必须注册它们来指定与操作相关联的菜单项或工具栏按钮。您可以按照下列任意一个方法进行注册操作：
- 在  ```plugin.xml``` 文件的 ```<action>``` 部分中注册操作。  
- 从Java代码进行注册操作。  

本节提供了一些示例来说明如何进行注册操作。有关更多信息，请参见[IntelliJ Platform Action System]()。

### 在plugin.xml文件中注册Actions
要注册您的操作，请对您的IDEA项目的plugin.xml文件的 ```<actions>``` 部分进行适当的更改。以下的plugin.xml文件代码片段演示的是如何将菜单组(选项)添加到主菜单中。单击此项使您可以访问 **Sample Menu | Text Boxes and Sample Menu | Show Dialog** 菜单命令：  

![](http://www.jetbrains.org/intellij/sdk/docs/basics/getting_started/img/sample_menu.jpg)

```xml
<actions>
  <group id="MyPlugin.SampleMenu" text="_Sample Menu" description="Sample menu">
    <add-to-group group-id="MainMenu" anchor="last"  />
       <action id="Myplugin.Textboxes" class="Mypackage.TextBoxes" text="Text _Boxes" description="A test menu item" />
       <action id="Myplugin.Dialogs" class="Mypackage.MyShowDialog" text="Show _Dialog" description="A test menu item" />
  </group>
</actions>
```
这个plugin.xml文件的代码片段只演示了您可以在<action>中使用的部分元素来注册您的操作。有关用于注册操作的所有元素的信息，请参阅[ntelliJ Platform Action System]()。

### 从Java代码中注册Actions
或者，您可以通过Java代码注册您的操作。有关说明如何从Java代码注册操作的更多信息和示例，请参见[IntelliJ Platform Action System]()。
