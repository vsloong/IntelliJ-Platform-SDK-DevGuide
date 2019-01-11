### 该文章系本人初次练手翻译，同时也是为了强迫自己阅读英文文档并学习IntelliJ平台插件开发的一次尝试，如有翻译不恰当之处还请多多指教。

原文档地址在：https://github.com/JetBrains/intellij-sdk-docs

-----  
# 开发指南目录在这里【[目录](Summary.md)】
-----


# IntelliJ 平台SDK开发文档
这是[IntelliJ平台SDK文档](http://www.jetbrains.org/intellij/sdk/docs/welcome.html)的存储仓库。

## 反馈Bugs
如果文章内容有任何不一致之处请及时告知我们，包括但不限于过时的材料，没有实质性的issues以及您发现的其他文档或者例子中的瑕疵，您都可以提交issue到[YouTrack](https://youtrack.jetbrains.com/issues/IJSDK)。

## 在本地协助开发
要check out并运行该仓库的副本，请按照以下步骤操作。

### 准备环境
- 确认您已安装[git客户端](https://git-scm.com/downloads)
- 该站点需要Ruby2.0或更高版本。请按照Ruby语言官方网站的说明进行[下载](https://www.ruby-lang.org/en/downloads/)和[安装](https://www.ruby-lang.org/en/documentation/installation/)，以便Ruby可以在您的电脑上正常运行。
- 该站点还需要Jekyll，一个基于Ruby的构建框架。要安装Jekyll，请参考它的[安装指南](要安装Jekyll，请参考它的安装指南。)。**注意**：如果您使用的是Windows操作系统，在安装Jekyll时可能会遇到一些特殊的情况。请参阅[Windows运行Jekyll指南](http://jekyll-windows.juthilo.com/)以获取更多信息。

## Checking Out 该站点仓库
运行以下指令来check out代码：

```
git clone https://github.com/JetBrains/intellij-sdk-docs.git
```

## 初始化子模块
该站点使用的是JetBrains自定义的Web模板。要在本地启用自定义模板，您需要先初始化仓库子模块。为此，您需要在checkout目录中运行以下指令：

```
git submodule update --init --remote
```

## 安装Gems
当你checkout主仓库并且成功初始化子模块之后，运行指令```bundle install```来安装额外需要的gems。

## 构建和预览
通过Ruby来实现一组Rake任务，这样通过提供在本地构建和运行站点的简短指令，来实现一个类似的生产程序。

### 从源码构建站点
- 确保你在工程项目的根目录
- 构建静态站点内容请运行指令```rake build```

### 站点预览
- 开启Web服务请运行指令```rake preview```
- 在你的浏览器中打开地址：http://localhost:4000/intellij/sdk/docs/ 。**注意**：确保你在安装的时候没有更改Jekyll默认的端口号。
