九大工具助你玩转Java性能优化
-----------------

在这篇文章中，我会带着大家一起看一下9个可以帮助我们优化Java性能的工具。有一些我们已经在IDR Solutions中使用了，而另外一些有可能在个人项目中使用。

**NetBeans Profiler**


NetBeans profiler是一个NetBeans IDE插件，主要为NetBeans IDE提供性能分析相关的功。NetBeans IDE是一个开源的集成开发环境。它很好地支持所有Java应用类型（包括Java SE、JavaFX、Java ME、Web、EJB和移动应用）的开发。

这个性能分析器包含了诸如CPU、内存和线程性能分析功能，并且提供了一些用于基本JVM监控的附加工具和功能。对于需要解决内存和性能相关问题的开发者非常有用。

**JProfiler**


JProfiler 在我之前的文章中有提及。同样，它也是一个很好的Java性能分析工具。JProfiler集CPU、内存和线程性能分析于一体，可以用于分析性能瓶颈、内存泄漏、CPU负载和解决线程相关的问题，并且支持本地性能分析（分析与JProfiler软件安装在同一台机的应用）和远程性能分析（它可以分析远程没有安装JProfiler机器上的应用），这一点对开发人员非常有用。

JProfiler由ej-technologies GmbH开发的商业授权的Java性能分析工具，主要为Java EE和Java SE应用所设计。

**GC Viewer**


GC Viewer可以（从主页和Github）免费获取。GC Viewer是一个开源工具，可以对vmflags -verboase:gc和-Xloggc:等Java VM选项产生的数据进行可视化分析。GC Viewer可以用于计算GC（垃圾回收）相关的性能数据记录，包括吞吐、累积暂停、最长时间的暂停等等。当你想要通过改变生成大小和设置初始堆大小来调整某个特定应用的GC时，它尤其有用。

GC Viewer是一个开源工具，由Tagtraum Industries Incorporated开发。这是一个很小的初创软件咨询公司，位于罗利、北卡罗莱纳州，由成立于1999年的非盈利项目Tagtraum Industries在2004年创立。

**VisualVM**


VisualVM 是一个由NetBeans平台派生的工具，遵循模块化的架构思想。这意味着可以通过插件方便的进行扩展。

Visual VM允许你获取Java程序的详细信息，只要它在一个Java虚拟机（JVM）上运行即可。生成的数据可以由JDK工具生成和读取，多个Java程序的所有数据和信息都可以很方便地进行查看，包括本地和远程的运行程序。同时可以保存JVM软件的数据快照，把数据保存在本地，方便以后进行查看或者和其他人一起分享。

Visual VM可以进行CPU性能分析、内存性能分析，进行GC（译者注：可以进行强制调用GC)、保存快照等。

**Patty**

“Patty” 项目 是一个开源项目，可以从Source Forge下载。它致力于为Java 1.5.0和更高版本的虚拟机提供性能分析工具。与其他性能分析工具不同之处在于，它专注性能分析，并且允许用户在运行时开启和关闭性能分析功能。

目前Patty正处于beta版阶段，但随着更多强大的功能如方法执行、代码覆盖、线程竞争（Thread Contention)分析的加入，Patty可以用于（内存、CPU等）性能调优，并且可以将信息通过TCP/IP Socket发送到其他电脑上。Patty有着易用的GUI，可以用于分析堆。还可以在Java编译流程中的准备阶段，对应用程序运行时对class进行监测（instrument）和取消监测（de-instrument）。

**JRockit——任务管理**


JRockit 是一个原由Appeal Virtual Machines开发的专有Java虚拟机。2002年由BEA Systems收购，之后从Sum Microsystems处转到Oracle。

Oracle的JRockit是一套完整的Java SE解决方案，包含了一个高性能JVM、性能分析、监控和排错工具。可用于预测Java程序中的延迟。

现在，JRockit打包有一系列的称为JRockit任务控制（JRockit Mission Control）的工具。这些工具包括：一个用于管理的控制台（console，译者注：此console跟终端不一样，是特指管理后台系统），进行GC数据可视化和其他的一些性能统计。它同样可以作为运行时性能分析工具Runtime Analyzer来使用，也可以分析内存问题。

**Eclipse Memory Analyzer**

Memory Analyzer (MAT) 可以在Eclipse IDE中找到.
Memory Analyzer (MAT) 可以在Eclipse IDE中找到.
Eclipse内存分析器(Eclipse Memory Analyzer)是一个可以帮助你找到内存泄漏和减少内存损耗的Java堆分析器。它更适合作为一个分析Java堆栈和计算大小的工具集，也可以用于监测内存泄漏和反模式带来的内存损耗。

**Java Interactive Profiler**


JIP是一个用Java开发的高性能、低损耗性能分析器。基于BSD许可协议发布，可以从Source Forge下载。使用JIP的开发者可以在VM运行时开启和关闭性能分析，并且可以过滤类和包、控制输出。

**Profiler4J**


Profiler4j 是一个专注于CPU性能分析的工具。它具有友好的用户界面，支持远程性能分析，并且支持动态修改配置。Profiler4j值得关注的特性有：基于动态字节码增强方式，这就表明它不需要任何本地的库和外部可执行文件支持。更重要的是，它完全由Java编写，可以提供图形化的调用图形信息、调用树结构、内存监控和类列表，支持细粒度配置文件。它基于Apache License v2.0协议发布，可以从Source Forge上下载

希望你觉得这些工具有用。

原文链接： http://blog.idrsolutions.com/2014/06/java-performance-tuning-tools/  
翻译： 陈 晓舜  
译文链接： http://www.importnew.com/12324.html  