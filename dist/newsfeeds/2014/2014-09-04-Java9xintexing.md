Java9新特性
----------

Oracle已将JAVA 9的开发提上日程。OpenJDK上已经出现了关于下一个主版本JAVA 9的改进建议（JEP）。与以往不同，Oracle在这次谈及了一些真正的特性。而早期对于JDK9的声明仅限于“bug修复和一些小的改进”。

译者注：有兴趣的同学可以看看JEP 地址如下 http://openjdk.java.net/jeps/0

校对注：JAVA还是包袱太重，更新特性非常谨慎和缓慢。


新API和性能特性：

下一个release版本将推出三个全新的API：

1、轻量级的JSON API(JEP 198):用于读写JSON文档和数据流。

2、HTTP 2客户端(JEP 110)：支持HTTP2.0和websocket，用以替代原有的HttpURLConnection。

3、进程API更新(JEP 102)：改进对操作系统进程的控制与管理（以往开发者只能用现有API提供的编写native代码的方式）

还有一些其他的小特性诸如在JEP中提及的数十个建议。此外，Oracle还承诺了另外三件有关性能的特性：

1、改进竞争锁(JEP 143)：此项旨在于改进当线程竞争访问对象时的性能。

2、分段代码缓存(JEP 197)：更好的性能，更短的扫描时间，更少的碎片，以及其他扩展能力。

3、智能的JAVA编译器sjavac(JEP 199)：默认使用sjavac来构建更为大型的项目。

JAVA的native接口会被作为本地运行时项目的一部分重新规划，2011 JavaOne大会上曾经指出，Oracle还讨论了2016年发布JAVA 9将支持多GB堆和自调节JVM。

模块化源代码
如果上述提到的特性不能满足你的胃口，Oracle还承诺了提供模块化源代码（JEP 201）。此项改进旨在重新组织JDK源码，使之模块化，同时为实现Jigsaw项目打下重要的基础。

被JAVA 7放弃的Jigsaw又回到了JAVA 9中，成为了下一个版本中讨论的热点话题。Jigsaw的主要目标是为小型设备提供扩展性，为JDK和JAVA SE提升安全性和性能，更方便的构建大型项目和类库。同时Penrose项目用于实现Jigsaw和OSGi之间的交互能力。

Georges Saab，Oracle JAVA平台组软件开发副总裁告诉JAXenter，目前的主要工作集中在Jigsaw项目，开发团队正在探索并构建简单的访问原型以确保在JAVA 9发布时可以使用。

不稳定的发布历史

Oracle在JAVA版本发布上是出了名的不准时，曾经多次的跳票，比如跳票到让人无奈的lambda项目，还有声名狼藉的基于Applet的安全性问题，这些使得Oracle发布JAVA 8整整推迟了两年。而且自发布后，JAVA 8还导致了许多开发工具无法使用。

JAVA 9预计2016年发布，留给Oracle的时间不到两年（而不是通常的三年），而且还需要足够的时间处理各方需求、谣言、新特性的公告等等，和其他不可避免的延期。

原文地址 :http://jaxenter.com/java-9-features-announced-50896.html  
作者：Coman Hamilton   
译者：zachariah    
校对：方腾飞  