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
