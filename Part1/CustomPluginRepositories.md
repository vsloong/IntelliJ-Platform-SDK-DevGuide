# 将插件发布到自定义插件库

如果您打算使用 [JetBrains Plugin Repository](JetBrains Plugin Repository) 以外的插件仓库，您需要：  

- 在用于自定义仓库的HTTPS Web服务器上创建并维护 ```updatePlugins.xml``` 文件。该文件描述了您的自定义仓库中所有可以使用的插件以及每个插件的下载地址。  
- 将jar/zip格式的插件文件上传到HTTPS Web服务器。它可以是与自定义仓库的相同的Web服务器，也可以是其他不同的HTTPS Web服务器。  
- 将自定义仓库的地址添加到 JetBrains IDE [Repository Settings/Preferences](https://www.jetbrains.com/help/idea/managing-plugins.html#repos)。  

## 在updatePlugins文件中描述插件信息
每个自定义插件仓库必须至少有一个 ```updatePlugins.xml``` 文件来描述每个被托管插件的最新可用版本。JetBrains IDE将使用 ```updatePlugins.xml``` 中的描述来根据id、IDE版本和插件版本等属性去安装插件。这些属性由JetBrains IDE显示，以帮助用户选择或升级插件。这些描述信息还告诉JetBrains IDE可以从哪里下载到该插件。  

自定义插件库的 ```updatePlugins.xml``` 文件是由仓库的管理员创建和维护的。如果自定义仓库的使用者使用了多个版本的JetBrains IDE，则可能需要多个 ```updatePlugins.xml``` 文件。例如，```updatePlugins-182.xml```、```updatePlugins-183.xml```分别用于IntelliJ IDEA 2018.2和2018.3。每个 ```updatePlugins-\*.xml``` 文件都将有一个惟一的URL，该URL将添加到[Repository Settings/Preferences](https://www.jetbrains.com/help/idea/managing-plugins.html#repos)。  

### updatePlugins文件的格式      

s ```updatePlugins.xml```只是描述每个插件所包含元素的列表格式：    

 ```xml
 <?xml version="1.0" encoding="UTF-8"?>
<!--
  The <plugins> element contains the description of the plugins available at this repository. Required.
-->
<plugins>
  <!--
    Each <plugin> element describes one plugin in the repository. Required.
    id - used by JetBrains IDEs to uniquely identify a plugin. Required. Must match <id> in plugin.xml
    url - path to download the plugin JAR/ZIP file. Required. Must be HTTPS
    version - version of this plugin. Required. Must match <version> in plugin.xml
  -->
  <plugin id="fully.qualified.id.of.this.plugin" url="https://www.mycompany.com/my_repository/mypluginname.jar" version="major.minor.update">
    <!--
      The <idea-version> element must match the same element in plugin.xml. Required.
    -->
    <idea-version since-build="181.3" until-build="191.*" />
  </plugin>
  <plugin id="id.of.different.plugin" url="https://www.otherserver.com/other_repository/differentplugin.jar" version="major.minor">
    <idea-version since-build="181.3" until-build="191.*" />
  </plugin>
  <plugin>
    <!-- And so on for other plugins... -->
  </plugin>
</plugins>
 ```  

 **注意** ：
- 一个 ```updatePlugins.xml``` 文件必须至少包含一组 ```<plugin></plugin>``` 元素。  
- 插件的 ```id``` 只能在 ```updatePlugins.xml``` 文件中列一次。  
- 必须将具有相同 ```id``` 但不同的版本的多个插件拆分为单独的 ```updatePlugins-\*.xml``` 文件。

### 可选更新项
