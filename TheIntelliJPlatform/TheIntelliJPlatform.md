# IntelliJ平台是什么？
*IntelliJ平台* 本身并不是一个产品，而是为建立IDE提供的一个平台。它用于驱动JetBrains的产品，如[IntelliJ IDEA](https://www.jetbrains.com/idea/)，[WebStorm](https://www.jetbrains.com/webstorm/)，[RubyMine](https://www.jetbrains.com/ruby/)，[DataGrip](https://www.jetbrains.com/datagrip/) and [Rider](https://www.jetbrains.com/rider/)。它也是开源的，第三方开发者可以使用它来构建IDE，例如Google出品的[Android Studio](https://developer.android.com/studio/index.html)。  
*IntelliJ平台* 为这些IDE提供了丰富的语言工具支持等所有基础设施。它提供了一个组件驱动的、基于跨平台JVM的应用程序主机，提供了一个高级用户界面工具包，用于创建工具窗口、树视图和列表(支持快速搜索)以及弹出菜单和对话框。  

它还包括图像编辑器和富文本编辑器，并提供语法突出显示、代码折叠、代码补全成和其他富文本编辑功能的抽象实现。  

此外，它还包括构建通用IDE功能的可插入API，如项目模型和构建系统。它还为非常丰富的调试体验提供了基础设施，具有与语言无关的高级断点支持、调用堆栈、监视窗口和表达式评估。  

但是IntelliJ平台的真正能力来自于程序结构接口(PSI)。它提供了一系列功能，可用于解析文件，构建丰富的代码语法和语义模型，并根据这些数据建立索引。这提供了许多功能，从快速导航到文件、类型和符号，到代码补全窗口的内容、查找用法、代码检查和代码重写、快速修复或重构以及许多其他功能。  

*IntelliJ平台* 包括一些语言的解析器和PSI模型，它的可组合特性意味着可以添加对其他语言的支持。

## 插件  
构建在IntelliJ平台上的产品是可组合的应用程序，平台负责创建组件，并将依赖项注入到类中。IntelliJ平台完全支持插件，JetBrains拥有一个[插件仓库](https://plugins.jetbrains.com/)，可以用来分发支持一个或多个产品的插件。它也可以托管您自己的仓库，并单独分发插件。  

插件可以以多种方式扩展平台，从添加一个简单的菜单项到添加对完整语言、构建系统和调试器的支持。IntelliJ平台中的许多现有功能都是以插件的形式编写的，这些插件可以根据最终产品的需求进行添加或删除。有关更多细节，请参见[插件](http://www.jetbrains.org/intellij/sdk/docs/basics.html)部分。  

IntelliJ平台是一个JVM应用程序，主要用Java和Kotlin编写。您需要熟悉这些语言和相关工具，以便为基于IntelliJ平台的产品编写插件。同时，您不能用非JVM语言扩展IntelliJ平台。  

## 开源  
*IntelliJ平台* 是开源的，使用[Apache许可证](https://github.com/JetBrains/intellij-community/blob/master/LICENSE.txt)，并托管在[GitHub](https://github.com/JetBrains/intellij-community)上。  

虽然本指南将IntelliJ平台作为一个单独的整体，但没有“IntelliJ平台”GitHub repo。相反，该平台被认为与IntelliJ IDEA Community Edition版几乎完全重叠，后者是IntelliJ IDEA旗舰版的免费开源版本(GitHub repo的链接是[JetBrains/JetBrains/intellij-community](https://github.com/JetBrains/intellij-community))。  

IntelliJ IDEA Ultimate版是IntelliJ IDEA Community版的一个超集。它基于社区版本，但包含了封闭的源代码插件([参见此功能比较](https://www.jetbrains.com/idea/features/editions_comparison_matrix.html))。类似地，其他产品，如WebStorm和DataGrip也是基于IntelliJ IDEA Community Edition版本，但包含了不同的插件集，并且去除了其他默认插件。  

这允许插件针对多个产品，每个产品可以选择基本功能或者从IntelliJ IDEA Community Edition版本repo中提供的插件中进行选择。这就是我们所说的 *IntelliJ平台* 。  

通常，基于 *IntelliJ平台* 的IDE将 ```intellij-community ``` repo作为Git子模块，并提供配置来描述是来自 ```intellij-community ```的哪些插件，以及哪些自定义插件将构成产品。这就是 IDEA Ultimate团队的工作方式，他们为定制插件和IntelliJ平台本身都贡献了代码。  

当然，因为 *IntelliJ平台* 是开源的，所以我们也接受对平台本身的[pull请求](https://github.com/JetBrains/intellij-community/pulls)，而不仅仅是打开源代码以供查看。可以使用[YouTrack(使用IDEA项目)](https://youtrack.jetbrains.com/oauth?state=%2Fissues%2FIDEA)来管理Issue，如果您希望对平台做出贡献，一般最好先打开一个issue，在进行更改之前先说明您希望所做的改变-这让团队有机会给您提供反馈和建议。更多细节可以在[如何为IntelliJ平台作出贡献]((ContributingToTheIntelliJPlatform.md))中找到。  

## Rider
[Rider](https://www.jetbrains.com/rider/)使用的 *IntelliJ平台* 与其他基于IntelliJ的IDE不同。它使用 *IntelliJ平台* 为C#和.NET IDE提供用户界面，并提供标准IntelliJ编辑器、工具窗口、调试体验等。它还集成了标准的Find Usages和Search Everywhere UI，并包含代码补全、语法高亮等功能。  

但是，它没有为C#文件创建完整的PSI(语法和语义)模型。相反的，它重用[ReSharper](https://www.jetbrains.com/resharper/)来提供语言功能。在ReSharper的命令行版本中，所有的C# PSI模型，以及所有检查和代码重写(如快速修复和重构)都在进程之外运行。这意味着为Rider创建插件涉及两个部分-一个负责在IntelliJ“前端”显示用户界面，一个负责在ReSharper“后端”来分析和配合C# PSI。  

幸运的是，许多插件可以简单地使用ReSharper后端-Rider负责显示检查和代码完成的结果，并且可以编写许多不需要IntelliJ UI组件的插件。更多详细信息可在“特定产品”部分中找到。
