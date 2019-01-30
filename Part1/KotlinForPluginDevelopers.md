# 插件开发者如何使用Kotlin

## 1.为什么使用Kotlin ？
使用Kotlin为IntelliJ平台编写插件与用Java编写插件非常相似。现有插件开发者可以通过使用绑定到IntelliJ平台(版本143.+)的J2K编译器将样板Java类转换为与它们等效的Kotlin类，开发者可以轻松地将Kotlin类与他们现有的Java代码匹配和混合。  

除了[null safety](https://kotlinlang.org/docs/reference/null-safety.html)和[type-safe builders](https://kotlinlang.org/docs/reference/type-safe-builders.html)之外，Kotlin语言还为插件开发提供了许多方便的特性，使插件更易于阅读和维护。与[Kotlin for Android](https://kotlinlang.org/docs/tutorials/kotlin-android.html)非常相似，IntelliJ平台广泛使用回调，这在Kotlin中很容易使用lambdas来解决。

同样，在IntelliJ IDEA中使用[扩展](https://kotlinlang.org/docs/reference/extensions.html)很容易定制内部类的行为。例如，通常保护日志记录的语句，以避免参数构造的成本，从而导致在使用日志时会使用以下方式：

```java
if(logger.isDebugEnabled()) {
    logger.debug("...");
}
```  

通过声明以下扩展方法，我们可以在Kotlin中更简洁地获得相同的结果：

```java
inline fun Logger.debug(lazyMessage: () -> String) {
  if (isDebugEnabled) {
    debug(lazyMessage())
  }
}
```  
