# 插件的Content

有两种的方法来组织插件内容：  

- 由放置在plugins目录中的一个```.jar```文件组成：
该文件应该包含配置文件( ```META-INF/plugin.xml``` )、插件图标文件( ```META-INF/pluginIcon*.svg``` )以及实现插件功能的类。配置文件指定插件的名称、描述信息、版本、开发者、受支持的IntelliJIDEA版本、插件组件、动作和动作组以及动作用户界面布局。有关插件图标SVG文件的更多信息，请参见[Plugin Icon File]()。
```
.IntelliJIDEAx0/
  plugins/
    sample.jar/
      com/foo/...
      ...
      ...
      META-INF/
        plugin.xml
        pluginIcon.svg
        pluginIcon_dark.svg
```  

- 位于lib文件夹中的.jar文件格式的插件：  
```
.IntelliJIDEAx0/
  plugins/
    Sample/
      lib/
        libfoo.jar
        libbar.jar
        Sample.jar/
          com/foo/...
          ...
          ...
          META-INF/
            plugin.xml
            pluginIcon.svg
            pluginIcon_dark.svg
```

该 ```lib``` 文件夹中的所有jar包都会自动添加到classpath中。
