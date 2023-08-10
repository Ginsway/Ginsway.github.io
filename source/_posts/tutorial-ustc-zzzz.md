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

### 概述

本部分以制作一个红石苹果为例，讲述如何做出一个新的食物，并设置其食用后的效果。

### 制作一个崭新的食物

在包`com.github.ustc_zzzz.fmltutor.item`下新建一个文件`ItemRedstoneApple.java`，并让`ItemRedstoneApple`类继承`ItemFood`类：

**`src/main/java/com/github/ustc_zzzz/fmltutor/item/ItemRedstoneApple.java:`**

```java
    package com.github.ustc_zzzz.fmltutor.item;
    
    import net.minecraft.item.ItemFood;
    
    import com.github.ustc_zzzz.fmltutor.creativetab.CreativeTabsLoader;
    
    public class ItemRedstoneApple extends ItemFood
    {
        public ItemRedstoneApple()
        {
            super(4, 0.6F, false);
            this.setAlwaysEdible();
            this.setUnlocalizedName("redstoneApple");
            this.setCreativeTab(CreativeTabsLoader.tabFMLTutor);
        }
    }
```

`ItemFood`类的构造方法一共有三个参数：

* 第一个参数`amount`表示该食物所能回复的饥饿值，这里被设定成和苹果相同，即`4`。
* 第二个参数`saturation`表示该食物所能添加的相对饱和度，其正比于饱和度和饥饿值的比值，这里设定为`0.6F`。
* 最后一个参数`isWolfFood`表示该食物能否被狼食用，这里简单地设置为`false`就可以了。

饱和度的计算：`2 * amount * saturation`。如面包的`amount`为5，其`saturation`为0.6F，对应的饱和度为2 *5* 0.6 = 6

为了方便读者，我们在这里列了一个常见食物对应的`amount`和`saturation`表。

| 食物 | amount | saturation |
|:----|:------|:------|
| 苹果 | 4 | 0.3F |
| 面包 | 5 | 0.6F |
| 生猪排 | 3 | 0.3F |
| 熟猪排 | 8 | 0.8F |
| 曲奇 | 2 | 0.1F |
| 西瓜片 | 2 | 0.3F |
| 生牛肉 | 3 | 0.3F |
| 牛排 | 8 | 0.8F |
| 生鸡肉 | 2 | 0.3F |
| 熟鸡肉 | 6 | 0.6F |
| 腐肉 | 4 | 0.1F |
| 蜘蛛眼 | 2 | 0.8F |
| 烤马铃薯 | 5 | 0.6F |
| 毒马铃薯 | 2 | 0.3F |
| 金萝卜 | 6 | 1.2F |
| 南瓜派 | 8 | 0.3F |

方法`setAlwaysEdible`表示该食物何时何地都可以被食用，即便玩家不需要回复饥饿度和饱和值。

下面就是一些例行公事了（模型、贴图、语言文件、以及注册）（贴图同为金苹果调色=_=||）：

**`src/main/resources/assets/fmltutor/models/item/redstone_apple.json:`**

```json
 {
     "parent": "builtin/generated",
     "textures": {
         "layer0": "fmltutor:items/redstone_apple"
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

**`src/main/resources/assets/fmltutor/textures/items/redstone_apple.png:`**

![redstone_apple](resources/redstone_apple.png)

**`src/main/resources/assets/fmltutor/lang/en_US.lang（部分）:`**

```lang
    item.redstoneApple.name=Redstone Apple
```

**`src/main/resources/assets/fmltutor/lang/zh_CN.lang（部分）:`**

```lang
    item.redstoneApple.name=红石苹果
```

**`src/main/java/com/github/ustc_zzzz/fmltutor/item/ItemLoader.java（部分）:`**

```java
        public static Item goldenEgg = new ItemGoldenEgg();
        public static ItemPickaxe redstonePickaxe = new ItemRedstonePickaxe();
        public static ItemFood redstoneApple = new ItemRedstoneApple();
    
        public ItemLoader(FMLPreInitializationEvent event)
        {
            register(goldenEgg, "golden_egg");
            register(redstonePickaxe, "redstone_pickaxe");
            register(redstoneApple, "redstone_apple");
        }
    
        @SideOnly(Side.CLIENT)
        public static void registerRenders()
        {
            registerRender(goldenEgg);
            registerRender(redstonePickaxe);
            registerRender(redstoneApple);
        }
```

当然我们也可以加上合成表：

**`src/main/java/com/github/ustc_zzzz/fmltutor/crafting/CraftingLoader.java（部分）:`**

```java
            GameRegistry.addShapedRecipe(new ItemStack(ItemLoader.redstoneApple), new Object[]
            {
                    "###", "#*#", "###", '#', Items.redstone, '*', Items.apple
            });
```

打开游戏试试吧～

### 为食物添加食用后的药水效果

实际上，`ItemFood`类本身就预置了药水效果的轮子，我们在构造函数中加上这么一句：

**`src/main/java/com/github/ustc_zzzz/fmltutor/item/ItemRedstoneApple.java（部分）:`**

```java
            this.setPotionEffect(Potion.absorption.id, 10, 1, 1.0F);
```

`setPotionEffect`方法共有四个参数：

* 第一个参数表示对应药水效果的`potionId`，读者可以去`net.minecraft.potion.Potion`类中查看MC提供的二十四种药水效果，这里为伤害吸收。
* 第二个参数表示对应药水效果的持续时间，以秒计数，这里为十秒。
* 第三个参数表示对应药水效果的等级，很明显，0为一级，1为二级，2为三级，以此类推，这里为二级。
* 最后一个参数表示产生该药水效果的概率，这里为100%。

到这里我们就完成了对于添加食用食物后的药水效果的设置，这对大部分的食物设定来说，是够用了的。事实上，MC游戏本身的大部分食物，它们食用后的药水效果（如食用腐肉后产生的饥饿效果，食用蜘蛛眼后产生的中毒效果）都是这么设定的。

### 为食物添加食用后的更多效果

当然，总有例外，例如食用河豚或金苹果后产生的多种药水效果，就不能通过上面的方法完成。

`ItemFood`类提供了一个方法`onFoodEaten`，我们可以把它覆写掉：

**`src/main/java/com/github/ustc_zzzz/fmltutor/item/ItemRedstoneApple.java（部分）:`**

```java
        @Override
        public void onFoodEaten(ItemStack stack, World worldIn, EntityPlayer player)
        {
            if (!worldIn.isRemote)
            {
                player.addPotionEffect(new PotionEffect(Potion.saturation.id, 200, 1));
                player.addExperience(10);
            }
            super.onFoodEaten(stack, worldIn, player);
        }
```

这段代码的意思可能已经比较明显了：除了伤害吸收二，食用该食物还会给玩家带来十秒的饱和二效果，和十点经验。这里有一点不同的地方，就是`PotionEffect`的构造函数使用的时间是以gametick计数的。

打开游戏试试吧～

## 2.2.3 新的盔甲

### 概述

本部分以制作全套红石盔甲为例，讲述如何做出一个新的盔甲，并讲述如何制作盔甲的材质。

### ArmorMaterial

和ToolMaterial类似，ArmorMaterial表示的就是盔甲的材质。

我们在包`com.github.ustc_zzzz.fmltutor.item`下新建一个文件`ItemRedstoneArmor.java`，并让`ItemRedstoneArmor`类继承`ItemArmor`类：

**`src/main/java/com/github/ustc_zzzz/fmltutor/item/ItemRedstoneArmor.java:`**

```java
    package com.github.ustc_zzzz.fmltutor.item;
    
    import com.github.ustc_zzzz.fmltutor.FMLTutor;
    
    import net.minecraft.item.ItemArmor;
    import net.minecraftforge.common.util.EnumHelper;
    
    public class ItemRedstoneArmor extends ItemArmor
    {
        public static final ItemArmor.ArmorMaterial REDSTONE_ARMOR = EnumHelper.addArmorMaterial("REDSTONE",
                FMLTutor.MODID + ":" + "redstone", 10, new int[]
                { 2, 6, 4, 2 }, 10);
    
        public ItemRedstoneArmor(int armorType)
        {
            super(REDSTONE_ARMOR, REDSTONE_ARMOR.ordinal(), armorType);
        }
    }
```

和ToolMaterial一样，我们看看ArmorMaterial的构造方法：

```java
    private ArmorMaterial(String name, int maxDamage, int[] reductionAmounts, int enchantability) {...}
```

和原版提供的五种材料的参数：

* `LEATHER("leather", 5, new int[]{1, 3, 2, 1}, 15),`
* `CHAIN("chainmail", 15, new int[]{2, 5, 4, 1}, 12),`
* `IRON("iron", 15, new int[]{2, 6, 5, 2}, 9),`
* `GOLD("gold", 7, new int[]{2, 5, 3, 1}, 25),`
* `DIAMOND("diamond", 33, new int[]{3, 8, 6, 3}, 10);`

ArmorMaterial的构造方法共有四个参数：

* `name`参数与该ArmorMaterial的材质所在位置有关，这一部分的稍后面会讲到。这里是“`fmltutor:redstone`”。
* `maxDamage`参数和该ArmorMaterial对应的盔甲的耐久成正比。这里刻意降低了大小，为10。
* `reductionAmounts`参数的四个元素表示对应盔甲的头盔、胸甲、护腿、和靴子抵御伤害的能力，如皮甲分别为1，3，2，1，和为7，钻石甲分别为3，8，6，3，和为20，**请不要让四个元素值的和超过这个值**。这里为2，6，4，2，和为14。
* `enchantability`参数和ToolMaterial一样，和对应盔甲的附魔能力正相关，同样，金盔甲的附魔能力最高。这里为10。

### 制作一套崭新的盔甲

在包`com.github.ustc_zzzz.fmltutor.item`下新建一个文件`ItemRedstoneArmor.java`，并让`ItemRedstoneArmor`类继承`ItemArmor`类：

**`src/main/java/com/github/ustc_zzzz/fmltutor/item/ItemRedstoneArmor.java（部分）:`**

```java
        public static class Helmet extends ItemRedstoneArmor
        {
            public Helmet()
            {
                super(0);
                this.setUnlocalizedName("redstoneHelmet");
                this.setCreativeTab(CreativeTabsLoader.tabFMLTutor);
            }
        }
    
        public static class Chestplate extends ItemRedstoneArmor
        {
            public Chestplate()
            {
                super(1);
                this.setUnlocalizedName("redstoneChestplate");
                this.setCreativeTab(CreativeTabsLoader.tabFMLTutor);
            }
        }
    
        public static class Leggings extends ItemRedstoneArmor
        {
            public Leggings()
            {
                super(2);
                this.setUnlocalizedName("redstoneLeggings");
                this.setCreativeTab(CreativeTabsLoader.tabFMLTutor);
            }
        }
    
        public static class Boots extends ItemRedstoneArmor
        {
            public Boots()
            {
                super(3);
                this.setUnlocalizedName("redstoneBoots");
                this.setCreativeTab(CreativeTabsLoader.tabFMLTutor);
            }
        }
```

`ItemArmor`的构造方法共有三个参数：

* 第一个参数表示该盔甲的ArmorMaterial，自然就是我们刚刚创建的那个。
* 第二个参数的名称为`renderIndex`，目前在源代码中没有找到对其的引用，作者个人认为其在某个版本中被弃用了，随便填一个就可以了。但是为了保证不同的ArmorMaterial对应不同的值，作者这里使用了该ArmorMaterial的序数值。
* 第三个参数表示该盔甲的类型，0为头盔，1为胸甲，2为护腿，3为靴子。

这里新建了四个子类，分别表示头盔、胸甲、护腿、和靴子。

### 一些例行公事

语言文件：

**`src/main/resources/assets/fmltutor/lang/en_US.lang（部分）:`**

```lang
    item.redstoneHelmet.name=Redstone Helmet
    item.redstoneChestplate.name=Redstone Chestplate
    item.redstoneLeggings.name=Redstone Leggings
    item.redstoneBoots.name=Redstone Boots
```

**`src/main/resources/assets/fmltutor/lang/zh_CN.lang（部分）:`**

```lang
 item.redstoneHelmet.name=红石头盔
 item.redstoneChestplate.name=红石胸甲
 item.redstoneLeggings.name=红石护腿
 item.redstoneBoots.name=红石靴子
```

模型及物品材质（大家没有猜错，物品材质仍然是调色）（读者：你TM就不能搞点原创么 (￣ε(#￣)☆╰╮o(￣皿￣///) 整天调色 (￣ε(#￣)☆╰╮o(￣皿￣///) ）：

**`src/main/resources/assets/fmltutor/models/item/redstone_helmet.json:`**

```json
 {
     "parent": "builtin/generated",
     "textures": {
         "layer0": "fmltutor:items/redstone_helmet"
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

**`src/main/resources/assets/fmltutor/models/item/redstone_chestplate.json:`**

```json
 {
     "parent": "builtin/generated",
     "textures": {
         "layer0": "fmltutor:items/redstone_chestplate"
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

**`src/main/resources/assets/fmltutor/models/item/redstone_leggings.json:`**

```json
 {
     "parent": "builtin/generated",
     "textures": {
         "layer0": "fmltutor:items/redstone_leggings"
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

**`src/main/resources/assets/fmltutor/models/item/redstone_boots.json:`**

```json
 {
     "parent": "builtin/generated",
     "textures": {
         "layer0": "fmltutor:items/redstone_boots"
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

**`src/main/resources/assets/fmltutor/textures/items/redstone_helmet.png:`**

![redstone_helmet](resources/redstone_helmet.png)

**`src/main/resources/assets/fmltutor/textures/items/redstone_chestplate.png:`**

![redstone_chestplate](resources/redstone_chestplate.png)

**`src/main/resources/assets/fmltutor/textures/items/redstone_leggings.png:`**

![redstone_leggings](resources/redstone_leggings.png)

**`src/main/resources/assets/fmltutor/textures/items/redstone_boots.png:`**

![redstone_boots](resources/redstone_boots.png)

注册：

**`src/main/java/com/github/ustc_zzzz/fmltutor/item/ItemLoader.java（部分）:`**

```java
        public static ItemArmor redstoneHelmet = new ItemRedstoneArmor.Helmet();
        public static ItemArmor redstoneChestplate = new ItemRedstoneArmor.Chestplate();
        public static ItemArmor redstoneLeggings = new ItemRedstoneArmor.Leggings();
        public static ItemArmor redstoneBoots = new ItemRedstoneArmor.Boots();
    
        public ItemLoader(FMLPreInitializationEvent event)
        {
            register(goldenEgg, "golden_egg");
            register(redstonePickaxe, "redstone_pickaxe");
            register(redstoneApple, "redstone_apple");
    
            register(redstoneHelmet, "redstone_helmet");
            register(redstoneChestplate, "redstone_chestplate");
            register(redstoneLeggings, "redstone_leggings");
            register(redstoneBoots, "redstone_boots");
        }
    
        @SideOnly(Side.CLIENT)
        public static void registerRenders()
        {
            registerRender(goldenEgg);
            registerRender(redstonePickaxe);
            registerRender(redstoneApple);
    
            registerRender(redstoneHelmet);
            registerRender(redstoneChestplate);
            registerRender(redstoneLeggings);
            registerRender(redstoneBoots);
        }
```

加点合成表：

**`src/main/java/com/github/ustc_zzzz/fmltutor/crafting/CraftingLoader.java（部分）:`**

```java
            GameRegistry.addShapedRecipe(new ItemStack(ItemLoader.redstoneHelmet), new Object[]
            {
                    "###", "# #", '#', Items.redstone
            });
            GameRegistry.addShapedRecipe(new ItemStack(ItemLoader.redstoneChestplate), new Object[]
            {
                    "# #", "###", "###", '#', Items.redstone
            });
            GameRegistry.addShapedRecipe(new ItemStack(ItemLoader.redstoneLeggings), new Object[]
            {
                    "###", "# #", "# #", '#', Items.redstone
            });
            GameRegistry.addShapedRecipe(new ItemStack(ItemLoader.redstoneBoots), new Object[]
            {
                    "# #", "# #", '#', Items.redstone
            });
```

现在打开游戏，应该就可以看到全套盔甲了。

### 盔甲的材质

读者可能注意到了，现在的盔甲，虽然看进来很不错，但是穿上去后就很容易发现，盔甲的外观，只是单调的两种颜色交替。这就是因为虽然我们指定了盔甲对应物品的材质，我们还没有指定盔甲本身的材质。

盔甲的材质图是两个大小为64x32的图片。还记得刚刚说的ArmorMaterial的构造方法的`name`参数吗？那个就决定了这两个图片的位置。

例如，钻石的`name`参数为`diamond`，其两张图片的位置就是`textures/models/armor/diamond_layer_1.png`和`textures/models/armor/diamond_layer_2.png`。

这里我们的ArmorMaterial的`name`参数为`fmltutor:redstone`，其两张图片的位置就是`fmltutor:textures/models/armor/redstone_layer_1.png`和`fmltutor:textures/models/armor/redstone_layer_2.png`了。我们在那里新建文件夹，把我们想要的两张图放进去就可以了。

现在打开原版的材质图，我们可以注意到一团乱糟糟的外观碎片被放到了一起。实际上，这些碎片的摆放位置都是有规律的：

![armor_texture_analysis](resources/armor_texture_analysis.png)

（材质分区图，其中F表示前面，B表示后面，L表示左面。R表示右面，U表示顶面，D表示底面，紫色背景表示尺寸，每格大小为7x7，边框尺寸为1）

我们注意到，这一张材质图被分成了五个大部分，每一个部分都有不同的尺寸。它们分别为头（Head，8x8x8），头饰（Headwear，8x8x8），下肢（RightLeg/LeftLeg，4x12x4），身体（Body，8x12x4），和上肢（RightArm/LeftArm，4x12x4）。每一个部分分成了六个小部分，表示六个面。

那。。。为什么是两张图呢？

这是因为当游戏渲染不同的盔甲的时候，使用的材质图不一样。当游戏渲染护腿时使用第二张图，这里就是`redstone_layer_2.png`，渲染其他类型的盔甲时使用第一张图，这里为`redstone_layer_1.png`。

游戏会根据玩家已经穿戴的盔甲，决定哪一部分被渲染：

* 当玩家穿戴上头盔，游戏渲染第一张图的Head和Headwear部分。
* 当玩家穿戴上胸甲，游戏渲染第一张图的Body和RightArm/LeftArm部分。
* 当玩家穿戴上护腿，游戏渲染第二张图的Body和RightLeg/LeftLeg部分。
* 当玩家穿戴上靴子，游戏渲染第一张图的RightLeg/LeftLeg部分。

这里准备了一张已经划分好不同部分的，大小为64x32的图，以方便读者设计盔甲。读者可以下载然后修改：

![armor_texture](resources/armor_texture.png)

我们这里使用这样的两张图（没错。。。调色。。。）：

**`src/main/resources/assets/fmltutor/textures/models/armor/redstone_layer_1.png:`**

![redstone_layer_1](resources/redstone_layer_1.png)

**`src/main/resources/assets/fmltutor/textures/models/armor/redstone_layer_2.png:`**

![redstone_layer_2](resources/redstone_layer_2.png)

打开游戏试试吧～

## 2.3.1 新的伤害类型

### DamageSource

原版提供了一个`DamageSource`类，并且预置了一些常用的`DamageSource`：

* `public static DamageSource inFire;` <br> 当站在火中时产生
* `public static DamageSource lightningBolt;` <br> 当遭雷劈时产生
* `public static DamageSource onFire;` <br> 当着火时产生
* `public static DamageSource lava;` <br> 当在岩浆中产生
* `public static DamageSource inWall;` <br> 当被方块窒息时产生
* `public static DamageSource drown;` <br> 当被水窒息时产生
* `public static DamageSource starve;` <br> 当饥饿值为零时产生
* `public static DamageSource cactus;` <br> 当被仙人掌刺伤时产生
* `public static DamageSource fall;` <br> 当受到跌落伤害时产生
* `public static DamageSource outOfWorld;` <br> 当跌落出这个世界时产生
* `public static DamageSource generic;` <br> 当死亡原因未知时产生
* `public static DamageSource magic;` <br> 当受到有伤害效果药水伤害时产生
* `public static DamageSource wither;` <br> 当被凋灵效果伤害时产生
* `public static DamageSource anvil;` <br> 当头顶铁砧时产生
* `public static DamageSource fallingBlock;` <br> 当头顶掉落的方块时产生

当希望对实体产生对应伤害时，就可以通过调用实体的`attackEntityFrom`方法，比如下面的例子：

```java
    player.attackEntityFrom(DamageSource.lightningBolt, 8.0F);
```

意思就是对装\*过度（误）的玩家产生八滴血的雷劈伤害。

### 创造一个新的DamageSource

我们注意到，`DamageSource`类只有一个构造方法参数：

```java
    public DamageSource(String damageTypeIn)
```

这个参数就是`DamageSource`的类型，决定着玩家死亡后会输出什么样的信息。

我们打开Minecraft原版的zh_CN.lang文件：

```lang
    ...
    death.attack.indirectMagic.item=%1$s 被 %2$s 用 %3$s 杀死了
    death.attack.lava=%1$s 试图在岩浆里游泳
    death.attack.lava.player=%1$s 在逃离 %2$s 时试图在岩浆里游泳
    death.attack.lightningBolt=%1$s 被闪电击中
    death.attack.magic=%1$s 被魔法杀死了
    death.attack.mob=%1$s 被 %2$s 杀死了
    death.attack.onFire=%1$s 被烧死了
    death.attack.onFire.player=%1$s 在试图与 %2$s 战斗时被烤的酥脆
    death.attack.outOfWorld=%1$s 掉出了这个世界
    death.attack.player=%1$s 被 %2$s 杀死了
    death.attack.player.item=%1$s 被 %2$s 用 %3$s 杀死了
    death.attack.starve=%1$s 饿死了
    death.attack.thorns=%1$s 在试图伤害 %2$s 时被杀
    death.attack.thrown=%1$s 被 %2$s 给砸死了
    death.attack.thrown.item=%1$s 被 %2$s 用 %3$s 给砸死了
    death.attack.wither=%1$s 凋零了
    ...
```

换言之，玩家死亡收到的信息，就是`death.attack.<damageTypeIn>`，或者`death.attack.<damageTypeIn>.item`。

我们新建这样一个事件：

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/EventLoader.java（部分）:`**

```java
        @SubscribeEvent
        public void onEntityInteract(EntityInteractEvent event)
        {
            EntityPlayer player = event.entityPlayer;
            if (player.isServerWorld() && event.target instanceof EntityPig)
            {
                EntityPig pig = (EntityPig) event.target;
                ItemStack stack = player.getCurrentEquippedItem();
                if (stack != null && (stack.getItem() == Items.wheat || stack.getItem() == Items.wheat_seeds))
                {
                    player.attackEntityFrom((new DamageSource("byPig")).setDifficultyScaled().setExplosion(), 8.0F);
                    player.worldObj.createExplosion(pig, pig.posX, pig.posY, pig.posZ, 2.0F, false);
                    pig.setDead();
                }
            }
        }
```

并在语言文件中加上：

**`src/main/resources/assets/fmltutor/lang/en_US.lang（部分）:`**

```lang
 death.attack.byPig=%s was dead because of a pig!
```

**`src/main/resources/assets/fmltutor/lang/zh_CN.lang（部分）:`**

```lang
 death.attack.byPig=%s被猪弄死了！
```

读者应该能够看明白，这段代码的作用就是当玩家向猪试图喂食小麦或者小麦种子的时候，因为喂错饲料而发怒（误）的那头猪会Boom，并给玩家一定的伤害。

### DamageSource的属性

刚刚读者可能已经注意到了，我们为这个`DamegeSource`赋予了两个属性：

* `setDefficultyScaled`方法设置的属性表示受到的伤害随着难度的变化而变化。
* `setExplosion`方法设置的属性表示该伤害由爆炸造成，爆炸保护附魔会起到作用。

除此之外，还可以设置`DamageSource`的其他属性：

* `setDamageBypassesArmor`设置伤害不会因为盔甲的保护而折减。
* `setDamageAllowedInCreativeMode`设置创造模式同样会受到伤害。
* `setDamageIsAbsolute`设置伤害是绝对的，不会受到附魔、药水效果等影响。
* `setFireDamage`设置伤害由火焰造成，火焰保护附魔会起到作用。
* `setMagicDamage`设置伤害是由药水造成的。
* `setProjectile`设置伤害由弹射物造成，弹射物保护附魔会起到作用。

## 2.3.2 新的附魔属性

### 概述

作者在玩MC这款游戏时一直在想：要是挖铁矿出铁，挖金矿出金，那将是一件多么美妙的事。本部分将带领大家，一步一步地制作一个被用于工具的新的附魔：火焰灼烧。

### Enchantment类

和附魔相关的类就是`Enchantment`类，打开`Enchantment`类，我们可以看到多个已经预设过的附魔种类。

我们先看看`Enchantment`类的构造方法：

```java
    protected Enchantment(int enchID, ResourceLocation enchName, int enchWeight, EnumEnchantmentType enchType)
```

我们解释一下这个构造方法的四个参数：

* `enchID`指的就是这个附魔的ID，我们看到原版已经定义了很多ID，当新建的ID重复时，游戏会报错。
* `enchName`指的就是这个附魔的名称，使用`ResourceLocation`的方式标记，比如时运就是`"minecraft:fortune"`，精准采集就是`"minecraft:silk_touch"`，这个名称和方块、物品的ID是类似的。
* `enchWeight`指的就是这个附魔的权重，和修复附魔需要的经验等级成负相关，和通过附魔台得到该种附魔的概率成正相关。
* `enchType`表示这种附魔是什么类型的，有武器、工具、弓等多种。

我们注意到，`enchID`如果重复，游戏会报错，所以我们将这个ID写进配置，使得玩家可以修改它，以免和原版或者某些Mod重复。

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/ConfigLoader.java（部分）:`**

```java
        public static int diamondBurnTime;
    
        public static int enchantmentFireBurn;
    
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
    
            comment = "Fire burn enchantment id. ";
            enchantmentFireBurn = config.get(Configuration.CATEGORY_GENERAL, "enchantmentFireBurn", 36, comment).getInt();
    
            config.save();
            logger.info("Finished loading config. ");
        }
```

然后我们新建包`com.github.ustc_zzzz.fmltutor.enchantment`，并在其中新建一个文件`EnchantmentFireBurn.java`：

**`src/main/java/com/github/ustc_zzzz/fmltutor/enchantment/EnchantmentFireBurn.java`**

```java
    package com.github.ustc_zzzz.fmltutor.enchantment;
    
    import com.github.ustc_zzzz.fmltutor.FMLTutor;
    import com.github.ustc_zzzz.fmltutor.common.ConfigLoader;
    
    import net.minecraft.enchantment.Enchantment;
    import net.minecraft.enchantment.EnumEnchantmentType;
    import net.minecraft.init.Items;
    import net.minecraft.item.ItemStack;
    import net.minecraft.util.ResourceLocation;
    
    public class EnchantmentFireBurn extends Enchantment
    {
        public EnchantmentFireBurn()
        {
            super(ConfigLoader.enchantmentFireBurn, new ResourceLocation(FMLTutor.MODID + ":" + "fire_burn"), 1,
                    EnumEnchantmentType.DIGGER);
            this.setName("fireBurn");
        }
    
        @Override
        public int getMinEnchantability(int enchantmentLevel)
        {
            return 15;
        }
    
        @Override
        public int getMaxEnchantability(int enchantmentLevel)
        {
            return super.getMinEnchantability(enchantmentLevel) + 50;
        }
    
        @Override
        public int getMaxLevel()
        {
            return 1;
        }
    
        @Override
        public boolean canApplyTogether(Enchantment ench)
        {
            return super.canApplyTogether(ench) && ench.effectId != silkTouch.effectId && ench.effectId != fortune.effectId;
        }
    
        @Override
        public boolean canApply(ItemStack stack)
        {
            return stack.getItem() == Items.shears ? true : super.canApply(stack);
        }
    }
```

`setName`方法的作用和方块、物品等的`setUnlocalizedName`方法类似，我们修改一下语言文件：

**`src/main/resources/assets/fmltutor/lang/en_US.lang（部分）:`**

```lang
    enchantment.fireBurn=Fire Burning
```

**`src/main/resources/assets/fmltutor/lang/zh_CN.lang（部分）:`**

```lang
    enchantment.fireBurn=火焰灼烧
```

`getMinEnchantability`和`getMaxEnchantability`方法的作用就是获取可以获取到此附魔的最低等级和最高等级。这里被设置成了和精准采集相同。

`getMaxLevel`方法指的就是这个附魔的最大等级了。自然，这个附魔只应该有一个等级。

`canApplyTogether`方法表示的是这个附魔可否与其他附魔共存。这里设定为不能和精准采集和时运共存。

`canApply`方法表示的是这个附魔可以作用的物品。既然是一个作用于工具的附魔，自然作用对象是所有工具和剪刀。

然后我们在`com.github.ustc_zzzz.fmltutor.enchantment`包下新建`EnchantmentLoader.java`文件，完成对这个附魔属性的注册：

**`src/main/java/com/github/ustc_zzzz/fmltutor/enchantment/EnchantmentLoader.java`**

```java
    package com.github.ustc_zzzz.fmltutor.enchantment;
    
    import com.github.ustc_zzzz.fmltutor.common.ConfigLoader;
    
    import net.minecraft.enchantment.Enchantment;
    
    public class EnchantmentLoader
    {
        public static Enchantment fireBurn;
    
        public EnchantmentLoader()
        {
            try
            {
                fireBurn = new EnchantmentFireBurn();
                Enchantment.addToBookList(fireBurn);
            }
            catch (Exception e)
            {
                ConfigLoader.logger().error(
                        "Duplicate or illegal enchantment id: {}, the registry of class '{}' will be skipped. ",
                        ConfigLoader.enchantmentFireBurn, EnchantmentFireBurn.class.getName());
            }
        }
    }
```

这里对该种附魔进行注册，如果ID重复，则输出错误信息。

`addToBookList`方法使得该附魔被注册，使其在附魔台上可以被注册到，在创造模式物品栏上也可以找到对应的附魔书。

下面是一张拥有此种附魔的钻石镐示例：

![fire_burn](resources/fire_burn.png)

### 完善你的附魔

为了使我们的附魔可以产生作用，我们需要在特定的地方监听事件，以使这个附魔产生作用：

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/EventLoader.java（部分）:`**

```java
        @SubscribeEvent
        public void onBlockHarvestDrops(BlockEvent.HarvestDropsEvent event)
        {
            if (!event.world.isRemote && event.harvester != null)
            {
                ItemStack itemStack = event.harvester.getHeldItem();
                if (EnchantmentHelper.getEnchantmentLevel(EnchantmentLoader.fireBurn.effectId, itemStack) > 0
                        && itemStack.getItem() != Items.shears)
                {
                    for (int i = 0; i < event.drops.size(); ++i)
                    {
                        ItemStack stack = event.drops.get(i);
                        ItemStack newStack = FurnaceRecipes.instance().getSmeltingResult(stack);
                        if (newStack != null)
                        {
                            newStack = newStack.copy();
                            newStack.stackSize = stack.stackSize;
                            event.drops.set(i, newStack);
                        }
                        else if (stack != null)
                        {
                            Block block = Block.getBlockFromItem(stack.getItem());
                            boolean b = (block == null);
                            if (!b && (block.isFlammable(event.world, event.pos, EnumFacing.DOWN)
                                    || block.isFlammable(event.world, event.pos, EnumFacing.EAST)
                                    || block.isFlammable(event.world, event.pos, EnumFacing.NORTH)
                                    || block.isFlammable(event.world, event.pos, EnumFacing.SOUTH)
                                    || block.isFlammable(event.world, event.pos, EnumFacing.UP)
                                    || block.isFlammable(event.world, event.pos, EnumFacing.WEST)))
                            {
                                event.drops.remove(i);
                            }
                        }
                    }
                }
            }
        }
```

我们监听了方块被挖掘后即将掉落物品的事件，在玩家手持存在“火焰灼烧”附魔的工具时，将其换成被灼烧过的物品掉落。

最后在`CommonProxy`中注册：

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/CommonProxy.java（部分）:`**

```java
        public void init(FMLInitializationEvent event)
        {
            new CraftingLoader();
            new EnchantmentLoader();
            new EventLoader();
        }
```

打开游戏试试吧～

## 2.3.3 新的药水效果

### 概述

很多MOD都添加了玩家的附属效果，使用药水效果的方式展示玩家的效果有着清晰直观、方便控制的特点，本部分将以一个拥有摔落保护的药水效果为例，一步一步地带领大家实现一个崭新的药水效果。

### 一个崭新的药水效果

新建包`com.github.ustc_zzzz.fmltutor.potion`，在其中新建一个文件`PotionLoader.java`：

**`src/main/java/com/github/ustc_zzzz/fmltutor/potion/PotionLoader.java:`**

```java
    package com.github.ustc_zzzz.fmltutor.potion;
    
    import net.minecraft.potion.Potion;
    import net.minecraftforge.fml.common.event.FMLPreInitializationEvent;
    
    public class PotionLoader
    {
        public static Potion potionFallProtection;
    
        public PotionLoader(FMLPreInitializationEvent event)
        {
            potionFallProtection = new PotionFallProtection();
        }
    }
```

在包`com.github.ustc_zzzz.fmltutor.potion`下新建文件`PotionFallProtection.java`：

**`src/main/java/com/github/ustc_zzzz/fmltutor/potion/PotionFallProtection.java:`**

```java
    package com.github.ustc_zzzz.fmltutor.potion;
    
    import com.github.ustc_zzzz.fmltutor.FMLTutor;
    
    import net.minecraft.potion.Potion;
    import net.minecraft.util.ResourceLocation;
    
    public class PotionFallProtection extends Potion
    {
        private static final ResourceLocation res = new ResourceLocation(FMLTutor.MODID + ":" + "textures/gui/potion.png");
    
        public PotionFallProtection()
        {
            super(new ResourceLocation(FMLTutor.MODID + ":" + "fall_protection"), false, 0x7F0000);
            this.setPotionName("potion.fallProtection");
            this.setIconIndex(0, 0);
        }
    }
```

我们讲一下`Potion`构造方法的三个参数：

* 第一个参数表示这个药水效果的名称，其使用方式和附魔相同
* 第二个参数表示这个附魔是否有害，这里很明显是无害的
* 第三个参数表示这个附魔的粒子效果（螺旋）颜色，这里定为深红色

`setIconIndex`方法表示这个药水效果在显示的时候使用的图标在下面这张图（来自`assets.minecraft.textures.gui.container.inventory.png`）中的位置，两个参数表示x和y坐标，这里设置为和速度药水效果的图标一致：

![inventory_potion_analysis](resources/inventory_potion_analysis.png)

`setPotionName`方法和附魔的`setName`方法，以及方块、物品等的`setUnlocalizedName`方法类似，我们修改一下语言文件：

**`src/main/resources/assets/fmltutor/lang/en_US.lang（部分）:`**

```lang
    potion.fallProtection=Fall Protection
```

**`src/main/resources/assets/fmltutor/lang/zh_CN.lang（部分）:`**

```lang
    potion.fallProtection=摔落保护
```

在`preInit`阶段初始化：

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/CommonProxy.java（部分）:`**

```java
        public void preInit(FMLPreInitializationEvent event)
        {
            new ConfigLoader(event);
            new CreativeTabsLoader(event);
            new ItemLoader(event);
            new BlockLoader(event);
            new PotionLoader(event);
        }
```

打开游戏，输入：

```
    /effect @a fmltutor:fall_protection
```

按下E键，就可以看到药水效果啦～

### 使药水效果起到作用

我们在适当的时机通过调用`EntityLiving`类的`getActivePotionEffect`方法来使得药水效果真正起到作用，这里我们监视事件`LivingHurtEvent`：

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/EventLoader.java（部分）:`**

```java
        @SubscribeEvent
        public void onLivingHurt(LivingHurtEvent event)
        {
            if (event.source.getDamageType().equals("fall"))
            {
                PotionEffect effect = event.entityLiving.getActivePotionEffect(PotionLoader.potionFallProtection);
                if (effect != null)
                {
                    if (effect.getAmplifier() == 0)
                    {
                        event.ammount /= 2;
                    }
                    else
                    {
                        event.ammount = 0;
                    }
                }
            }
        }
```

这段代码的作用就是当该药水效果等级为一时，摔落效果带来的伤害减半，如果等级超过一，伤害置零。

`PotionEffect`类和`Potion`类的区别就是`PotionEffect`是一个特殊化了的药水效果，该药水效果被赋予了时长和等级等。

### 让药水效果拥有自己的图标

刚刚我们可能注意到了，虽然我们可以指定药水效果的图标，但是这些图标都只被局限在一个图片中，所幸的是，Forge给我们提供了一个方法，让我们可以自定义药水的图标：

**`src/main/java/com/github/ustc_zzzz/fmltutor/potion/PotionFallProtection.java:`**

```java
    package com.github.ustc_zzzz.fmltutor.potion;
    
    import com.github.ustc_zzzz.fmltutor.FMLTutor;
    
    import net.minecraft.client.Minecraft;
    import net.minecraft.potion.Potion;
    import net.minecraft.potion.PotionEffect;
    import net.minecraft.util.ResourceLocation;
    
    public class PotionFallProtection extends Potion
    {
        private static final ResourceLocation res = new ResourceLocation(FMLTutor.MODID + ":" + "textures/gui/potion.png");
    
        public PotionFallProtection()
        {
            super(new ResourceLocation(FMLTutor.MODID + ":" + "fall_protection"), false, 0x7F0000);
            this.setPotionName("potion.fallProtection");
            // this.setIconIndex(0, 0);
        }
    
        @Override
        public void renderInventoryEffect(int x, int y, PotionEffect effect, Minecraft mc)
        {
            mc.getTextureManager().bindTexture(PotionFallProtection.res);
            mc.currentScreen.drawTexturedModalRect(x + 6, y + 7, 0, 0, 18, 18);
        }
    }
```

我们这里覆写的就是`renderInventoryEffect`方法，这个方法是当该药水效果的图标绘制时调用的。

除此之外，我们还要提供一个大小为256x256（其它尺寸是不可行的，只能256x256）的图片，并在左上角放上对应的18x18图标（这里终于是原创的了^_^）。

**`src/main/resources/assets/fmltutor/textures/gui/potion.png:`**

![gui_potion](resources/gui_potion.png)

（轻点打我。。。这个想法是一个叫作Blair的同学提供的，也为了纪念本部分的章节号23333333）<br>（我相信知道Blair是谁的人一定不会阅读到这部分教程(～￣▽￣)～）

现在，我们分析一下这个方法：

* `x`参数表示药水效果框左上角的横坐标
* `y`参数表示药水效果框左上角的纵坐标
* `effect`参数表示该药水效果对应的`PotionEffect`
* `mc`参数表示当前的这个游戏

`bindTexture`方法用于绑定我们想要用于绘制的图片，这里就是上面我们提供的图片。

`drawTexturedModalRect`方法就是用于绘制这个图标了，我们这里简要分析一下这个方法，该方法在后面的部分还会提到，并加以更加详细的解释：

* 第一个参数和第二个参数表示绘制的图标在游戏中的左上角的横纵坐标（xy值）。这里照着原版的数据做就行了
* 第三个参数和第四个参数表示绘制的图标在图片中的左上角的横纵坐标（uv值）。这里是整张图的左上角，自然都是零
* 第五个参数和第六个参数表示绘制的图标大小。这里和原版一样，是18x18

现在打开游戏，是不是看到自定义的药水效果图标了呢～

下面是效果示例图：

![fall_protection](resources/fall_protection.png)

## 2.4.1 热键绑定

### 概述

这一部分，作者将带领大家完成热键绑定的操作。在操作前，读者需要记住一点：**所有和热键绑定相关的操作都只在客户端执行。**

### KeyBinding

我们在包`com.github.ustc_zzzz.fmltutor.client`下新建一个文件`KeyLoader.java`：

**`src/main/java/com/github/ustc_zzzz/fmltutor/client/KeyLoader.java:`**

```java
    package com.github.ustc_zzzz.fmltutor.client;
    
    import net.minecraft.client.settings.KeyBinding;
    import net.minecraftforge.fml.client.registry.ClientRegistry;
    import org.lwjgl.input.Keyboard;
    
    public class KeyLoader
    {
        public static KeyBinding showTime;
    
        public KeyLoader()
        {
            KeyLoader.showTime = new KeyBinding("key.fmltutor.showTime", Keyboard.KEY_H, "key.categories.fmltutor");
    
            ClientRegistry.registerKeyBinding(KeyLoader.showTime);
        }
    }
```

然后我们在`ClientProxy`下注册：

**`src/main/java/com/github/ustc_zzzz/fmltutor/client/ClientProxy.java（部分）:`**

```java
        @Override
        public void init(FMLInitializationEvent event)
        {
            super.init(event);
            new KeyLoader();
        }
```

现在解释一下上面的代码。

首先，我们先定义一个`KeyBinding`，我们看一看`KeyBinding`的构造方法：

```java
        public KeyBinding(String description, int keyCode, String category) {...}
```

* `description`参数表示这个键的介绍。
* `keyCode`参数表示这个键的默认键名。这里是H。
* `category`参数表示这个键所在的键类别。

我们修改一下语言文件：

**`src/main/resources/assets/fmltutor/lang/en_US.lang（部分）:`**

```lang
    key.fmltutor.showTime=Show Time
    
    key.categories.fmltutor=FML Tutor
```

**`src/main/resources/assets/fmltutor/lang/zh_CN.lang（部分）:`**

```lang
    key.fmltutor.showTime=显示时间
    
    key.categories.fmltutor=FML教程
```

然后我们使用`ClientRegistry`的`registerKeyBinding`方法注册。

打开游戏，进入控制设置，我们就可以看到我们注册到的`KeyBinding`了：

![key_binding](resources/key_binding.png)

### 使绑定的热键产生作用

我们在注册的事件中加入监听按键按下的事件：

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/EventLoader.java（部分）:`**

```java
        @SideOnly(Side.CLIENT)
        @SubscribeEvent
        public void onKeyInput(InputEvent.KeyInputEvent event)
        {
            if (KeyLoader.showTime.isPressed())
            {
                EntityPlayer player = Minecraft.getMinecraft().thePlayer;
                World world = Minecraft.getMinecraft().theWorld;
                player.addChatMessage(new ChatComponentTranslation("chat.fmltutor.time", world.getTotalWorldTime()));
            }
        }
```

上面这段代码的作用，就是当键被按下的时候，如果检测到刚才我们注册的`KeyBinding`使用的键被按下，就向游戏控制台输入世界当前的总时间，也就是从世界创始以来的时间。

`ChatComponentTranslation`这个类的作用，就是把语言文件对应的信息翻译掉，翻译的本质方式是`String`类的`format`方法，也就是说，首先Minecraft会从对应的语言文件中获取`ChatComponentTranslation`类的第一个参数对应的内容，然后调用`String`类的`format`方法，使用后面的参数替换前面的格式符，比如`%s`（字符串），`%d`（整数）等。

所以我们现在更新一下语言文件：

**`src/main/resources/assets/fmltutor/lang/en_US.lang（部分）:`**

```lang
    chat.fmltutor.time=The total world time is: %s.
```

**`src/main/resources/assets/fmltutor/lang/zh_CN.lang（部分）:`**

```lang
    chat.fmltutor.time=现在时间为：%s。
```

打开游戏试试吧～

## 2.4.2 成就系统

### 概述

成就系统往往在一个游戏中占有着重要的部分，通过这个成就系统，游戏可以引导玩家向游戏希望的方向发展。一个好的游戏，往往都会有着完善的、引导性强的成就系统。Minecraft这款游戏的原版成就系统虽然引导性不够强，但却是相当完善的。Forge通过成就页的方式使得每个Mod都可以产生自己的成就系统。

本部分将一步一步地带领读者完成一个新成就，和一个新成就页。

### 新的成就

本部分我们首先试图制作一个在前面部分制作的当玩家提供给猪错误的饲料产生伤害而死的成就。

我们新建一个包`com.github.ustc_zzzz.fmltutor.achievement`，并在其中新建一个文件`AchievementLoader.java`：

**`src/main/java/com/github/ustc_zzzz/fmltutor/achievement/AchievementLoader.java:`**

```java
    package com.github.ustc_zzzz.fmltutor.achievement;
    
    import com.github.ustc_zzzz.fmltutor.item.ItemLoader;
    
    import net.minecraft.stats.Achievement;
    import net.minecraft.stats.AchievementList;
    
    public class AchievementLoader
    {
        public static Achievement worseThanPig = new Achievement("achievement.fmltutor.worseThanPig",
                "fmltutor.worseThanPig", 5, -4, ItemLoader.goldenEgg, AchievementList.buildSword);
    
        public AchievementLoader()
        {
            worseThanPig.setSpecial().registerStat();
        }
    }
```

Minecraft提供了一个`Achievement`类，现在我们看一个这个`Achievement`类的构造方法：

* 第一个参数是这个成就的名称。
* 第二个参数是这个成就的非本地化名称。
* 第三个参数和第四个参数是这个成就所在成就图中的位置，分别为x和y坐标。
* 第五个参数是这个成就在成就图上显示的图标。
* 最后一个参数是这个成就依赖的成就，如果这个成就独立，那么设为null。

为使得成就的非本地化名称生效，我们更改一下语言文件：

**`src/main/resources/assets/fmltutor/lang/en_US.lang（部分）:`**

```lang
    achievement.fmltutor.worseThanPig=Worse Than Pig
    achievement.fmltutor.worseThanPig.desc=Die from inappropriate food for the pig
```

**`src/main/resources/assets/fmltutor/lang/zh_CN.lang（部分）:`**

 achievement.fmltutor.worseThanPig=人不如猪
 achievement.fmltutor.worseThanPig.desc=因为提供给猪错误的饲料而死

含`desc`部分的行表示成就的显示描述，不含`desc`部分的行表示这个成就的显示名称。

下面这张图是一张Minecraft原版所有成就的坐标位置图，其中x和y均为零的位置，就是打开物品栏那个成就的位置：

![achievement_analysis](resources/achievement_analysis.png)

`setSpecial`方法用于设置这个成就是一种特殊成就，在成就图上会有花边，获得成就时显示的文字也是紫色的。

然后，我们通过对这个`Achievement`类的实例调用`registerStat`方法注册这个成就。

最后我们在`CommonProxy`完成注册：

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/CommonProxy.java（部分）:`**

```java
        public void init(FMLInitializationEvent event)
        {
            new CraftingLoader();
            new EnchantmentLoader();
            new AchievementLoader();
            new EventLoader();
        }
```

打开游戏，就可以在成就图上看到对应的成就了：

![achievement_example](resources/achievement_example.png)

### 新的成就页

Forge提供了一个名为`AchievementPage`的类，我们可以实例化这个类，并注册他们（这里新添加了两个成就）：

**`src/main/java/com/github/ustc_zzzz/fmltutor/achievement/AchievementLoader.java:`**

```java
    package com.github.ustc_zzzz.fmltutor.achievement;
    
    import com.github.ustc_zzzz.fmltutor.FMLTutor;
    import com.github.ustc_zzzz.fmltutor.block.BlockLoader;
    import com.github.ustc_zzzz.fmltutor.item.ItemLoader;
    
    import net.minecraft.init.Blocks;
    import net.minecraft.stats.Achievement;
    import net.minecraft.stats.AchievementList;
    import net.minecraftforge.common.AchievementPage;
    
    public class AchievementLoader
    {
        public static Achievement worseThanPig = new Achievement("achievement.fmltutor.worseThanPig",
                "fmltutor.worseThanPig", 5, -4, ItemLoader.goldenEgg, AchievementList.buildSword);
        public static Achievement buildGrassBlock = new Achievement("achievement.fmltutor.buildGrassBlock",
                "fmltutor.buildGrassBlock", 0, 0, Blocks.vine, null);
        public static Achievement explosionFromGrassBlock = new Achievement("achievement.fmltutor.explosionFromGrassBlock",
                "fmltutor.explosionFromGrassBlock", 2, -1, BlockLoader.grassBlock, buildGrassBlock);
    
        public static AchievementPage pageFMLTutor = new AchievementPage(FMLTutor.NAME, buildGrassBlock,
                explosionFromGrassBlock);
    
        public AchievementLoader()
        {
            worseThanPig.setSpecial().registerStat();
            buildGrassBlock.initIndependentStat().registerStat();
            explosionFromGrassBlock.setSpecial().registerStat();
    
            AchievementPage.registerAchievementPage(pageFMLTutor);
        }
    }
```

`AchievementPage`类是Forge提供的，作用就是把不同种类的成就以不同的成就页方式隔离。

`AchievementPage`类构造方法的第一个参数，是这个成就页的名称，这里用这个Mod的名称代替，而其第二个参数开始，就是这个成就页的所有成就，没有加入任何成就页的成就默认在原版的成就页上显示。

`initIndependentStat`方法用于设置这个成就是一个独立的、不依赖于其他成就的成就。很明显，一个成就页必须至少存在这样一个成就。

`registerAchievementPage`这个静态方法，就是注册这个成就页了。

最后我们修改一下语言文件，把新添加的两个成就对应的显示文字加进去：

**`src/main/resources/assets/fmltutor/lang/en_US.lang（部分）:`**

```lang
 achievement.fmltutor.worseThanPig=Worse Than Pig
 achievement.fmltutor.worseThanPig.desc=Die from inappropriate food for the pig
 achievement.fmltutor.buildGrassBlock=Grass Block
 achievement.fmltutor.buildGrassBlock.desc=Build a grass block
 achievement.fmltutor.explosionFromGrassBlock=Grass Block Explosion
 achievement.fmltutor.explosionFromGrassBlock.desc=Make an explosion of grass block by the click
```

**`src/main/resources/assets/fmltutor/lang/zh_CN.lang（部分）:`**

```lang
 achievement.fmltutor.worseThanPig=人不如猪
 achievement.fmltutor.worseThanPig.desc=因为提供给猪错误的饲料而死
 achievement.fmltutor.buildGrassBlock=草块
 achievement.fmltutor.buildGrassBlock.desc=制作一个草块
 achievement.fmltutor.explosionFromGrassBlock=草块爆炸
 achievement.fmltutor.explosionFromGrassBlock.desc=通过点击草块使草块爆炸
```

### 使成就可以被获得

我们可以注意到，我们添加的成就，并不能通过任何自然的、符合成就本身目的的方式获得，所以我们添加并修改一些事件：

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
                event.entityPlayer.triggerAchievement(AchievementLoader.explosionFromGrassBlock);
            }
        }
```

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/EventLoader.java（部分）:`**

```java
        @SubscribeEvent
        public void onLivingDeath(LivingDeathEvent event)
        {
            if (event.entityLiving instanceof EntityPlayer && event.source.getDamageType().equals("byPig"))
            {
                ((EntityPlayer) event.entityLiving).triggerAchievement(AchievementLoader.worseThanPig);
            }
        }
    
        @SubscribeEvent
        public void onPlayerItemCrafted(PlayerEvent.ItemCraftedEvent event)
        {
            if (event.crafting.getItem() == Item.getItemFromBlock(BlockLoader.grassBlock))
            {
                event.player.triggerAchievement(AchievementLoader.buildGrassBlock);
            }
        }
```

我们通过调用玩家的`triggerAchievement`方法，成功地使玩家获得了我们想要获得的成就。

## 2.4.3 系统命令

### 系统命令的意义

看起来，系统命令这种扩展，似乎只有服务端的插件才有做的必要，其实不然。这里，我们假设有一个Mod，被用于服务端，那么服务器的管理员该如何管控呢？

我们设想这样一个情况，如果这个Mod具有复杂的任务树，那么服务器的管理员可能就存在通过一种方式让玩家一下子跳到某一个任务节点的需要。

我们再设想，如果这个Mod给玩家添加了一个属性值，那个对于服务器的管理员来说，获取并试图修改这个属性值，可能就是一件十分必要的事情。

那么而这样的事情该如何做到呢？显然系统命令是最适合的，通过敲指令来达到这种目的往往是服务器的管理员最擅长的事情，同时，通过命令方块往往能达到自动化的操作。

### 目标

我们这部分将要带领读者一步一步地制作一个系统命令，这里的示例命令名称为`position`，用途是显示一个玩家的位置。

* 当输入命令：`/position`的时候显示自己的位置
* 当输入命令：`/position Player`的时候显示特定玩家`Player`的位置
* 当输入命令：`/position @a`的时候显示所有玩家的位置。

### 新建一个系统命令

这里，我们要用到一个之前讲到过的FML生命周期事件：`FMLServerStartingEvent`：

**`src/main/java/com/github/ustc_zzzz/fmltutor/FMLTutor.java（部分）:`**

```java
        @EventHandler
        public void serverStarting(FMLServerStartingEvent event)
        {
            proxy.serverStarting(event);
        }
```

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/CommonProxy.java（部分）:`**

```java
        public void serverStarting(FMLServerStartingEvent event)
        {
            
        }
```

我们注意到`FMLServerStartingEvent`有一个名为`registerServerCommand`的方法，显而易见，我们需要通过调用这个方法来完成系统命令的注册。

`registerServerCommand`方法需要一个实现了接口`ICommand`的对象，还好Minecraft这个游戏已经准备好了一个接口`ICommand`的方法大部分已经实现了的类：`CommandBase`。

新建包`com.github.ustc_zzzz.fmltutor.command`在其中新建文件`CommandPosition.java`，并使`CommandPosition`类继承`CommandBase`类：

**`src/main/java/com/github/ustc_zzzz/fmltutor/command/CommandPosition.java:`**

```java
    package com.github.ustc_zzzz.fmltutor.command;
    
    import net.minecraft.command.CommandBase;
    import net.minecraft.command.CommandException;
    import net.minecraft.command.ICommandSender;
    
    public class CommandPosition extends CommandBase
    {
        @Override
        public String getCommandName()
        {
            return "position";
        }
    
        @Override
        public String getCommandUsage(ICommandSender sender)
        {
            return "commands.position.usage";
        }
    
        @Override
        public void processCommand(ICommandSender sender, String[] args) throws CommandException
        {
    
        }
    }
```

我们注意到，对于`ICommand`接口，`CommandBase`类还有三个方法没有实现，当然，这三个方法的内容其实都非常简单：

* `getCommandName`方法就是这个命令的名称，也就是一个斜线之后紧接着出现的那个。
* `getCommandUsage`方法就是这个命令的用法，当玩家输入`/help position`的时候就会出现。这里自然需要国际化，随后我们就会在语言文件中添加相应字段。
* `processCommand`方法的含义更加显而易见，就是这个命令执行的时候会调用的方法。这里的`args`参数指的是**去掉命令名称本身**之后剩下的参数，比如如果我们输入`/position alice bob carol`，`args`就会是以`alice`、`bob`、`carol`为顺序的数组，如果我们只输入`/position`命令本身，那么`args`数组就是空的。

`processCommand`方法可能会抛出`CommandException`异常，这是因为在执行命令的时候可能会出现各种各样的错误，比如请求的玩家不存在、需要整数的地方提供了一个浮点数、等等，这时候就应该直接抛出异常，阻止命令的继续执行。

`CommandBase`类提供了很多静态方法，用于一些常见的命令操作，比如通过玩家名称取出对应的玩家实体、将字符串转换成整数、等等，开发者可以放心大胆地使用。这个部分使用了三个这样的方法：

* `getPlayer`方法用于通过玩家名称获取对应的玩家实体，如果无法找到，则会抛出异常。
* `getCommandSenderAsPlayer`方法用于获取输入该命令的玩家，如果该命令是命令方块等非玩家实体执行的，则会抛出异常。
* `getListOfStringsMatchingLastWord`方法用于将当前输入的字符串匹配对应字符串数组中对应的字符串列表，常用于自动补全。

更多方法的使用，只要参照原版的命令来模仿就可以了。

我们这里完成`processCommand`方法：

**`src/main/java/com/github/ustc_zzzz/fmltutor/command/CommandPosition.java（部分）:`**

```java
        @Override
        public void processCommand(ICommandSender sender, String[] args) throws CommandException
        {
            if (args.length > 1)
            {
                throw new WrongUsageException("commands.position.usage");
            }
            else
            {
                EntityPlayerMP entityPlayerMP = args.length > 0 ? CommandBase.getPlayer(sender, args[0])
                        : CommandBase.getCommandSenderAsPlayer(sender);
                Vec3 pos = entityPlayerMP.getPositionVector();
                sender.addChatMessage(new ChatComponentTranslation("commands.position.success", entityPlayerMP.getName(),
                        pos, entityPlayerMP.worldObj.provider.getDimensionName()));
            }
        }
```

代码的意思很简单，就是取出第一个参数对应的玩家实体，如果这个参数不存在就取自己，把其坐标和所在世界输出出来。如果提供的参数不正确，就会抛出异常。

我们现在补充一下语言文件：

**`src/main/resources/assets/fmltutor/lang/en_US.lang（部分）:`**

```lang
 commands.position.usage=/position [player]
 commands.position.success=The position of %1$s is %2$s in world %3$s
```

**`src/main/resources/assets/fmltutor/lang/zh_CN.lang（部分）:`**

```lang
 commands.position.usage=/position [玩家]
 commands.position.success=玩家 %1$s 处于名为 %3$s 的世界，其坐标为 %2$s
```

除了上面`CommandBase`未实现的方法，还有一个方法往往需要覆写，这就是`getRequiredPermissionLevel`方法，这个方法返回执行该命令所需要的等级。

等级一共分四种，对应的数字为1、2、3和4,等级为1代表任何玩家都可以执行，比如`/ping`这样的命令，等级为2代表命令方块可以执行，而等级4，则只有这个服务器的OP、还有单人模式下的作弊玩家可以执行。

这里我们把等级设置成2：

**`src/main/java/com/github/ustc_zzzz/fmltutor/command/CommandPosition.java（部分）:`**

```java
        @Override
        public int getRequiredPermissionLevel()
        {
            return 2;
        }
```

然后我们注册这个命令，在包`com.github.ustc_zzzz.fmltutor.command`下新建文件`CommandLoader.java`：

**`src/main/java/com/github/ustc_zzzz/fmltutor/command/CommandLoader.java:`**

```java
    package com.github.ustc_zzzz.fmltutor.command;
    
    import net.minecraftforge.fml.common.event.FMLServerStartingEvent;
    
    public class CommandLoader
    {
        public CommandLoader(FMLServerStartingEvent event)
        {
            event.registerServerCommand(new CommandPosition());
        }
    }
```

在`CommonProxy`中完成注册：

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/CommonProxy.java（部分）:`**

```java
        public void serverStarting(FMLServerStartingEvent event)
        {
            new CommandLoader(event);
        }
```

就可以了。

最后补充一点，`@p`、`@a`这样的通配符在Minecraft执行这个命令之前就已经展开成特定的名称了，比如这个服务器有`Alice`和`Bob`两个人，现在执行命令`/position @a`，相当于执行了一次`/position Alice`和一次`/position Bob`，所以这方面是不需要开发者操心的。

打开游戏试试吧～

### 命令的自动补全

很明显，没有自动补全的命令行界面，无论什么情况下（包括cmd），都是很难用的，所以这里我们理所应当地应该提供自动补全的功能。

当然，实现自动补全的方法也并不难，这里只要实现`ICommand`的`addTabCompletionOptions`方法（也就是覆写`CommandBase`的对应方法）就可以了：

**`src/main/java/com/github/ustc_zzzz/fmltutor/command/CommandPosition.java（部分）:`**

```java
        @Override
        public List<String> addTabCompletionOptions(ICommandSender sender, String[] args, BlockPos pos)
        {
            if (args.length == 1)
            {
                String[] names = MinecraftServer.getServer().getAllUsernames();
                return CommandBase.getListOfStringsMatchingLastWord(args, names);
            }
            return null;
        }
```

这里也就是说当玩家输入第一个参数的部分内容，比如`/position Ali`，或者仅仅输入了一个空格的时候，这个方法会让系统会找到所有这个服务器上的玩家，并且把对应的提供给系统。

上面的例子中，如果服务器中有一个名为`Alice`的玩家，命令行界面可能就会自动补全成`/position Alice`。

## 2.4.4 声音系统

### 概述

一个好的游戏，往往都有着丰富的声音系统，比如说在Minecraft这个游戏中，当你靠近一个洞穴的时候系统就会播放风声，当你的附近有着僵尸的时候就会传来僵尸的吼叫声，玩家可以通过声音来判断目前的状态，这无疑增加了游戏的趣味性和冒险性。

本部分内容将带领读者完成在Minecraft中导入声音、并在适当的时候播放声音的内容。我们假设读者已经拥有了想要应用在Minecraft中的**OGG格式的**音乐，也就是后缀名为ogg的音乐文件。这里我们采用的是本部分的作者使用计算机合成的一段非常短的，被用作音效的音乐。如有需要，可以通过这个[链接](https://github.com/ustc-zzzz/fmltutor/raw/master/src/main/resources/assets/fmltutor/sounds/fmltutor.ogg)获取音乐，并随意使用，作者不保留这段音乐的任何版权。

### 音乐索引文件

我们需要在`assets.fmltutor`包下创建一个文件：`sounds.json`：

**`src/main/resources/assets/fmltutor/sounds.json:`**

```json
    {
        "fmltutor.test": {
            "category": "player", 
            "sounds": [ 
                "fmltutor" 
            ] 
        }
    }
```

这个名为`sounds.json`的文件，就是这个Mod的音乐索引文件，其中贮存了各种各样音乐的索引。

这个文件的内容，是一段非常普通的JSON文件，这个文件通过储存JSON对象的方式提供键值对。这里的键值对中，标识符为这个声音的名称，在这里就是`fmltutor.test`，在程序中调用的时候要加上Mod id，也就是`fmltutor:fmltutor.test`，而值就是关于这个声音的信息。

* `category`表示的是这个声音的类型，总共有`ambient`（环境）、`weather`（天气）、`player`（玩家）、`neutral`（中立）、`hostile`（敌对）、`block`（方块）、`record`（唱片）、`music`（音乐）、`master`（控制）这八种类型。
* `sounds`表示的就是声音了，这里表示的声音存放在这个音乐索引文件所在目录下的`sounds`文件夹下，在这里就是`assets.fmltutor.sounds`包下，这里表示的声音是一个列表，在游戏中会随机选取其中一个所代表的声音播放。

`sounds`表示的声音列表还可以有`volume`、`pitch`等选项，分别表示响度、音量等。不过这里我们不作讨论，感兴趣的读者可以自己查找相关信息。

然后我们在`assets.fmltutor.sounds`下放置一个名为`fmltutor.ogg`的**OGG格式**的音乐，就可以了。

### 播放这个音乐

`net.minecraft.world.World`类有数个用于播放音乐的方法，其中有两个方法比较常用，其中一个为`playSoundAtEntity`，用于在特定实体所在位置播放音乐，还有一个就是`playSound`方法，用于在特定位置播放特定的声音，还可以设定这个声音是否有声速延迟。

我们先来看一下`playSound`方法：

* 前三个参数表示这个声音所在位置的坐标，分别为`x`、`y`、`z`。
* 第四个参数表示这个声音的名称，在上面的声音索引文件中有所提及。
* 第五个参数表示这个声音的响度，默认响度为1.0F。
* 第六个参数表示这个声音的音调，默认音调为1.0F。
* 最后一个参数表示这个声音是否有延迟，比如雷声就存在着延迟。

我们再来看一下`playSoundAtEntity`方法：

* 第一个参数表示该实体，没有什么过多的解释。
* 第二个参数表示声音的名称，和上面一样。
* 最后两个参数分别表示声音的响度和音调，和上面的同样没有差别。

有了这些，我们就可以试一试了：

**`src/main/java/com/github/ustc_zzzz/fmltutor/common/EventLoader.java（部分）:`**

```java
        @SubscribeEvent
        public void onPlayerItemCrafted(PlayerEvent.ItemCraftedEvent event)
        {
            event.player.worldObj.playSoundAtEntity(event.player, "fmltutor:fmltutor.test", 1.0F, 1.0F);
            if (event.crafting.getItem() == Item.getItemFromBlock(BlockLoader.grassBlock))
            {
                event.player.triggerAchievement(AchievementLoader.buildGrassBlock);
            }
        }
```

当玩家在工作台合成物品之后便会在玩家所在处播放一个叫作`fmltutor`的Mod下的一个名为`fmltutor.test`的声音，也就是这里的示例声音。

打开游戏试试吧～

## 2.5.1 在世界生成矿物

## 2.5.2 注册和使用矿物字典

## 2.6.1 创建并注册一份流体

## 2.6.2 为流体添加对应的桶
