# gradle介绍

gradle是一个开源的自动化构建的工具。被设计用来更灵活的构建几乎所有类型的软件。

gradle最重要的几个特征：

1. 高性能
2. 以JVM为基础
3. 约定：可以重写或者自定义
4. 可扩展性
5. 支持IDE
6. 观察性：提供广泛的构建信息

gradle的核心原则：

1. gradle是一个通用的构建工具
2. gradle的核心模型是基于任务
3. gradle有一些固定的构建阶段
4. gradle不止一种扩展方式
5. 构建脚本对API进行操作

# gradle和maven对比

* 灵活性

  gradle可以扩展，自定义task。

* 性能

  gradle只执行修改的文件；

  重用构建输出内容；

  守护进程将构建信息存储在内存中。

* 用户体验

  提供了build scan

* 依赖管理

  gradle可以自定义依赖选择和替换规则，maven只能替换版本

# gradle和maven命令map

| maven   | gradle             |
| ------- | ------------------ |
| clean   | clean              |
| compile | classes            |
| test    | test               |
| package | assemble           |
| verify  | check              |
| install | publicToMavenLocal |
| deploy  | publish            |



# gradle和maven依赖范围map

| maven   | gradle                                             |
| ------- | -------------------------------------------------- |
| compile | implementation和api。api只用于Java Libirary Plugin |
| runtime | runtimeOnly                                        |
| test    | testImplementation和testRuntimeOnly                |
| provide | compileOnly                                        |

# gradle配置机制

* 命令行

  优先级最高，比如 --build-cache。

* 系统属性

  gradle.properties文件中的属性。

* gradle属性

  根目录下gradle.properties文件或者GRADLE_USER_HOME环境变量中。

* 环境变量

  

# 创建java应用

1. 执行`gradle init`命令

2. 项目结构

   ~~~
   ├── gradle 
   │   └── wrapper
   │       ├── gradle-wrapper.jar
   │       └── gradle-wrapper.properties
   ├── gradlew 
   ├── gradlew.bat 
   ├── settings.gradle 
   └── app
       ├── build.gradle 
       └── src
           ├── main
           │   └── java 
           │       └── demo
           │           └── App.java
           └── test
               └── java 
                   └── demo
                       └── AppTest.java
   
   ~~~

   **gradle**：生成的包装文件的文件夹

   **gradlew，gradlew.bat**：gradle包装启动脚本

   **settings.gradle**：定义build name和子项目的配置文件

   **build.gradle**：app项目的build脚本

   

# 项目文件介绍

## settings.gradle

~~~groovy
rootProject.name = 'demo'
include('app')
~~~

`rootProject.name`：项目build的名称。

`include`：定义包括的子项目

## build.gradle

~~~groovy
plugins {
    id 'application' 
}

version '0.0.1'

repositories {
    mavenCentral() 
}

dependencies {
    testImplementation 'junit:junit:4.13.1' 

    implementation 'com.google.guava:guava:30.0-jre' 
}

application {
    mainClass = 'demo.App' 
}
~~~

`id`：应用application插件用以支持build java CLI应用。

`mavenCentral()` ：使用maven中心解析依赖。

`testImplementation`：单元测试框架。

`implementation`：应用依赖。

`mainClass`：应用程序main方法所在的类。

`version`：生成的jar包版本，要写在plugins的后面

# gradle命令

## check

执行单元测试。如果执行单个子项目的单元测试，则使用**:<subproject>:check**。

生成的报告在**<subproject>/build/reports**文件夹。

## build

编译项目。

成功编译后会在<subproject>/build/distributions目录下生成tar和zip文件各一份。

**--scan**

生成build的后台执行明细。

## run

运行应用，执行build.radle配置的mainClass中的main方法。

