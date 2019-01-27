# 插件的依赖

您的插件可能依赖于其他插件的类，无论是绑定的、第三方的还是您自己的。为了了解这种依赖关系(例如Kotlin)，您可以执行以下步骤：
- 如果插件没有绑定，运行目标IDE的沙箱实例并安装插件。  
- 如果使用Gradle与Kotlin脚本一起构建插件，请在 ```intellij``` 块中使用 ```setPlugins()``` ，例如：  
```
intellij {
    plugins 'org.jetbrains.kotlin:1.3.11-release-IJ2018.3-1'
}
```

- 如果使用Gradle与Groovy脚本一起构建插件，则需要将依赖项添加到build.gradle中 ```intellij``` 块的 ```plugins``` 参数中，例如：

```
intellij {
    plugins 'org.jetbrains.kotlin:1.3.11-release-IJ2018.3-1'
}
```
