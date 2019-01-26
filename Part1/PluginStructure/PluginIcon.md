# 插件的图标
从2019.1版开始，IntelliJ平台支持使用图标来表示插件。插件图标是插件功能、技术或公司的独特代表。

插件图标显示在JetBrains产品插件管理器UI中的Settings/Preferences。插件图标也会显示在插件库中和插件市场中。  

**注意** ：插件中使用的图标和图像有不同的要求。有关更多信息，请参考[Working with Icons and Images]()。

## 插件图标格式
所有插件图标必须是SVG格式。插件图标将显示为40*40 px和80*80 px大小。但是，只需提供一个大小图标，因为它会自动缩放。

图标尺寸 | 示例SVG图标
----|------
40x40 |  ![](http://www.jetbrains.org/intellij/sdk/docs/basics/plugin_structure/img/kotlin40.svg)

## 插件图标文件命名规定
插件图标文件根据以下规定命名：
- ```pluginIcon.svg``` 用于默认(light)JetBrains IDE主题  
- ```pluginIcon_dark.svg``` 用于 JetBrains IDE的Darcula主题  

## 在插件项目中添加插件图标
插件图标文件必须位于插件分发文件的 ```META-INF``` 文件夹中，即上传到插件仓库并安装到JetBrains IDE中的*.jar或*.zip文件。  

若要在分发文件中包含插件图标，请将插件图标文件放入插件项目的 ```resources/META-INF``` 文件夹中。注意，无论使用DevKit或Gradle来开发插件，这个要求都是不变的。例如：  
![](http://www.jetbrains.org/intellij/sdk/docs/basics/plugin_structure/img/resource_directory_structure.png)
