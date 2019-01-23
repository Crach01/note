### Gradle构建工具

#### 构建工具

* 依赖管理
* 测试、打包、发布
* 主流构建工具
  * Ant  编译、测试、打包
  * Maven 依赖管理、发布
  * Gradle  Groovy

#### 项目自动化

* Gradle是什么

  > 一个开源的**项目自动化构建工具**，建立在Apache Ant 和Apache Maven概念的基础上，并引入了基于Groovy的特定领域语言（DSL），而不再使用XML形式管理构建脚本

* Groovy是什么

  * Groovy是用于Java虚拟机的一种敏捷的动态语言，它是一种成熟的面向对象编程的语言，既可以用于面向对象编程，又可以用作纯粹的脚本语言。使用这种语言不必编写过多的代码，同时又具有闭包和动态语言的其他特性。
  * Groovy与Java比较
    * Groovy完全兼容Java语法
    * 分号是可选的
    * 类、方法默认是public的
    * 编译器给属性自动添加getter/setter方法
    * 属性可以直接用点号获取
    * 最后一个表达式的值会被作为返回值
    * ==等同于equals()，不会有NullPointerExceptions
  * 高效的Groovy特性
    * assert语句
    * 可选类型定义
    * 可选的括号
    * 字符串
      * ‘string’
      * “string”
      * '''string'''
    * 集合API
    * 闭包

```
//groovy 高效特性
//1 可选的类型定义
def version = 1
//2 assert
assert version != 2
//3 括号是可选的
println(version)
println version
//4 字符串
//单引号是字符串
//双引号字符串可添加变量
//三引号字符串可以换行
def s1 = 'imooc'
def s2 = "gradle version is ${version}"
def s3 = '''my
name is
gradle'''
println(s1)
println(s2)
println(s3)
//5  集合API
//list
def buildTools = ['ant', 'maven']
buildTools << 'gradle'
assert buildTools.getClass() == ArrayList
assert buildTools.size() == 3
//map
def buildYears = ['ant':2000, 'maven':2004]
buildYears.gradle = 2009
println buildYears.gradle
println buildYears['ant']
println buildYears.getClass()
//6 闭包
def c1 = {
    v ->
        print v
}
def c2 = {
    print "hello"
}
def method1(Closure closure) {//不要导入包
    closure('param')
}
def method2(Closure closure){
    closure()
}
method1(c1)
method2(c2)
```

```
//构建脚本中默认都是有个Project实例的
apply pligin:'java'

version = '0.1'

repositories {
    mavenCentral()
}
dependencies {
    compile 'commons-code:commons-codec:1.6'
}
```

![](C:\Users\18451\Desktop\note\gradle目录结构.png)

* 创建  gradle  工程
* 把项目打包成jar包
* 把web项目打包成war文件

#### 构建脚本概要

##### 构建块

> Gradle构建中的两个基本概念是项目（**project**）和任务（**task**），每个构建至少包含一个项目，项目中包含一个或多个任务。在多项目构建中，一个项目可以依赖于其他项目；类似的，任务可以形成一个依赖关系图来确保他们的执行顺序

* 项目（Project）
  * 一个项目代表一个正在构建的组件（比如一个jar文件），当构建启动后，Gradle会基于build.gradle实例化一个**org.gradle.api.Project**类，并且能够通过project变量使其隐式可用
  * 项目的属性： **group、name、version**                ***唯一确定一个项目***
  * **apply、dependencies、repositories、task**
  * 属性的其他配置方式：ext、graddle.properties
* 任务（task）
  * 任务对应**org.gradle.api.Task** 。主要包括任务动作和任务依赖。任务动作定义了一个最小的工作单元。可以定义依赖于其他任务、动作序列和执行条件
  * dependsOn
  * doFirst、doLast <<