---
title: Minecraft 1.8.9 FML Mod 开发教程
date: 2020-11-05 00:00:00
tags: tutorial
author: Yanbing Zhao
---
  
**[原文链接](https://fmltutor.ustc-zzzz.net/)**

# 0 绪论

### FML与Forge的由来

在很久很久以前（一个可以追溯到Minecraft刚刚发布的年代），人们就开始为Minecraft制作和使用Mod了。那个时候，由于Mod的制作方式是直接通过底层对Minecraft修改，所以将两个Minecraft Mod进行整合几乎是一件不可能的事情。

随后，Minecraft MCP（Minecraft Mod Coder Pack）的诞生，让Mod的制作和整合变得容易。这是因为Minecraft被进行了混淆化，这件事使得原本好看好记的诸如Item这样的类名、方法名、变量名等，通通被混淆化成了alq这样难以阅读和辨认的符号。Minecraft MCP的意义，就是对其进行反混淆，把alq这样的类名用一个诸如Item的名称代替。这使得人们可以通过更改MCP提供的源代码来进行Mod制作。但是，若是想合并两个Mod，必须要把两个Mod的源代码**一行一行**地进行整合。

直到2010年年底，一个叫作ModLoader的Mod横空出世，这使得Mod的开发方式产生了跨时代性的变化。ModLoader提供了一个框架和一套API，玩家通过这个框架可以很方便地管理Mod，开发者通过这一套API可以完成很多任务，如添加新方块、添加新物品，等等。

从2010年圣诞节前夕，Minecraft Beta版本发布开始，直到2011年年底Minecraft正式版发布，这一段时间被称为Minecraft Mod的“黄金时期”。很多著名的Mod如RailCraft，IndustrialCraft，BuildCraft等，都是这一时期开始开发的。但是，这段时期的Mod开发仍然问题重重。比如BuildCraft和IndistrialCraft就不能共存。他们更改了太多底层的东西。

这些Mod的开发者们看到Mod的整合仍旧困难，他们便发起了一项新的计划，这个计划就是Minecraft Forge。随着Minecraft正式版的发布和“黄金时期”的终结，很多Minecraft Forge的开发者，渐渐不再参与Minecraft Forge的开发。然而随着Mod数量的爆炸式增加，以LexManos和Cpw等为首的开发人员仍然坚持着Minecraft Forge的开发，并加入了很多崭新的特性。他们甚至将ModLoader这个框架整合进了Minecraft Forge，也就是Forge Mod Loader，简称FML。直到今天，Minecraft Forge和通过Minecraft Forge开发的难以计数的Mods，仍在发展壮大。

### 阅读本教程需要什么

#### Java等程序设计基础知识

本教程不针对Java初学者，本教程假定读者已经对Java的基础知识有了一定深入的了解。换言之，本教程假设读者已经对下面这些Java概念进行了深入地了解：

* 对象
* 类、属性和方法
* 变量、运算符和表达式
* 条件和循环
* 包、继承、多态和接口
* 泛型
* 容器
* 异常

下面这些Java概念虽然对Mod开发比较重要，但是读者在阅读之前不需要掌握或者只有阅读本教程更深入的部分才需要掌握，如果教程的某一部分认为读者不需要掌握，教程会在用到的时候会加以简要的介绍，如果需要，教程会提醒读者阅读相关的资料：

* 注解
* 数字和字符串的操作
* 输入输出流
* 多线程和同步
* 反射
* 中立字节码

下面这些Java概念和本Mod开发教程无关，虽然这些概念在其他Java应用中比较重要，但在本教程中不会用到。当然，了解这些知识对Mod开发是有一定帮助的：

* 正则表达式
* 图形界面
* 网络
* 数据库
* ...

这里推荐一本书《Head First Java, 2nd Edition》，这本书的中文版适用于没有任何编程基础的读者。如果看完了这本书的前十一章，对于阅读本教程的Java基础知识就已经足够了。

如果想深入学习的话，推荐《Effective Java (2nd Edition)》和《The Java Language Specification (3rd Edition)》两本书。

相对而言，作者会保证教程使用的代码可以在Java6下运行，尽量减少使用一些令人迷惑的特性，并尽可能保证除了Forge/FML之外，不使用其他的第三方库。

除了Java知识之外，了解一些游戏开发的知识，如OpenGL等等，对阅读本教程有一些帮助，但对于阅读本教程没有专门学习的必要，除非作者在某些地方特定指出了对该知识的需要。

#### 对Minecraft游戏的大致了解

本教程假设读者在阅读本教程前，已经对Minecraft这款游戏有了一些了解，包括Minecraft中一些现象的运作机制，一些小技巧等有了一些了解。作者也相信对本教程感兴趣的读者已经做到了这一点。

#### Java开发环境

为实践本教程的内容，读者可能需要一台安装有[JDK7](http://www.oracle.com/technetwork/cn/java/javase/downloads/jdk7-downloads-1880260.html)或[JDK8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)或其他JDK版本的，可以保证流畅运行Minecraft的电脑。别忘了这里修改的是游戏（本人旧电脑的流畅度就很难满足这一条件= =）。

开发Mod最为推荐的JDK版本是JDK8，当然，使用特性相对较少的JDK7开发也是可以的。对于JDK6而言，虽然Minecraft 1.8.9可以在Java6上运行，但是JDK6相对而言太过陈旧了，所以并不推荐。对于JDK9及更高版本来说，由于Forge不准备对Minecraft 1.12.2及更低版本提供针对Java9及更高版本的支持，因此请不要尝试使用JDK9及更高版本编写Mod。

为了方便开发，你需要一个用于Java开发的IDE（Integrated Development Environment，集成开发环境），如Eclipse、IntelliJIDEA、Netbeans等，这里推荐[Eclipse](http://www.eclipse.org/downloads/)和[IntelliJIDEA](https://www.jetbrains.com/idea/download/)，本教程作者使用的IDE是IntelliJIDEA。

#### 一定程度的不借助JavaDoc的源代码分析能力

Minecraft作为一个商业软件，每一次次版本号更新对于Mod界都是一次灾难，直到现在，仍然有很多优秀的Mod卡在Minecraft1.6.4、Minecraft1.7.10等旧版本。因此，这要求开发者拥有不借助JavaDoc，通过MCP提供的名称进行的源代码分析能力。当然，如果读者怀疑自己没有这方面的能力，那也没有关系，因为我相信通过本教程的学习，读者会慢慢掌握不借助JavaDoc分析源代码的能力的。

#### 对Mod开发的热情、缜密的思维、以及一个足够大而又能正常运作的脑洞

这条很重要！！！

### 本教程为读者提供什么

#### 针对Minecraft 1.8.9版本并基于Forge的Mod开发教程

本教程主要面向的Minecraft版本为1.8.9，本教程也会兼顾到对于Minecraft 1.9、1.9.4、1.10等更高版本对应的Forge的支持。

本教程不讨论1.8、1.7.10及之前的如1.7.2、1.6.4等Minecraft版本，虽然较1.8.9早期的版本的Mod开发和1.8.9的有着很多相通之处，本教程的很多地方也可能会对这些版本的Mod开发提供帮助，但本教程不去探究这些差别。

本教程不讨论基于ModLoader、Liteloader等其他Mod API的开发，以及基于CraftBukkit、Spigot等插件的开发，等等。

虽然本教程的每一句都经过了作者的推敲，但是由于大意疏忽等原因，可能无法发现一些错误，恳情读者斧正。

#### 源代码

对于教程中的所有源代码实现，本教程会尽可能地保证在Minecraft 1.8.9版本、和给定Forge版本下的服务端和客户端下成功运行。本教程也会考虑到诸如Minecraft 1.9.4、1.10.2、1.11.2等更高的Minecraft版本对应的Forge版本，并尽可能使相同的代码在高版本的Forge里正常工作，但不作保证。

对于教程中出现的源代码，读者可能需要加上一些import等辅助语句以使其正常工作。对于教程附带的源代码，本教程已经在Minecraft 1.8.9版本、和给定Forge版本，也就是11.15.1.2318下的客户端和服务端进行了测试。

本教程的所有源代码虽然进行了充分的测试，但是由于作者水平有限，可能无法发现一些错误，恳情读者指出。

### 工作环境的准备

从[Forge官网](http://files.minecraftforge.net/)下载Forge**Mdk**，并**尽可能采用稳定版本**（作者采用的是[11.15.1.2318](http://adfoc.us/serve/sitelinks/?id=271228&url=http://files.minecraftforge.net/maven/net/minecraftforge/forge/1.8.9-11.15.1.2318-1.8.9/forge-1.8.9-11.15.1.2318-1.8.9-mdk.zip)版本），本教程的所有开发操作都基于此。

接下来对于工作环境的配置，教程会在下一部分进行讲解，如果读者希望升级自己的开发环境中Forge的版本，可以参阅[附录](附录A-配置Gradle的一些小技巧.md)。

# 1 基础部分

## 1.1 配置你的工作环境

### 写在前面

基础部分讲述的内容，是几乎所有Mod都会频繁使用的、较为容易实现的特性。

建议读者在阅读完绪论后，对于初级部分的每一部分都加以阅读。

### 配置工作环境

解压在上一部分，也就是绪论中提到的下载到的文件，进入该目录，也就是工作环境的根目录。

如果你使用的是Linux或者Mac OS X，在该目录下运行命令：

```
./gradlew setupDecompWorkspace
```

如果你使用的是Microsoft Windows，则运行：

```
gradlew.bat setupDecompWorkspace
```

后面的所有类似命令、都将描述的是在Linux或者Mac OS X的运行方式，如果是Microsoft Windows用户请将后面的所有命令中的`./gradlew`替换成`gradlew.bat`，教程不再赘述。

可以通过添加参数的方式设置代理，`<host>`表示代理服务器的地址，`<port>`表示开放代理的端口。

使用socks代理的方式：

```
./gradlew setupDecompWorkspace -DsocksProxyHost=<host> -DsocksProxyPort=<port>
```

针对https资源使用http代理的方式：

```
./gradlew setupDecompWorkspace -Dhttps.proxyHost=<host> -Dhttps.proxyPort=<port>
```

针对http资源使用http代理的方式：

```
./gradlew setupDecompWorkspace -Dhttp.proxyHost=<host> -Dhttp.proxyPort=<port>
```

因为资源大多在国外，所以可能要等待一段漫长的时间。建议在网络状况好的地方运行此命令，以配置开发环境，并获取反编译过的Minecraft源代码。如果有条件，建议使用国外的代理。

在此我们提供了一个免费Shadowsocks代理服务器来帮助您构建. 注意此代理只能用来构建Forge无法访问其他墙外网站:

> **server:** forge-build-srar-telecom.ustc-zzzz.net  
> **server\_port:** 65099  
> **password:** 9c4d5a9a6d97  
> **method:** rc4-md5  

Shadowsocks客户端默认监听`127.0.0.1:1080`作为代理服务来运行, 当您配置完Shadowsocks后您可以执行以下命令来使用代理进行构建:

```
./gradlew setupDecompWorkspace -Dhttps.proxyHost=127.0.0.1 -Dhttp.proxyHost=127.0.0.1 -Dhttps.proxyPort=1080 -Dhttp.proxyPort=1080
```

> 推荐使用的Shadowsocks客户端：<https://github.com/shadowsocks/shadowsocks-windows/releases>

由于获取反编译的Minecraft源代码的过程是在自己的计算机上进行的，所以说有时可能出现内存不足的情况。如果运行一次后出现的错误中有`Java Heap Space`的字样，那很可能是因为默认情况下分配的内存不足。这时你可以添加下面的参数手动指定JVM的初始内存，同时禁用[Gradle Daemon](https://docs.gradle.org/current/userguide/gradle_daemon.html)以减少不必要的内存消耗（当然，减少计算机上的其他不必要进程以提高内存可用空间也很重要）。经过作者测试，1.4G的内存足够反编译Minecraft了：

```
./gradlew setupDecompWorkspace -Dorg.gradle.jvmargs=-Xms1440m --no-daemon
```

最后如果出现`BUILD SUCCESSFUL`字样，就代表你第一步的配置成功了，以下所有需要用到`./gradlew`的运行结果都以`BUILD SUCCESSFUL`字样为成功的标志，如果出现了`BUILD FAILED`字样，那么代表尚未成功，可以重新运行相同的命令尝试，如果还是不可以，可以尝试使用上面提到的使用代理的模式，并尝试手动指定JVM的初始内存。

如果你使用Eclipse作为你的IDE，请在上面的命令运行完成后运行命令：

```
./gradlew eclipse
```

如果你使用IntelliJIDEA（IDEA大法好→_→）的话：

```
./gradlew idea
```

使用代理的方式同上。

然后打开IDE，将工程目录切换到这个目录，如果配置成功，IDE会注意到这个目录存在一个工程，并自动找到刚刚配置的这个Mod工程的源代码和资源文件的位置。

默认配置中，Mod工程的源代码在目录`src/main/java`下，而Mod工程的资源文件（如图片、模型等）在目录`src/main/resources`下。读者应该会发现在Mod源代码中有一个`com.example.examplemod`的包，那个是测试用的，删掉就可以了。

### 运行、构建和发布Mod的方法

如果你想构建并发布你的Mod，运行下面的命令：

```
./gradlew build
```

这样根目录下的`build/libs/`里会有一个jar包，那便是你构建完成的Mod文件。

如果想要运行客户端，运行：

```
./gradlew runClient
```

如果想要运行服务端，运行：

```
./gradlew runServer
```

客户端和服务端的运行文件将在根目录的`eclipse`文件夹中。

上面三个命令在非特殊情况下，强烈建议添加`--offline`参数，阻止与外界网络的连接，即对国外的资源的访问，以提高速度。

当然，你也可以在你的IDE中运行、或者调试你的Mod。

对于Eclipse而言，Forge在针对IDE配置的时候，已经添加了相关的运行选项。不过对于IntelliJIDEA而言，你可能需要多运行一条命令：

```
./gradlew genIntellijRuns
```

然后你的IntelliJIDEA中就应该有运行选项了。

### Mod的唯一标识符

每个Mod都会有一串唯一标识符用于与其他Mod相区分，这个标识符我们通常也称为modid。

**modid请全部使用小写字母，并且不要使用除英文字母和下划线外的其他符号**。实际上，Forge从Minecraft 1.11对应的某个Forge版本开始就不支持含有大写字母的modid了（同时modid的长度不得超过64个字符，不过应该没人会起那么长的modid）。当然，你可以使用小写加下划线的`snake_case`形式，事实上我们也推荐使用这一形式。

modid一旦决定，就不能轻易改动，因为其他Mod很可能根据这一唯一标识符来识别你的Mod，如果名称发生更改则可能带来意料之外的影响，所以考虑modid时一定要慎重。

### Forge是如何管理工程的

很明显，Mod的管理、构建都十分复杂，所以Forge使用了一个被称为[Gradle](https://gradle.org/)的工具，还通过编写插件的方式对其进行了[修改](https://github.com/MinecraftForge/ForgeGradle)。Gradle本身是一个比较强大的工程构建工具，它本身集成了很多的编译、构建选项，却又十分简单（相对[Maven](https://maven.apache.org/)来说）。

在你的根目录下应该有一个文件名为`bulid.gradle`的文件（为了节省篇幅，这里删掉了所有的注释）：

**`build.gradle:`**

```
buildscript {
    repositories {
        jcenter()
        maven {
            name = "forge"
            url = "http://files.minecraftforge.net/maven"
        }
    }
    dependencies {
        classpath 'net.minecraftforge.gradle:ForgeGradle:2.1-SNAPSHOT'
    }
}
apply plugin: 'net.minecraftforge.gradle.forge'

version = "1.0"
group= "com.yourname.modid"
archivesBaseName = "modid"

minecraft {
 version = "1.8.9-11.15.1.2318-1.8.9"
 runDir = "run"
 
 mappings = "stable_20"
}

dependencies {

}

processResources
{
 inputs.property "version", project.version
 inputs.property "mcversion", project.minecraft.version

 from(sourceSets.main.resources.srcDirs) {
     include 'mcmod.info'
             
     expand 'version':project.version, 'mcversion':project.minecraft.version
 }
     
 from(sourceSets.main.resources.srcDirs) {
     exclude 'mcmod.info'
 }
}
```

这里，我们找到其中的三行：

**`build.gradle（部分）:`**

```
version = "1.0"
group= "com.yourname.modid"
archivesBaseName = "modid"
```

并更改掉它：

**`build.gradle（部分）:`**

```
version = "1.0.0"
group = "com.github.ustc_zzzz"
archivesBaseName = "fmltutor"
```

`version`表示该Mod的版本。关于版本的设置，可以参照一个GitHub推荐的，被称为[语义化版本](http://semver.org/lang/zh-CN/)的标准（[英文原版](http://semver.org/)），按照这个标准，作为第一个正式版本，显然应该是1.0.0。

`group`表示组id，建议使用Java包命名规范，比如如果你这个项目所在网站为`www.example.com`，那么包名建议为：

```
com.example.<your_username>.<your_project_name>
```

比如本教程的所有代码都在这个包下：

```
com.github.ustc_zzzz.fmltutor
```

当然如果没有网站，也可以直接使用用户名：

```
<your_username>.<your_project_name>
```

比如对于本教程的代码，可以这样命名：

```
ustc_zzzz.fmltutor
```

当然，也有直接以Mod名称作为包命名的，等等。

总而言之，包的命名以不冲突为前提。

`archivesBaseName`表示生成的Mod包使用的名称，Mod包使用的文件名是“名称+横线+版本号.jar”，这里就是`fmltutor-1.0.jar`。

Gradle本身有一个详细的[User Guide](https://docs.gradle.org/current/userguide/userguide.html)，如果读者对进一步的配置感兴趣，可以仔细阅读。

对于Mod进一步的配置，我们会在下一部分中提及。

## 1.2 主类、代理和Mod信息

### 新建一个主类

新建一个包（这里是`com.github.ustc_zzzz.fmltutor`），并在其中新建一个类（**强烈建议这个类的类名和你的Mod名称相同**），这就是这个Mod的主类了。

把下面的代码抄进你新建的类中，下面我会解释为什么要这么做。

**`src/main/java/com/github/ustc_zzzz/fmltutor/FMLTutor.java:`**

```java
package com.github.ustc_zzzz.fmltutor;

import com.github.ustc_zzzz.fmltutor.common.CommonProxy;

import net.minecraftforge.fml.common.Mod;
import net.minecraftforge.fml.common.Mod.EventHandler;
import net.minecraftforge.fml.common.Mod.Instance;
import net.minecraftforge.fml.common.SidedProxy;
import net.minecraftforge.fml.common.event.FMLInitializationEvent;
import net.minecraftforge.fml.common.event.FMLPostInitializationEvent;
import net.minecraftforge.fml.common.event.FMLPreInitializationEvent;

/**
 * @author ustc_zzzz
 */
@Mod(modid = FMLTutor.MODID, name = FMLTutor.NAME, version = FMLTutor.VERSION, acceptedMinecraftVersions = "1.8.9")
public class FMLTutor
{
    public static final String MODID = "fmltutor";
    public static final String NAME = "FML Tutor";
    public static final String VERSION = "1.0.0";

    @Instance(FMLTutor.MODID)
    public static FMLTutor instance;

    @EventHandler
    public void preInit(FMLPreInitializationEvent event)
    {
        // TODO
    }

    @EventHandler
    public void init(FMLInitializationEvent event)
    {
        // TODO
    }

    @EventHandler
    public void postInit(FMLPostInitializationEvent event)
    {
        // TODO
    }
}
```

`package`、`import`部分含义显而易见。

**`src/main/java/com/github/ustc_zzzz/fmltutor/FMLTutor.java（部分）:`**

```java
@Mod(modid = FMLTutor.MODID, name = FMLTutor.NAME, version = FMLTutor.VERSION, acceptedMinecraftVersions = "1.8.9")
```

上面这一行是Java从JDK1.5引入的一种机制：注解。注解的功能类似于代码中的注释，不同的是，注解不仅提供代码功能的说明，还是实现程序功能的重要组成部分。Java注解已经在很多框架中得到了广泛的使用，用来简化程序中的配置。

比如下面三个最常见的注解：

* `@Override` 用于声明该方法覆写了父类方法，使用该注解，如果（由于拼写错误等）没有覆写，编译器会报错
* `@SuppressWarning` 用于显式忽略警告，使用该注解，编译器不会产生对应代码的警告
* `@Deprecated` 用于标记一个方法不应该再被使用，使用该注解，如果在代码中调用了该方法，编译器会产生一个警告

因为注解中可以添加键-值对，所以动态查看一个注解的内容是有意义的，也是可行的。比如这个注解，FML在加载这个Mod的时候，就会去自动寻找含有`@Mod`注解的类，并读取下面的数据：

* `modid`指的就是该Mod的唯一标识符
* `name`指的是该Mod的名称
* `version`指的是该Mod的版本号，在Mod间的依赖关系时可能会用作识别
* `acceptedMinecraftVersions`指的是Mod接受的Minecraft版本，当版本不对时，FML会优雅地抛出一个错误而不是继续加载这个Mod

`acceptedMinecraftVersions`约定的版本声明如下：

* `1.8.9`（本教程）表示该Mod只支持Minecraft 1.8.9
* `[1.8,1.9)`表示该Mod支持从1.8（包含）到1.9（不包含）的所有Minecraft版本
* `[1.8,1.10]`表示该Mod支持从1.8（包含）到1.10（包含）的所有Minecraft版本
* `[1.8,)`表示该Mod支持从1.8（包含）之后出现的所有Minecraft版本
* `(,1.8],[1.9,)`表示该Mod支持1.8（包含）之前出现的所有Minecraft版本和从1.9（包含）之后出现的所有Minecraft版本

当然这个注解也有着其他数据（没有声明的数据作为默认值），比如该Mod依赖于什么Mod等等，有些比较有必要的数据会在后面部分讲到。

下面的三个方法带有`@EventHandler`注解，它们的作用也是类似。Forge在找到这个类后，会检查这个类中所有含有`@EventHandler`注解的方法，并通过方法的参数类型来判定到底应该在何时调用它们：

* 含有`FMLPreInitializationEvent`参数的方法（这里是`preInit`）在所有Mod初始化之前调用，**这时候应该加载配置文件，实例化物品和方块，并注册它们**。
* 含有`FMLInitializationEvent`参数的方法（这里是`init`）用于该Mod的初始化，**这时候应该为Mod进行设置，如注册合成表和烧炼系统，并且向其他Mod发送交互信息**。
* 含有`FMLPostInitializationEvent`参数的方法（这里是`postInit`）在所有Mod都初始化之后调用，**这时候应该接收其他Mod发送的交互信息，并完成对Mod的设置**。

有些Mod会把注册方块、物品等等操作放在Mod初始化阶段完成，**这种做法是不推荐的，Forge推荐在`preInit`阶段完成**。

`@Instance`注解的作用是将生成的该Mod的实例，注册到对应的Mod的id，同时，也可以访问其他Mod的id对应的实例，当然，**这里的id要和本Mod的id相同**。

### 代理？那是什么

众所周知，Minecraft Mod有客户端和服务端两种使用方式，而两种方式的差异足够大使得Mod需要采用两种初始化方式，而两种方式的差异又足够小使得Mod没有必要制作客户端和服务端两个版本。这时候代理便起到了区别两种初始化方式的作用。**在单机运行时，Minecraft也会生成一个本地服务端**。服务端和客户端之间的差异十分复杂，甚至很多都只是经验之谈，然而有一点往往是通用的，**服务端的代码，往往客户端都会执行**。

在主类中添加以下代码：

**`src/main/java/com/github/ustc_zzzz/fmltutor/FMLTutor.java（部分）:`**

```java
    @SidedProxy(clientSide = "com.github.ustc_zzzz.fmltutor.client.ClientProxy", 
            serverSide = "com.github.ustc_zzzz.fmltutor.common.CommonProxy")
    public static CommonProxy proxy;
```

Forge会在加载Mod的时候自动使用上面的类名对这个代理进行实例化。

当然，我们需要创建上面所说的`CommonProxy`，和`ClientProxy`。

新建一个包`com.github.ustc_zzzz.fmltutor.common`，在其中新建一个类`CommonProxy`：

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/CommonProxy.java:`**

```java
package com.github.ustc_zzzz.fmltutor.common;

import net.minecraftforge.fml.common.event.FMLInitializationEvent;
import net.minecraftforge.fml.common.event.FMLPostInitializationEvent;
import net.minecraftforge.fml.common.event.FMLPreInitializationEvent;

public class CommonProxy
{
    public void preInit(FMLPreInitializationEvent event)
    {

    }

    public void init(FMLInitializationEvent event)
    {

    }

    public void postInit(FMLPostInitializationEvent event)
    {

    }
}
```

新建包`com.github.ustc_zzzz.fmltutor.client`，新建类`ClientProxy`，并继承类`CommonProxy`：

**`src/main/java/com/github/ustc_zzzz/fmltutor/client/ClientProxy.java:`**

```java
package com.github.ustc_zzzz.fmltutor.client;

import com.github.ustc_zzzz.fmltutor.common.CommonProxy;

import net.minecraftforge.fml.common.event.FMLInitializationEvent;
import net.minecraftforge.fml.common.event.FMLPostInitializationEvent;
import net.minecraftforge.fml.common.event.FMLPreInitializationEvent;


public class ClientProxy extends CommonProxy
{
    @Override
    public void preInit(FMLPreInitializationEvent event)
    {
        super.preInit(event);
    }

    @Override
    public void init(FMLInitializationEvent event)
    {
        super.init(event);
    }

    @Override
    public void postInit(FMLPostInitializationEvent event)
    {
        super.postInit(event);
    }
}
```

主类的调整：

**`src/main/java/com/github/ustc_zzzz/fmltutor/FMLTutor.java（部分）:`**

```java
    @EventHandler
    public void preInit(FMLPreInitializationEvent event)
    {
        proxy.preInit(event);
    }

    @EventHandler
    public void init(FMLInitializationEvent event)
    {
        proxy.init(event);
    }

    @EventHandler
    public void postInit(FMLPostInitializationEvent event)
    {
        proxy.postInit(event);
    }
```

很明显，当服务端被初始化时，`CommonProxy`类中对应方法会被调用，如果是客户端，`ClientProxy`类中对应方法会被调用，这样我们就可以实现服务端和客户端的差异。

### 完善你的Mod信息

一个Mod的信息在其jar根目录下的`mcmod.info`文件里，这里是`src/main/resources/mcmod.info`，打开就可以完善你的Mod信息。**注意：`version`和`mcversion`字段不应修改，它们会在Gradle构建Mod的时候被自动替换掉**。你应该更改`build.gradle`文件。

比如，本教程的`mcmod.info`文件是这个样子的：

**`src/main/resources/mcmod.info:`**

```json
[
{
  "modid": "fmltutor",
  "name": "FML Tutor",
  "description": "A Minecraft 1.8 Forge Mod Loader Tutorial by ustc_zzzz.",
  "version": "${version}",
  "mcversion": "${mcversion}",
  "url": "https://github.com/ustc-zzzz/fmltutor/wiki",
  "updateUrl": "https://github.com/ustc-zzzz/fmltutor/tags",
  "authorList": ["ustc_zzzz"],
  "credits": "Notch, Cpw, etc.",
  "logoFile": "",
  "screenshots": [],
  "dependencies": []
}
]
```

## 1.3 第一个物品

### 制作一个物品一共分几步

制作一个物品一共分三步：

1. 创建一个物品
2. 实例化并注册这个物品
3. 为这个物品添加模型和材质

Minecraft中，使用八个金锭和一个苹果可以创造一个金苹果，那我们是不是可以创造一个金蛋呢，这一次教程会一步一步地带着你完成制作新物品的全过程。

### 创建一个物品

如果读者翻看了`net.minecraft.item`包，想必就会发现你在Minecraft中遇到的各种物品，都继承了`Item`类，那很明显，我们制作的物品也要继承这个类。

新建一个包`com.github.ustc_zzzz.fmltutor.item`，在其中创建一个类`ItemGoldenEgg`：

**`src/main/java/com/github/ustc_zzzz/fmltutor/item/ItemGoldenEgg.java:`**

```java
package com.github.ustc_zzzz.fmltutor.item;

import net.minecraft.item.Item;

public class ItemGoldenEgg extends Item
{
    public ItemGoldenEgg()
    {
        super();
        this.setUnlocalizedName("goldenEgg");
    }
}
```

这里的`setUnlocalizedName`方法为该物品添加了一个非本地化的名称，该名称为“`item.`”+设置的名称，比如这里就是`item.goldenEgg`，这个非本地化名称，与本地化和国际化有关，在后面的部分我们会讲到。**非本地化名称尽量使用小写驼峰式写法，即第一个词以小写字母开始，第二个词开始首字母大写，中间不使用任何符号分隔**。

### 实例化并注册这个物品

在`CommonProxy`类中添加下面的代码：

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/CommonProxy.java（部分）:`**

```java
    public void preInit(FMLPreInitializationEvent event)
    {
        new ItemLoader(event);
    }
```

新建一个类`ItemLoader`：

**`src/main/java/com/github/ustc_zzzz/fmltutor/item/ItemLoader.java:`**

```java
package com.github.ustc_zzzz.fmltutor.item;

import net.minecraft.item.Item;
import net.minecraftforge.fml.common.event.FMLPreInitializationEvent;
import net.minecraftforge.fml.common.registry.GameRegistry;

public class ItemLoader
{
    public static Item goldenEgg = new ItemGoldenEgg();

    public ItemLoader(FMLPreInitializationEvent event)
    {
        register(goldenEgg, "golden_egg");
    }

    private static void register(Item item, String name)
    {
        GameRegistry.registerItem(item.setRegistryName(name));
    }
}
```

新建一个类进行初始化的原因是当你的物品越来越多，如果所有的注册过程都直接在`CommonProxy`类中进行，随着注册的对象越来越多，这些注册的对象会越来越难以管理。换句话说，这体现了代码模块化的原则。

首先，我们要实例化这个物品：

**`src/main/java/com/github/ustc_zzzz/fmltutor/item/ItemLoader.java（部分）:`**

```java
    public static Item goldenEgg = new ItemGoldenEgg();
```

然后，我们来到了这块的重点，注册这个物品：

**`src/main/java/com/github/ustc_zzzz/fmltutor/item/ItemLoader.java（部分）:`**

```java
        GameRegistry.registerItem(item.setRegistryName(name));
```

`GameRegistry`是Forge提供的一个用来注册物品、方块、合成表、烧炼规则等各种常见内容的类，比如下面的用于注册的方法我们在后面都会遇到并加以讲解：

* `registerBlock`方法用于注册方块
* `registerFuelHandler`方法用于注册燃料
* `registerItem`方法用于注册物品
* `registerTileEntity`方法用于注册TileEntity（后面会讲到什么是TileEntity）
* `registerWorldGenerator`方法用于注册世界生成器以生成不同的世界
* `addRecipe`方法和`addShapedRecipe`方法用于注册合成表
* `addSmelting`方法用于注册物品烧炼规则

这个方法需要传入一个`Item`类的实例用于注册物品，那么如何指定这个物品的id呢？在示例中，我们通过调用物品的`setRegistryName`方法指定了物品的id，这也是从1.8.9开始指定物品id的通用做法。

有一点需要注意，从Minecraft的1.9及以上版本开始，`GameRegistry`类中相应的方法名被修改为了`register`方法，因此注册物品时需要用到的代码会有一点微小的变动。

我们这里通过参数提供物品的id。**id请尽量使用小写字母加下划线，并且同一个Mod下的物品id不能相同**，有的Mod会使用驼峰式，这样的好处是把物品的非本地化名称和物品id设置成相同的，**但是我们不推荐这样的做法**。

现在不管是在服务端，还是在客户端，Forge都会在`preInit`阶段，运行到`ItemLoader`类中的构造函数，也就是实例化一个物品，并注册它。

现在运行客户端，运行命令：

```
/give @a fmltutor:golden_egg
```

玩家的手上便多了一个新的物品。

### 为这个物品添加模型和材质

可以看到，你手上的物品，现在还什么都没有，只是一个两种颜色交替的方块，这是因为你没有添加模型和材质。模型的作用是保证你手里的物品是一个扁平的长方体，而材质的作用，就是给这个长方体上色。

首先，新建一个文件夹：`src/main/resources/assets/fmltutor/models/item`，并在其中新建一个文件：`golden_egg.json`：

**`src/main/resources/assets/fmltutor/models/item/golden_egg.json:`**

```json
{
    "parent": "builtin/generated",
    "textures": {
        "layer0": "fmltutor:items/golden_egg"
    },
    "display": {
        "thirdperson": {
            "rotation": [ -90, 0, 0 ],
            "translation": [ 0, 1, -2 ],
            "scale": [ 0.55, 0.55, 0.55 ]
        },
        "firstperson": {
            "rotation": [ 0, -135, 25 ],
            "translation": [ 0, 4, 2 ],
            "scale": [ 1.7, 1.7, 1.7 ]
        }
    }
}
```

当然，这里的`fmltutor`就是Mod id，`golden_egg`就是你的物品id。

这个json的文件，就是这个物品的模型，这个文件的内容解释起来极其复杂，这也不是教程负责的介绍内容，直接抄下来就好了，但是有的地方是显而易见的：

**`src/main/resources/assets/fmltutor/models/item/golden_egg.json（部分）:`**

```json
    "textures": {
        "layer0": "fmltutor:items/golden_egg"
    },
```

这一部分告诉我们的是这个物品材质的位置，也就是`fmltutor:items/golden_egg`，很明显，我们需要建立一个材质文件。这里使用的是16x16的材质文件（当然Minecraft也支持尺寸更大如32x32的材质文件，不过建议还是使用16x16的），新建文件夹`src/main/resources/assets/fmltutor/textures/items`，把制作完成的golden_egg.png放入（其实是我照着鸡蛋的原图嗐改的=_=||）：

**`src/main/resources/assets/fmltutor/textures/items/golden_egg.png:`**

![golden_egg](golden_egg.png)

所有模型和材质都准备好了，现在需要做的，就是**让Minecraft知道你准备的模型和材质**了。

修改`ItemLoader`类的内容：

**`src/main/java/com/github/ustc_zzzz/fmltutor/item/ItemLoader.java:`**

```java
package com.github.ustc_zzzz.fmltutor.item;

import net.minecraft.client.resources.model.ModelResourceLocation;
import net.minecraft.item.Item;
import net.minecraftforge.client.model.ModelLoader;
import net.minecraftforge.fml.common.event.FMLPreInitializationEvent;
import net.minecraftforge.fml.common.registry.GameRegistry;
import net.minecraftforge.fml.relauncher.Side;
import net.minecraftforge.fml.relauncher.SideOnly;

public class ItemLoader
{
    public static Item goldenEgg = new ItemGoldenEgg();

    public ItemLoader(FMLPreInitializationEvent event)
    {
        register(goldenEgg, "golden_egg");
    }

    @SideOnly(Side.CLIENT)
    public static void registerRenders()
    {
        registerRender(goldenEgg);
    }

    private static void register(Item item, String name)
    {
        GameRegistry.registerItem(item.setRegistryName(name));
    }

    @SideOnly(Side.CLIENT)
    private static void registerRender(Item item)
    {
        ModelResourceLocation model = new ModelResourceLocation(item.getRegistryName(), "inventory");
        ModelLoader.setCustomModelResourceLocation(item, 0, model);
    }
}
```

我们来看这一段：

**`src/main/java/com/github/ustc_zzzz/fmltutor/item/ItemLoader.java（部分）:`**

```java
    @SideOnly(Side.CLIENT)
    private static void registerRender(Item item)
    {
        ModelResourceLocation model = new ModelResourceLocation(item.getRegistryName(), "inventory");
        ModelLoader.setCustomModelResourceLocation(item, 0, model);
    }
```

Forge提供了`ModelLoader`类用于加载和处理模型，其`setCustomModelResourceLocation`方法有三个参数：

* 第一个参数是要被注册的物品。
* 第二个参数是这个物品的Metadata。Metadata是一个用于区分同一个物品或方块的不同状态的数据，比如钟表的十六种状态、羊毛的十六种颜色，在3.2.1节会讲到Metadata，默认为零就好了。
* 第三个参数就是这个物品模型的资源位置了，资源位置是类`ModelResourceLocation`的一个实例，它用于描述一个模型，在后面我们还会比较常用到这个类的。

`ModelResourceLocation`被用于标注模型的位置，通常为由冒号（`:`）和井号（`#`）分隔的三个字符串组成，对于我们这里构造的`ModelResourceLocation`，它的一部分通过调用物品的`getRegistryName`方法得到，第二部分由我们指定，为`inventory`，是一个固定的字符串，代表作为一个物品的渲染模型。

在这里，第一部分为`fmltutor:golden_egg`，第二部分为`inventory`，组合后的`ModelResourceLocation`就是`fmltutor:golden_egg#inventory`。Minecraft便会去相应的目录下寻找相应的资源：

* `fmltutor`指示游戏应该在`assets.fmltutor`包下找到这个资源
* `inventory`指示游戏应该在`assets.fmltutor.models.item`包下找到这个资源
* `golden_egg`指示这个资源就是`assets.fmltutor.models.item.golden_egg.json`，对应到源代码，就是`src/main/resources/assets/fmltutor/models/item/golden_egg.json`这一文件

当然，上面已经提到了，`golden_egg.json`模型文件里已经包含了材质的信息。

`@SideOnly`注解的作用是注解这一方法、类等只作用于客户端或服务端。很明显，对于模型和材质的操作只会在客户端执行（实际上如果在服务端执行会出错），所以我们同时要在**`ClientProxy`的`preInit`阶段**中初始化：

**`src/main/java/com/github/ustc_zzzz/fmltutor/client/ClientProxy.java:`**

```java
package com.github.ustc_zzzz.fmltutor.client;

import com.github.ustc_zzzz.fmltutor.common.CommonProxy;

import net.minecraftforge.fml.common.event.FMLInitializationEvent;
import net.minecraftforge.fml.common.event.FMLPostInitializationEvent;
import net.minecraftforge.fml.common.event.FMLPreInitializationEvent;

public class ClientProxy extends CommonProxy
{
    @Override
    public void preInit(FMLPreInitializationEvent event)
    {
        super.preInit(event);
        new ItemRenderLoader();
    }

    @Override
    public void init(FMLInitializationEvent event)
    {
        super.init(event);
    }

    @Override
    public void postInit(FMLPostInitializationEvent event)
    {
        super.postInit(event);
    }
}
```

在`com.github.ustc_zzzz.fmltutor.client`下新建`ItemRenderLoader`类：

**`src/main/java/com/github/ustc_zzzz/fmltutor/client/ItemRenderLoader.java:`**

```java
package com.github.ustc_zzzz.fmltutor.client;

import com.github.ustc_zzzz.fmltutor.item.ItemLoader;

public class ItemRenderLoader
{
    public ItemRenderLoader()
    {
        ItemLoader.registerRenders();
    }
}
```

现在在客户端，Forge会在`preInit`阶段，运行到`ItemRenderLoader`类的构造函数，进而运行到`ItemLoader`类中的`registerRenders`方法中的代码，也就是注册这个物品的渲染，而在服务端则不会运行。

现在运行游戏，你的手上是不是有了一个金色的蛋啦～

最后说一句，把所有只在客户端执行的代码放到同一个`client`文件夹下是一个好的习惯。

## 1.4 第一个方块

### 制作一个方块一共分几步

制作一个方块一共只比物品多一步：

1. 创建一个方块
2. 实例化并注册这个方块
3. 为这个方块对应的物品添加模型和材质
4. 为这个方块添加模型和材质

我们发现雪可以合成雪块，那么，草可不可以合成草块呢= =本次教程将带领你一步一步地做出一个草块。

### 创建一个方块

创建一个方块的过程，和创建一个物品的过程非常相似。

这里我们如法炮制，新建一个包`com.github.ustc_zzzz.fmltutor.block`，并新建文件`BlockGrassBlock.java`，在其中创建一个类，使其继承方块类：

**`src/main/java/com/github/ustc_zzzz/fmltutor/block/BlockGrassBlock.java:`**

```java
package com.github.ustc_zzzz.fmltutor.block;

import net.minecraft.block.Block;
import net.minecraft.block.material.Material;

public class BlockGrassBlock extends Block
{
    public BlockGrassBlock()
    {
        super(Material.ground);
        this.setUnlocalizedName("grassBlock");
        this.setHardness(0.5F);
        this.setStepSound(soundTypeGrass);
    }
}
```

一个方块初始化的时候和物品有一点不同，例如需要设定方块的材质，这里设定成和泥土一样的材质。

当然，就像上面那样，方块往往有很多需要设定的性质，现将一些常见的设定方法列举如下：

* `setBlockUnbreakable`方法用于设定方块的硬度为-1，即不能损坏。
* `setHardness`方法用于设定方块的硬度，如黑曜石是50，铁块5，金块3，圆石2，石头1.5，南瓜1，泥土0.5，甘蔗0，基岩-1。
* `setHarvestLevel`方法用于设定方块的可挖掘等级，如钻石镐是3，铁2，石1，木金0。
* `setLightLevel`方法用于设定方块的光照，其周围的光照为设定值x15，如岩浆1.0，对应15，红石火把0.5，对应7.5。
* `setLightOpacity`方法用于设定方块的透光率，数值越大透光率越低，如树叶和蜘蛛网是1，水和冰3。
* `setResistance`方法用于设定方块的爆炸抗性，如木头的抗性为4，石头为10，黑曜石为2000，基岩为6000000。
* `setStepSound`方法用于设定走在方块上的响声。
* `setTickRandomly`方法用于设定方块是否会接受随机Tick（如农作物）。

### 实例化并注册这个方块

同样，在`CommonProxy`类中添加下面的代码：

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/CommonProxy.java（部分）:`**

```java
    public void preInit(FMLPreInitializationEvent event)
    {
        new ItemLoader(event);
        new BlockLoader(event);
    }
```

新建一个类`BlockLoader`，以完成对应的方块的注册：

**`src/main/java/com/github/ustc_zzzz/fmltutor/block/BlockLoader.java:`**

```java
package com.github.ustc_zzzz.fmltutor.block;

import net.minecraft.block.Block;
import net.minecraftforge.fml.common.event.FMLPreInitializationEvent;
import net.minecraftforge.fml.common.registry.GameRegistry;

public class BlockLoader
{
    public static Block grassBlock = new BlockGrassBlock();

    public BlockLoader(FMLPreInitializationEvent event)
    {
        register(grassBlock, "grass_block");
    }

    private static void register(Block block, String name)
    {
        GameRegistry.registerBlock(block.setRegistryName(name));
    }
}
```

有一点需要注意，从Minecraft的1.9及以上版本开始，除和物品一样，`GameRegistry`类中相应的方法名被修改为了`register`方法之外，方块对应的物品需要手动实例化一个`ItemBlock`去注册。关于`ItemBlock`的更多内容，请参阅3.2.1节。

现在不管是在服务端，还是在客户端，Forge都会在`preInit`阶段，运行到`BlockLoader`类的构造方法中的代码，实例化一个方块并注册它。

### 为这个方块对应的物品添加模型和材质

和物品一样，我们现在扩充一下`BlockLoader`类的代码：

**`src/main/java/com/github/ustc_zzzz/fmltutor/block/BlockLoader.java:`**

```java
package com.github.ustc_zzzz.fmltutor.block;

import net.minecraft.block.Block;
import net.minecraft.client.resources.model.ModelResourceLocation;
import net.minecraft.item.Item;
import net.minecraftforge.client.model.ModelLoader;
import net.minecraftforge.fml.common.event.FMLPreInitializationEvent;
import net.minecraftforge.fml.common.registry.GameRegistry;
import net.minecraftforge.fml.relauncher.Side;
import net.minecraftforge.fml.relauncher.SideOnly;

public class BlockLoader
{
    public static Block grassBlock = new BlockGrassBlock();

    public BlockLoader(FMLPreInitializationEvent event)
    {
        register(grassBlock, "grass_block");
    }

    @SideOnly(Side.CLIENT)
    public static void registerRenders()
    {
        registerRender(grassBlock);
    }

    private static void register(Block block, String name)
    {
        GameRegistry.registerBlock(block.setRegistryName(name));
    }

    @SideOnly(Side.CLIENT)
    private static void registerRender(Block block)
    {
        ModelResourceLocation model = new ModelResourceLocation(block.getRegistryName(), "inventory");
        ModelLoader.setCustomModelResourceLocation(Item.getItemFromBlock(block), 0, model);
    }
}
```

由于注册的是方块对应的物品的模型和材质，所以就如上面的代码描述的一样，和物品唯一不一样的地方就是，我们通过`Item`类的静态方法`getItemFromBlock`获取方块对应的物品，其他的和物品相同。

接下来的事情也十分顺理成章，只不过这里有一些微小的变动。

我们这次先新建一个文件夹：`src/main/resources/assets/fmltutor/models/block`，并在其中新建一个文件：`grass_block.json`：

**`src/main/resources/assets/fmltutor/models/block/grass_block.json:`**

```json
{
    "parent": "block/cube_all",
    "textures": {
        "all": "fmltutor:blocks/grass_block"
    }
}
```

在`src/main/resources/assets/fmltutor/models/item`里新建文件：`grass_block.json`：

**`src/main/resources/assets/fmltutor/models/item/grass_block.json:`**

```json
{
    "parent": "fmltutor:block/grass_block",
    "display": {
        "thirdperson": {
            "rotation": [ 10, -45, 170 ],
            "translation": [ 0, 1.5, -2.75 ],
            "scale": [ 0.375, 0.375, 0.375 ]
        }
    }
}
```

细心的读者可能发现，下面的文件中出现了一个名为`parent`的字段，表示的含义似乎是继承了上面的文件，本部分的稍后面我们会讲为何要这么做。

然后我们新建文件夹`src/main/resources/assets/fmltutor/textures/blocks`，在其中创建尺寸同样为16x16的图片文件`grass_block.png`（其实也仅仅是干草堆调了个色=_=||）：

**`src/main/resources/assets/fmltutor/textures/blocks/grass_block.png:`**

![grass_block](grass_block.png)

对`ItemRenderLoader`的修改：

**`src/main/java/com/github/ustc_zzzz/fmltutor/client/ItemRenderLoader.java:`**

```java
package com.github.ustc_zzzz.fmltutor.client;

import com.github.ustc_zzzz.fmltutor.item.ItemLoader;

public class ItemRenderLoader
{
    public ItemRenderLoader()
    {
        ItemLoader.registerRenders();
        BlockLoader.registerRenders();
    }
}
```

现在在客户端，Forge就会在初始化Mod的时候，运行到`BlockLoader`类中的`registerRenders`方法中的代码，注册这个方块对应物品的渲染，而在服务端则不会运行。

现在运行游戏，并在其中运行命令：

```
/give @a fmltutor:grass_block
```

玩家的手上是不是多了一个新的方块呢～

### 为这个方块添加模型和材质

但是，当方块被放到地上的时候，我们会发现，方块并没有显示出应有的样子，而只是一个两种颜色交替的方块。这是因为刚刚我们仅仅注册了方块对应物品的模型和材质，而没有注册方块本身的模型和材质。

这里就多出来了一个问题，为什么Minecraft要分开注册方块和其对应物品的材质呢？

这是因为Minecraft往往一个方块有多种状态，如一个漏斗，就有漏斗的口向下、向北、向东、向南、向西五种状态，一个火焰甚至有三千多种状态，而这些状态，每个的模型都不一样。

当然，如何指定一个超过一个状态的方块、为一个方块指定多个物品、甚至不指定物品等，后面的部分会有讲到，这里我们不作探讨。

Minecraft会将方块的状态和模型之间的关系信息放在`assets.minecraft.blockstates`文件夹下，同样，Minecraft会自动寻找对应的存放方块状态的文件夹，比如这里就是`assets.fmltutor.blockstates`，也就是`src/main/resources/assets/fmltutor/blockstates`文件夹，如果没有特殊设置，再在这个文件夹下寻找文件名和`<方块id>.json`相同的文件。

我们新建这样一个文件夹，并在其中新建一个文件`grass_block.json`：

**`src/main/resources/assets/fmltutor/blockstates/grass_block.json:`**

```json
{
    "variants": {
        "normal": { "model": "fmltutor:grass_block" }
    }
}
```

这个文件告诉游戏，这个方块使用`assets.fmltutor.models.block`包下的一个名为`grass_block.json`的文件作为模型，这也是物品模型被拆分成两个文件的原因。

现在打开游戏，放在地上的方块，是不是成功地渲染了呢～

最后说一句，把所有物品相关的类使用Item开头，所有方块相关的类使用Block开头，等等，并把它们放到对应的包里，是一个好的习惯。

## 1.5 创造模式物品栏

### 将你的物品和方块放入创造模式物品栏

其实这很简单，只要在物品和方块初始化的时候加上一句就好了：

**`src/main/java/com/github/ustc_zzzz/fmltutor/block/BlockGrassBlock.java:`**

```java
package com.github.ustc_zzzz.fmltutor.block;

import net.minecraft.block.Block;
import net.minecraft.block.material.Material;
import net.minecraft.creativetab.CreativeTabs;

public class BlockGrassBlock extends Block
{
    public BlockGrassBlock()
    {
        super(Material.ground);
        this.setUnlocalizedName("grassBlock");
        this.setHardness(0.5F);
        this.setStepSound(soundTypeGrass);
        this.setCreativeTab(CreativeTabs.tabBlock);
    }
}
```

构造函数的最后一句：

**`src/main/java/com/github/ustc_zzzz/fmltutor/block/BlockGrassBlock.java（部分）:`**

```java
        this.setCreativeTab(CreativeTabs.tabBlock);
```

把这个方块放到了名为“方块”的创造模式物品栏里。

### 新建一个创造模式物品栏

Minecraft的所有物品栏都是`CreativeTabs`类的子类，我们首先新建包`com.github.ustc_zzzz.fmltutor.creativetab`，并在其下新建类`CreativeTabsFMLTutor`，使其继承`CreativeTabs`类：

**`src/main/java/com/github/ustc_zzzz/fmltutor/creativetab/CreativeTabsFMLTutor.java:`**

```java
package com.github.ustc_zzzz.fmltutor.creativetab;

import com.github.ustc_zzzz.fmltutor.item.ItemLoader;

import net.minecraft.creativetab.CreativeTabs;
import net.minecraft.item.Item;

public class CreativeTabsFMLTutor extends CreativeTabs
{
    public CreativeTabsFMLTutor()
    {
        super("fmltutor");
    }

    @Override
    public Item getTabIconItem()
    {
        return ItemLoader.goldenEgg;
    }
}
```

`getTabIconItem`方法，返回的是创造模式物品栏上显示的物品。

新建包`com.github.ustc_zzzz.fmltutor.creativetab`并在其下新建类`CreativeTabsLoader`：

**`src/main/java/com/github/ustc_zzzz/fmltutor/creativetab/CreativeTabsLoader.java:`**

```java
package com.github.ustc_zzzz.fmltutor.creativetab;

import net.minecraft.creativetab.CreativeTabs;
import net.minecraftforge.fml.common.event.FMLPreInitializationEvent;

public class CreativeTabsLoader
{
    public static CreativeTabs tabFMLTutor;

    public CreativeTabsLoader(FMLPreInitializationEvent event)
    {
        tabFMLTutor = new CreativeTabsFMLTutor();
    }
}
```

并将物品注册进去：

**`src/main/java/com/github/ustc_zzzz/fmltutor/item/ItemGoldenEgg.java:`**

```java
package com.github.ustc_zzzz.fmltutor.item;

import com.github.ustc_zzzz.fmltutor.creativetab.CreativeTabsLoader;

import net.minecraft.item.Item;

public class ItemGoldenEgg extends Item
{
    public ItemGoldenEgg()
    {
        super();
        this.setUnlocalizedName("goldenEgg");
        this.setCreativeTab(CreativeTabsLoader.tabFMLTutor);
    }
}
```

最后在`CommonProxy`中的`preInit`阶段添加代码，记得创造模式物品栏的初始化一定要在物品和方块的初始化之前：

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/CommonProxy.java（部分）:`**

```java
    public void preInit(FMLPreInitializationEvent event)
    {
        new CreativeTabsLoader(event);
        new ItemLoader(event);
        new BlockLoader(event);
    }
```

打开游戏，你是不是看到了物品被注册到了新的创造模式物品栏，方块被注册到了“方块”创造模式物品栏呢～

### 个性化创造模式物品栏

`CreativeTab`类提供了`hasSearchBar`方法，可以用于设置是否有搜索框，这里我们通过覆写这个方法设置为`true`：

**`src/main/java/com/github/ustc_zzzz/fmltutor/creativetab/CreativeTabsFMLTutor.java（部分）:`**

```java
    @Override
    public boolean hasSearchBar()
    {
        return true;
    }
```

我们就可以在打开的创造模式物品栏上看到搜索框了。同时还有的是一个名为`getSearchbarWidth`的方法，用于设置搜索框的宽度。

现在我们再进一步，设置一下创造模式物品栏的背景。这里我们需要的是一个名为`setBackgroundImageName`的方法，这个方法传入的参数最终会和一个非常长的路径接合，这里我们先设置一下：

```java
    public CreativeTabsFMLTutor()
    {
        super("fmltutor");
        this.setBackgroundImageName("fmltutor.png");
    }
```

然后我们需要新建文件夹`assets/minecraft/textures/gui/container/creative_inventory`（**注意这里的名称是`minecraft`不是Mod id**），然后建立一个以`tab_`开头的PNG文件，名字由刚刚设置的字符串决定，这里就是`tab_fmltutor.png`，注意，这里的图片尺寸大小需要是256x256，其他的尺寸大小会出问题，需要设置的背景放置在左上角，其他的位置设置成透明就可以了：

**`src/main/resources/assets/minecraft/textures/gui/container/creative_inventory/tab_fmltutor.png`**

![tab_fmltutor](tab_fmltutor.png)

现在我们就可以看到由于作者的艺术细胞极度缺乏而导致的金光闪闪熠熠生辉的创造模式物品栏了：

![tab_fmltutor_analysis](./tutorial-ustc-zzzz/tab_fmltutor_analysis.png)

## 1.6 第一份合成表、烧炼规则和燃料

### 第一份合成表

新建包`com.github.ustc_zzzz.fmltutor.crafting`，并新建文件`CraftingLoader.java`：

**`src/main/java/com/github/ustc_zzzz/fmltutor/crafting/CraftingLoader.java:`**

```java
package com.github.ustc_zzzz.fmltutor.crafting;

import net.minecraftforge.fml.common.registry.GameRegistry;

public class CraftingLoader
{
    public CraftingLoader()
    {
        registerRecipe();
        registerSmelting();
        registerFuel();
    }

    private static void registerRecipe()
    {

    }

    private static void registerSmelting()
    {

    }

    private static void registerFuel()
    {

    }
}
```

向`registerRecipe`方法添加内容：

**`src/main/java/com/github/ustc_zzzz/fmltutor/crafting/CraftingLoader.java（部分）:`**

```java
    private static void registerRecipe()
    {
        GameRegistry.addShapedRecipe(new ItemStack(ItemLoader.goldenEgg), new Object[]
        {
                "###", "#*#", "###", '#', Items.gold_ingot, '*', Items.egg
        });
        GameRegistry.addShapedRecipe(new ItemStack(BlockLoader.grassBlock), new Object[]
        {
                "##", "##", '#', Blocks.vine
        });
        GameRegistry.addShapelessRecipe(new ItemStack(Blocks.vine, 4), BlockLoader.grassBlock);
    }
```

前两句通过调用`addShapedRecipe`方法添加了有序合成表（如合成木棍等等）。

后一句通过调用`addShapelessRecipe`方法添加了无序合成表（如合成书等等）。

Minecraft原版所有的方块和物品都被存放在`Blocks`类和`Items`类里。

读者想必到这里已经明白，这三份合成表是什么意思了，不过需要注意的可能是表示字符的单引号和表示字符串的双引号。

### 第一个烧炼规则

向`registerSmelting`方法添加内容：

**`src/main/java/com/github/ustc_zzzz/fmltutor/crafting/CraftingLoader.java（部分）:`**

```java
    private static void registerSmelting()
    {
        GameRegistry.addSmelting(BlockLoader.grassBlock, new ItemStack(Items.coal), 0.5F);
    }
```

第一个参数是待烧炼的物品，第二个参数是烧炼后的物品，第三个参数是烧炼后玩家可以得到的经验。

### 第一个燃料

向`registerFuel`方法添加内容：

**`src/main/java/com/github/ustc_zzzz/fmltutor/crafting/CraftingLoader.java（部分）:`**

```java
    private static void registerFuel()
    {
        GameRegistry.registerFuelHandler(new IFuelHandler()
        {
            @Override
            public int getBurnTime(ItemStack fuel)
            {
                return Items.diamond != fuel.getItem() ? 0 : 12800;
            }
        });
    }
```

注册燃料需要实现`IFuelHandler`接口，这里使用了匿名类以节省代码量。

实现`IFuelHandler`接口需要实现`getBurnTime`方法，该方法判断物品的烧炼时间，如果返回0，则为不能判断物品的烧炼时间。

这里的烧炼时间为gametick，**一秒为20个gametick**，下面列出一些常见的烧炼时间数据：

* 树苗　　100
* 木板　　200
* 煤炭　　1600
* 烈焰棒　2400
* 煤炭块　16000
* 岩浆桶　20000

这段代码添加了钻石作为燃料，想必读者也很容易看出这段代码的含义。

最后，在CommonProxy中的`init`阶段注册：

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/CommonProxy.java（部分）:`**

```java
    public void init(FMLInitializationEvent event)
    {
        new CraftingLoader();
    }
```

打开游戏，进行测试吧～

## 1.7 本地化和国际化

### 什么是本地化和国际化

下面内容来自来自[wikipedia](https://zh.wikipedia.org/wiki/%E5%9B%BD%E9%99%85%E5%8C%96%E4%B8%8E%E6%9C%AC%E5%9C%B0%E5%8C%96)：

>本地化则是指当移植软件时，加上与特定区域设置有关的信息和翻译文件的过程。国际化是指在设计软件，将软件与特定语言及地区脱钩的过程。当软件被移植到不同的语言及地区时，软件本身不用做内部工程上的改变或修正。

本地化的国际化的英文分别是Localization和internationalization，由于字母过多，它们被简化成了L10n和i18n，其中i18n的简写由于其中间有足足十八个字母等原因而更加常用。

Minecraft本身就提供了本地化和国际化方案。在`assets/minecraft/lang`文件夹下，便有着各种语言的语言文件。

### 为自己的Mod创建语言文件

大家在前面几部分中可能注意到了，拿在手里的不是金蛋，而是`item.goldenEgg.name`，放在地上的不是草块，而是`tile.grassBlock.name`，这正是因为没有添加语言文件。

英语是全球最广泛使用的语言，Minecraft的默认使用语言也是英语，所以很明显地，我们应该首先对英语提供支持。

新建文件夹`src/main/resources/assets/fmltutor/lang`，并在其中新建文件`en_US.lang`，注意等号的两边没有空格：

**`src/main/resources/assets/fmltutor/lang/en_US.lang:`**

```
item.goldenEgg.name=Golden Egg

tile.grassBlock.name=Grass Block

itemGroup.fmltutor=FML Tutor
```

* `item.goldenEgg.name`便是金蛋的名称，这个名称由该物品的`setUnlocalizedName`方法设置
* `tile.grassBlock.name`便是草块的名称，这个名称由该方块的`setUnlocalizedName`方法设置
* `itemGroup.tabFMLTutor`便是新创造模式物品栏的名称

如果搞不清楚语言文件等号前面应该使用什么，可以先不写，在游戏中看一看，然后把不正常的部分写入语言文件。

当然，作为面向中国人制作的Mod，中文语言文件还是要有的。在同一个文件夹下新建文件`zh_CN.lang`：

**`src/main/resources/assets/fmltutor/lang/zh_CN.lang:`**

```
item.goldenEgg.name=金蛋

tile.grassBlock.name=草块

itemGroup.fmltutor=FML教程
```

当然，你也可以添加其他语言文件，如繁体中文（zh_TW）等，语言文件的文件名通常按照“**[语言](https://zh.wikipedia.org/wiki/ISO_639-1)\_[国家](https://zh.wikipedia.org/wiki/ISO_3166-1)**”代码标准。

最后说一句，语言文件**一定要使用UTF-8编码**。

## 1.8 创建一份配置文件

### 为什么需要一份配置文件

很明显，一个Mod做得再好，也会有玩家不满意。这时候的一种常见的解决方案就是，开放Mod的部分参数选项，使其从配置文件中读取，这样玩家就可以通过修改Mod的配置文件来对Mod做出一些个性化设置了。

一个好的配置文件，不仅应该全面，更应该条理分明、层次紧密、简明易懂，让玩家不需要帮助甚至注释就可以理解并顺利地修改配置文件内容，而不会出现困惑的现象。

幸运的是，Forge本身就提供了配置文件的接口，Mod开发者们可以轻而易举地完成配置文件的创建、读取、以及写入等操作。

本部分通过对钻石作为燃料的烧炼秒数的配置，一步一步地完成配置文件的相关操作。

### 实际操作

首先，我们创建一个配置文件管理类，在包`com.github.ustc_zzzz.fmltutor.common`下创建文件`ConfigLoader.java`：

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/ConfigLoader.java:`**

```java
package com.github.ustc_zzzz.fmltutor.common;

import net.minecraftforge.common.config.Configuration;
import net.minecraftforge.fml.common.event.FMLPreInitializationEvent;
import org.apache.logging.log4j.Logger;

public class ConfigLoader
{
    private static Configuration config;

    private static Logger logger;

    public static int diamondBurnTime;

    public ConfigLoader(FMLPreInitializationEvent event)
    {
        logger = event.getModLog();
        config = new Configuration(event.getSuggestedConfigurationFile());

        config.load();
        load();
    }

    public static void load()
    {
        logger.info("Started loading config. ");
        String comment;
        
        comment = "How many seconds can a diamond burn in a furnace. ";
        diamondBurnTime = config.get(Configuration.CATEGORY_GENERAL, "diamondBurnTime", 640, comment).getInt();

        config.save();
        logger.info("Finished loading config. ");
    }

    public static Logger logger()
    {
        return logger;
    }
}
```

首先，我们实例化了一个`Configuration`类：

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/ConfigLoader.java（部分）:`**

```java
        config = new Configuration(event.getSuggestedConfigurationFile());
```

这里的`event.getSuggestedConfigurationFile()`，就是Forge推荐的配置文件位置。这个位置在游戏根目录的config文件夹下，名为“`<Mod id>.cfg`”，这里就是“`fmltutor.cfg`”。

然后我们读入配置：

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/ConfigLoader.java（部分）:`**

```java
        config.load();
```

接下来是加载配置：

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/ConfigLoader.java（部分）:`**

```java
        comment = "How many seconds can a diamond burn in a furnace. ";
        diamondBurnTime = config.get(Configuration.CATEGORY_GENERAL, "diamondBurnTime", 640, comment).getInt();
```

在一个正常的Forge Mod配置文件里，会有多个类别，Forge提供了一种类别“`general`”（`Configuration.CATEGORY_GENERAL`），`get`方法的第一个参数就是表示“`general`”类别。

`get`方法的第三个参数，是该键的默认值（这里是640），当对应的键不存在时，就会返回该默认值。

`get`方法的第四个参数，是该键的注释，用于描述该项配置的。

那么很明显，`get`方法的作用，就是获取`diamondBurnTime`键对应的值。

`getInt`方法的作用，就是取整数（因为配置文件里的值一定是字符串）。

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/ConfigLoader.java（部分）:`**

```java
        config.save();
```

最后我们保存配置。

这里要说一下，为什么要保存配置呢？这是因为当配置缺失（最常见的原因就是配置文件没有创建，这常常发生在你第一次使用Mod的时候）的时候，这一句会将默认的配置保存下来。

这里我们还顺路设置了一个`logger`，并在加载配置的时候使用到了它作日志记录。

在`CommonProxy`中注册：

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/CommonProxy.java（部分）:`**

```java
    public void preInit(FMLPreInitializationEvent event)
    {
        new ConfigLoader(event);
        new CreativeTabsLoader(event);
        new ItemLoader(event);
        new BlockLoader(event);
    }
```

最后修改一下`CraftingLoader`文件：

**`src/main/java/com/github/ustc_zzzz/fmltutor/crafting/CraftingLoader.java（部分）:`**

```java
private static void registerFuel()
{
    GameRegistry.registerFuelHandler(new IFuelHandler()
    {
        @Override
        public int getBurnTime(ItemStack fuel)
        {
            return Items.diamond != fuel.getItem() ? 0 : Math.max(0, ConfigLoader.diamondBurnTime) * 20;
        }
    });
}
```

运行游戏，我们会发现，在`config`文件夹下，生成了一个名为`fmltutor.cfg`的文件。下面是在作者的电脑下生成的这个文件的内容：

```cfg
# Configuration file

general {
    # How many seconds can a diamond burn in a furnace. 
    I:diamondBurnTime=640
}
```

# 2 初级部分

## 2.1.1 注册已有的事件

### 绪论

Forge的事件系统一直在Forge中占有十分重要的地位，可以这么说，没有事件，就没有Mod。大家可以注意到，主类的`preInit`，`init`，`postInit`方法，全部都是事件驱动的。换句话说，理论上一个Mod的开发教程本身应该从事件讲起。

Forge的事件系统几乎涵盖了方方面面，从服务端到客户端，从世界生成到物品方块行为，从玩家行为到一般实体行为，等等。

Forge的事件系统分为两类，一类是FML生命周期事件，一类是Minecraft事件。

### FML生命周期事件

FML生命周期事件，顾名思义，就是FML加载、关闭、和Mod加载等等相关的事件，这些希望监听对应事件的方法使用`@EventHandler`注解修饰，并且应在被`@Mod`注解修饰的主类下，Forge会寻找并注册仅含一个参数并且参数符合特定类型的方法。如下面三个FML生命周期事件是最常用的：

* `FMLPreInitializationEvent`
* `FMLInitializationEvent`
* `FMLPostInitializationEvent`

这三个事件的使用方法已经讲过，此处不再赘述。

还有下面两个事件：

* `FMLConstructionEvent`在Mod开始加载时触发。
* `FMLLoadCompleteEvent`在Mod加载完成时触发。

除上面这些之外，还有下面的这些比较常用的用于服务端的FML生命周期事件：

* `FMLServerAboutToStartEvent`
* `FMLServerStartingEvent`
* `FMLServerStartedEvent`
* `FMLServerStoppingEvent`
* `FMLServerStoppedEvent`

想必读者已经可以猜出来这五个事件的异同，并了解这些事件被触发的条件了。

### Minecraft事件

Forge本身提供了很多Minecraft事件，这些事件基本上可以完成对Minecraft大部分物品、方块、实体等特性的修改，并且这些事件的数量还在不断地上升。开发者只需要注册一个包含监听这些事件的方法的类，Forge就会挂钩上这些方法。这些方法使用`@SubscribeEvent`注解进行修饰，Forge寻找并挂钩这些方法的方式和上面的FML生命周期事件类似，只不过由于挂钩的方式不同，调用的时候效率要更高。

首先我们创造一个类。在包`com.github.ustc_zzzz.fmltutor.common`下新建一个文件`EventLoader.java`：

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/EventLoader.java:`**

```java
    package com.github.ustc_zzzz.fmltutor.common;
    
    import net.minecraftforge.common.MinecraftForge;
    import net.minecraftforge.event.entity.player.PlayerInteractEvent;
    import net.minecraftforge.fml.common.eventhandler.SubscribeEvent;
    import net.minecraftforge.fml.common.gameevent.PlayerEvent;
    
    public class EventLoader
    {
        public EventLoader()
        {
            MinecraftForge.EVENT_BUS.register(this);
        }
    
        @SubscribeEvent
        public void onPlayerItemPickup(PlayerEvent.ItemPickupEvent event)
        {
            if (event.player.isServerWorld())
            {
                String info = String.format("%s picks up: %s", event.player.getName(), event.pickedUp.getEntityItem());
                ConfigLoader.logger().info(info);
            }
        }
    
        @SubscribeEvent
        public void onPlayerInteract(PlayerInteractEvent event)
        {
            if (!event.world.isRemote)
            {
                String info = String.format("%s interacts with: %s", event.entityPlayer.getName(), event.pos);
                ConfigLoader.logger().info(info);
            }
        }
    }
```

这里作者选取了两个事件进行举例，我们一步一步分析上面代码的含义：

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/EventLoader.java（部分）:`**

```java
        @SubscribeEvent
        public void onPlayerItemPickup(PlayerEvent.ItemPickupEvent event)
        {
            if (event.player.isServerWorld())
            {
                String info = String.format("%s picks up: %s", event.player.getName(), event.pickedUp.getEntityItem());
                ConfigLoader.logger().info(info);
            }
        }
    
        @SubscribeEvent
        public void onPlayerInteract(PlayerInteractEvent event)
        {
            if (!event.world.isRemote)
            {
                String info = String.format("%s interacts with: %s", event.entityPlayer.getName(), event.pos);
                ConfigLoader.logger().info(info);
            }
        }
```

`@SubscribeEvent`注解的作用是Forge在你注册这个类的时候，会扫描所有具有该注解的方法，然后挂钩。 Forge会根据方法的参数类型来区分不同的事件。比如，这里的`onPlayerItemPickup`方法挂钩的就是物品即将被捡起的时候触发的事件`PlayerEvent.ItemPickupEvent`，而`onPlayerInteract`方法挂钩的就是玩家在和物品或方块互动的时候触发的事件`PlayerInteractEvent`。这里因为只是为了演示，我们这里只输出日志信息。

`@SubscribeEvent`注解有两个参数，其中一个是`receiveCanceled`，与是否取消该事件相关，默认为`false`，这个参数不太常用，我们不去管它。还有一个参数是`priority`，比较常用，表示事件的优先级，可能的情况有五种：

* `EventPriority.HIGHEST`
* `EventPriority.HIGH`
* `EventPriority.NORMAL`
* `EventPriority.LOW`
* `EventPriority.LOWEST`

默认的优先级是`EventPriority.NORMAL`，当然，如果想自定优先级，往往都会选择`EventPriority.HIGH`，和`EventPriority.HIGHEST`。如果没有特殊需求，这一项还是默认好了。

代码`event.player.isServerWorld()`用于检测调用该事件的游戏到底是客户端还是服务端，往往我们只希望服务端调用代码，这是因为服务端产生的变化，客户端往往都会同步，比如这里的向玩家输出游戏控制台信息。代码`!event.world.isRemote`也是同样的道理，在后面的内容中，这个用于判断服务端还是客户端的方法很常用。

最后就是事件的注册部分：

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/EventLoader.java（部分）:`**

```java
        public EventLoader()
        {
            MinecraftForge.EVENT_BUS.register(this);
        }
```

我们使用`EventBus`的`register`方法，注册了所有我们想要注册的事件。

除此之外，Forge还提供了需要在`MinecraftForge.TERRAIN_GEN_BUS`上注册的地形生成事件，需要在`MinecraftForge.ORE_GEN_BUS`上注册的矿物生成事件等等。

最后在`CommonProxy`中注册：

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/CommonProxy.java（部分）:`**

```java
        public void init(FMLInitializationEvent event)
        {
            new CraftingLoader();
            new EventLoader();
        }
```

打开游戏试试吧～

### Event类解析

Forge提供的所有事件，都是`net.minecraftforge.fml.common.eventhandler.Event`类的子类。

Event类添加了下面几个公开方法：

* `public boolean isCancelable()` <br> 返回该事件是否可以被取消。
* `public boolean isCanceled()` <br> 返回该事件是否已被取消。
* `public void setCanceled(boolean cancel)` <br> 设置该事件是否被取消。
* `public boolean hasResult()` <br> 返回该事件是否有结果，添加了`@HasResult`注解的事件默认为`true`，否则为`false`。
* `public Result getResult()` <br> 返回该事件的结果，有`Result.DENY`，`Result.DEFAULT`，`Result.ALLOW`三种，默认为`Result.DEFAULT`。
* `public void setResult(Result value)` <br> 为该事件设置一个结果。
* `public ListenerList getListenerList()` <br> 获取所有注册该事件的监听器。
* `public EventPriority getPhase()` <br> 获取该事件的优先级，上面已有说明。
* `public void setPhase(EventPriority value)` <br> 设置该事件的优先级，上面已有说明。

## 2.1.2 自定义新的事件

### 自定义新的事件类

我们在`EventLoader`类中新建一个我们想要的事件类。

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/EventLoader.java（部分）:`**

```java
        @Cancelable
        public static class PlayerRightClickGrassBlockEvent extends net.minecraftforge.event.entity.player.PlayerEvent
        {
            public final BlockPos pos;
            public final World world;
    
            public PlayerRightClickGrassBlockEvent(EntityPlayer player, BlockPos pos, World world)
            {
                super(player);
                this.pos = pos;
                this.world = world;
            }
        }
```

很明显，这个类和玩家右键草块相关。该事件类继承了`PlayerEvent`，`@Cancelable`注解表明了该事件可取消。

### 自定义事件的注册机制

在上一部分，我们注意到`FMLCommonHandler.instance().bus()`和`MinecraftForge.EVENT_BUS`均为`EventBus`类型。该类型提供了名为`register`的方法使得事件可以被注册。

显然，我们自己也可以创建这样一个`EventBus`，并且使得所有自定义的事件在这里被注册。

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/EventLoader.java（部分）:`**

```java
        public static final EventBus EVENT_BUS = new EventBus();
```

### 使自定义事件会被触发到

`EventBus`本身提供了一个名为`post`的方法，负责执行事件。大家如果经常翻源代码的话，会在Minecraft的许多类中找到这个方法的调用。这里我们希望在玩家点击草块时触发，我们也如法炮制。

**`src/main/java/com/github/ustc_zzzz/fmltutor/block/BlockGrassBlock.java（部分）:`**

```java
        @Override
        public boolean onBlockActivated(World worldIn, BlockPos pos, IBlockState state, EntityPlayer playerIn,
                EnumFacing side, float hitX, float hitY, float hitZ)
        {
            EventLoader.PlayerRightClickGrassBlockEvent event;
            event = new EventLoader.PlayerRightClickGrassBlockEvent(playerIn, pos, worldIn);
            EventLoader.EVENT_BUS.post(event);
            if (!event.isCanceled() && !worldIn.isRemote)
            {
                worldIn.setBlockToAir(pos);
                return true;
            }
            return false;
        }
```

很明显，这段代码的意思是，如果事件被取消了，就阻止草块变成空气，否则草块就会变成空气。

### 自定义事件的实现

自定义事件的实现和Forge提供的完全一样，只不过我们要找准在哪里注册。

在`EventLoader`类中添加一个方法。

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/EventLoader.java（部分）:`**

```java
        @SubscribeEvent
        public void onPlayerClickGrassBlock(PlayerRightClickGrassBlockEvent event)
        {
            if (!event.world.isRemote)
            {
                BlockPos pos = event.pos;
                Entity tnt = new EntityTNTPrimed(event.world, pos.getX() + 0.5, pos.getY() + 0.5, pos.getZ() + 0.5, null);
                event.world.spawnEntityInWorld(tnt);
            }
        }
```

很明显，这里定义了当玩家两手空空时的行为。

然后我们注册这个事件。

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/EventLoader.java（部分）:`**

```java
        public EventLoader()
        {
            MinecraftForge.EVENT_BUS.register(this);
            EventLoader.EVENT_BUS.register(this);
        }
```

打开游戏，放置一个草块，右键点击它，三、二、一。。。BOOM

## 2.2.1 新的工具

### 概述

本部分以制作一个红石镐为例，讲述如何做出一个新的工具或武器，如斧、镐、铲、以及锄和剑。

### 物品和ToolMaterial

在包`com.github.ustc_zzzz.fmltutor.item`下新建一个文件`ItemRedstonePickaxe.java`，并让`ItemRedstonePickaxe`类继承`ItemPickaxe`类：

**`src/main/java/com/github/ustc_zzzz/fmltutor/item/ItemRedstonePickaxe.java:`**

```java
    package com.github.ustc_zzzz.fmltutor.item;
    
    import com.github.ustc_zzzz.fmltutor.creativetab.CreativeTabsLoader;
    
    import net.minecraft.item.Item;
    import net.minecraft.item.ItemPickaxe;
    import net.minecraftforge.common.util.EnumHelper;
    
    public class ItemRedstonePickaxe extends ItemPickaxe
    {
        public static final Item.ToolMaterial REDSTONE = EnumHelper.addToolMaterial("REDSTONE", 3, 16, 16.0F, 0.0F, 10);
    
        public ItemRedstonePickaxe()
        {
            super(REDSTONE);
            this.setUnlocalizedName("redstonePickaxe");
            this.setCreativeTab(CreativeTabsLoader.tabFMLTutor);
        }
    }
```

我们先来说说这一行：

**`src/main/java/com/github/ustc_zzzz/fmltutor/item/ItemRedstonePickaxe.java（部分）:`**

```java
        public static final Item.ToolMaterial REDSTONE = EnumHelper.addToolMaterial("REDSTONE", 3, 16, 16.0F, 0.0F, 10);
```

这里添加了一个作为工具需要的枚举类，ToolMaterial的实例。顾名思义，ToolMaterial就是工具或武器使用的材料，Forge本身定义了五种材料：WOOD，STONE，IRON，EMERALD，GOLD。它们分别表示木头、石头、铁、钻石、金。

我们来看看ToolMaterial的构造方法：

```java
    private ToolMaterial(int harvestLevel, int maxUses, float efficiency, float damageVsEntity, int enchantability) {...}
```

和五种材料的参数：

* `WOOD(0, 59, 2.0F, 0.0F, 15),`
* `STONE(1, 131, 4.0F, 1.0F, 5),`
* `IRON(2, 250, 6.0F, 2.0F, 14),`
* `EMERALD(3, 1561, 8.0F, 3.0F, 10),`
* `GOLD(0, 32, 12.0F, 0.0F, 22);`

ToolMaterial的构造方法共有五个参数：

* `harvestLevel`参数表示制作出的工具等级。这一点在镐中尤其明显，如木头为0，只能挖掘对应等级为0的方块才能掉落物品，如石头等，而钻石为3，就可以挖掘出对应等级为3的，其他镐挖不出物品的方块，如黑曜石。这里使用了最高等级3
* `maxUses`参数表示制作出的工具对应耐久。如钻石工具就是1561，耐久最高，而木工具为59，耐久最低。这里刻意降低了该数值，为16
* `efficiency`参数表示制作出的工具使用效率。使用效率和该参数的值成正比。这里刻意提高了该数值，为16.0F
* `damageVsEntity`参数表示攻击伤害力度。同样该力度和该参数的值成正相关。这里为0.0F，表示攻击力很低
* `enchantability`参数与附魔等级相关。Minecraft中关于附魔等级的系统十分复杂。但是有一点，就是该值越高，对应的附魔就越容易得到高等级。这也是为何金更容易得到高等级附魔，而石头得到的附魔就相当低。这里为10，和钻石相同

`EnumHelper`的作用就是为Minecraft的一些枚举类型注册新的实例，该方法的第一个参数为实例的名称，后面的参数就是该枚举类型构造方法需要的参数。比如这里就是向ToolMaterial枚举类型添加一个名为`REDSTONE`的实例，并提供相应的参数。

### 物品模型和材质

该物品的模型：

**`src/main/resources/assets/fmltutor/models/item/redstone_pickaxe.json:`**

```json
 {
     "parent": "builtin/generated",
     "textures": {
         "layer0": "fmltutor:items/redstone_pickaxe"
     },
     "display": {
         "thirdperson": {
             "rotation": [ 0, 90, -35 ],
             "translation": [ 0, 1.25, -3.5 ],
             "scale": [ 0.85, 0.85, 0.85 ]
         },
         "firstperson": {
             "rotation": [ 0, -135, 25 ],
             "translation": [ 0, 4, 2 ],
             "scale": [ 1.7, 1.7, 1.7 ]
         }
     }
 }
```

和对应的材质（然而大家可以明显地看出这只是钻石镐的调色=_=||）：

**`src/main/resources/assets/fmltutor/textures/items/redstone_pickaxe.png:`**

![redstone_pickaxe](resources/redstone_pickaxe.png)

### 一些例行公事

语言文件和在`GameRegistry`中注册（这里需要稍微注意一下的可能是`ToolMaterial`和物品的先后注册顺序）：

**`src/main/resources/assets/fmltutor/lang/en_US.lang（部分）:`**

```lang
    item.redstonePickaxe.name=Redstone Pickaxe
```

**`src/main/resources/assets/fmltutor/lang/zh_CN.lang（部分）:`**

```lang
    item.redstonePickaxe.name=红石镐
```

**`src/main/java/com/github/ustc_zzzz/fmltutor/item/ItemLoader.java（部分）:`**

```java
        public static Item goldenEgg = new ItemGoldenEgg();
        public static ItemPickaxe redstonePickaxe = new ItemRedstonePickaxe();
    
        public ItemLoader(FMLPreInitializationEvent event)
        {
            register(goldenEgg, "golden_egg");
            register(redstonePickaxe, "redstone_pickaxe");
        }
    
        @SideOnly(Side.CLIENT)
        public static void registerRenders()
        {
            registerRender(goldenEgg);
            registerRender(redstonePickaxe);
        }
```

当然最后，我们也可以加上合成表：

**`src/main/java/com/github/ustc_zzzz/fmltutor/crafting/CraftingLoader.java（部分）:`**

```java
            GameRegistry.addShapedRecipe(new ItemStack(ItemLoader.redstonePickaxe), new Object[]
            {
                    "###", " * ", " * ", '#', Items.redstone, '*', Items.stick
            });
```

打开游戏试试吧～

当然，其他需要ToolMaterial的工具，如斧、铲、锄与剑，它们的制作，都是同样的道理。

## 2.2.2 新的食物

## 2.2.3 新的盔甲

## 2.3.1 新的伤害类型

## 2.3.2 新的附魔属性

## 2.3.3 新的药水效果

## 2.4.1 热键绑定

## 2.4.2 成就系统

## 2.4.3 系统命令

## 2.4.4 声音系统

## 2.5.1 在世界生成矿物

## 2.5.2 注册和使用矿物字典

## 2.6.1 创建并注册一份流体

## 2.6.2 为流体添加对应的桶
