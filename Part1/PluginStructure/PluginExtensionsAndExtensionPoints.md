# 插件的Extensions和Extension Points
IntelliJ平台提供了扩展和扩展点的概念，允许插件与其他插件或IDE本身进行交互。  

## Extension points
如果您希望您的插件允许其他插件扩展其功能，则必须在插件中声明一个或多个扩展点。每个扩展点定义了允许访问此扩展点的类或接口。  

## Extensions
如果您希望您的插件扩展其他插件或IntelliJ平台的功能，则必须声明一个或多个扩展。  

## 如何声明Extension和Extension Points
您可以分别在plugin配置文件 ```plugin.xml``` 中的 ```<extensions>``` 和 ```<extensionPoints>``` 部分中声明扩展和扩展点。

### 声明extension point
在 ```<extensionPoints>``` 部分中，插入一个子元素 ```<extensionPoint>``` ，该元素分别定义扩展点名称和bean类的名称，或者在 ```name``` 、```beanClass``` 和 ```interface``` 属性中允许扩展插件功能的接口名称。  

为了解释这个过程，请参考以下plugin.xml文件的示例部分：  
```xml
<extensionPoints>
  <extensionPoint name="MyExtensionPoint1" beanClass="MyPlugin.MyBeanClass1">
  <extensionPoint name="MyExtensionPoint2" interface="MyPlugin.MyInterface">
</extensionPoints>
```  

- ```interface ```属性设置了对扩展点做出贡献的插件所必须实现的接口。  
- ```beanClass```属性设置一个bean类，该类指定一个或几个带有[@Attribute](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/xml/dom-openapi/src/com/intellij/util/xml/Attribute.java)注释的属性。  

贡献于扩展点的插件将从```plugin.xml```文件中读取这些属性。   

为了解释这一点，请参考上面的```plugin.xml```文件中使用的```MyBeanClass1bean```类：  

```java
public class MyBeanClass1 extends AbstractExtensionPointBean {
  @Attribute("key")
  public String key;

  @Attribute("implementationClass")
  public String implementationClass;

  public String getKey() {
    return key;
  }

  public String getClass() {
    return implementationClass;
  }
}
```  

注意，要声明用于访问```MyExtensionPoint1```扩展点的扩展名，```plugin.xm```l文件必须包含```<MyExtensionPoint1```>标记，并将```key```和```implementationClass```属性设置为适当的值（见下面的示例）。  

### 声明extension
- 对于```<extensions>```元素，将```xmlns```(已弃用)或```defaultExtensionNs```属性设置为以下值之一：  
  - ```com.intellij```，如果您的插件扩展了IntelliJ平台的核心功能。  
  - ```{ID of a plugin}```，如果您的插件扩展了另一个插件的功能。   
- 将一个新的子元素添加到```<extensions>```元素中。子元素名必须与要访问扩展的扩展点的名称相匹配。  
- 根据扩展点的类型，执行以下操作之一：
  - 如果扩展点是使用```interface```属性声明的，对于新添加的子元素，设置```implementation```给实现指定接口的类的名称。
  - 如果扩展点是使用```beanClass```属性声明的，对于新添加的子元素，在指定的bean类中设置所有带有[@Attribute](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/xml/dom-openapi/src/com/intellij/util/xml/Attribute.java)注释的属性。  

为了澄清这个过程，请考虑plugin.xml文件的以下示例部分，该部分定义了两个扩展，用于访问 *IntelliJ Platform* 中的 ```appStarter``` 和 ```applicationConfigurable``` 扩展点，以及一个用于访问测试插件中 ```MyExtensionPoint1``` 扩展点的扩展：  

```xml
<!-- Declare extensions to access extension points in the IntelliJ Platform.
     These extension points have been declared using the "interface" attribute.
 -->
  <extensions defaultExtensionNs="com.intellij">
    <appStarter implementation="MyTestPackage.MyTestExtension1" />
    <applicationConfigurable implementation="MyTestPackage.MyTestExtension2" />
  </extensions>

<!-- Declare extensions to access extension points in a custom plugin
     The MyExtensionPoint1 extension point has been declared using *beanClass* attribute.
-->
  <extensions defaultExtensionNs="MyPluginID">
     <MyExtensionPoint1 key="keyValue" implementationClass="MyTestPackage.MyClassImpl"></MyExtensionPoint1>
  </extensions>
```

## 如何获得扩展点列表？
要获得 *IntelliJ Platform* 核心中可用的扩展点列表，请参阅以下XML配置文件的 ```<extensionPoints>``` 部分：  
- [LangExtensionPoints.xml](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/platform-resources/src/META-INF/LangExtensionPoints.xml)
- [PlatformExtensionPoints.xml](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/platform-resources/src/META-INF/PlatformExtensionPoints.xml)
- [VcsExtensionPoints.xml](https://upsource.jetbrains.com/idea-ce/file/idea-ce-a7b3d4e9e48efbd4ac75105e9737cea25324f11e/platform/platform-resources/src/META-INF/VcsExtensionPoints.xml)  

## 补充资料和样本
有关如何创建有助于IDEA核心的插件的示例插件和详细说明，请参阅定制IDEA设置对话框和创建工具窗口。
