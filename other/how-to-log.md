
> 转载： [一个著名的日志系统是怎么设计出来的？](https://mp.weixin.qq.com/s/XiCky-Z8-n4vqItJVHjDIg)

# 1. 前言
Java帝国在诞生之初就提供了集合、线程、IO、网络等常用功能，从C和C++领地那里吸引了大量程序员过来加盟，但是却有意无意地忽略了一个重要的功能： 输出日志。

对于这一点，IO大臣其实非常清楚， 日志是个很重要的东西， 因为程序运行起来以后， 基本上就是一个黑盒子，如果程序的行为和预料的不一致，那就是出现Bug了，如何去定位这个Bug 呢?   

臣民们能用的工具有两个，第一个就是单步调试，一步步地跟踪，查看代码中变量的值， 这种办法费时费力， 并且只能在程序员的机器上才能用。 

第二种就是在特定的地方打印日志， 通过日志的输出，帮助快速定位。尤其是当代码在生产环境上跑起来以后， 日志信息更是必不可少，要不然出了状况两眼一抹黑，上哪儿找问题去？ 总不能让臣民们把自己变成一个线程进入系统来执行吧？

但是IO大臣也有自己的小算盘： 日志嘛， 用我的System.out.println(.....) 不就可以了？！   我还提供了System.err.println不是？  

在IO大臣的阻挠下， 从帝国的第一代国王到第三代国王， 都没有在JDK中提供日志相关的工具包， 臣民们只好忍受着去使用System.out.println去输出日志，把所有的信息都输出到控制台， 让那里变成一堆垃圾。

# 2. 张家村
张家村的电子商务系统也不能幸免，自然也遇到了日志的问题。经验丰富的老村长已经烦透了System.out.println所输出的大量难于理解的无用信息，看着村民民整天苦逼地和这些System.out做斗争，他找来了小张，命令他设计一个通用的处理日志的系统。

小张在消息队列和JMS的设计上花了不少功夫， 积累了丰富的经验，从那以后一直都是实现业务代码，一直都是CRUD， 张二妮整天笑话自己是HTML填空人员，这一回一定要让她看看自己的设计功力！

老村长给小张下达的需求是这样的：

> 1. 日志消息除了能打印到控制台， 还可以输出到文件，甚至可以通过邮件发送出去（例如生成环境出错的消息）
> 2. 日志内容应该可以做格式化， 例如变成纯文本，XML, HTML格式等等
> 3. 对于不同的Java class，不同的 package ， 还有不同级别的日志，应该可以灵活地输出到不同的文件中。
>   例如对于com.foo 这个package，所有的日志都输出到 foo.log 文件中
>   对于com.bar 这个package ，所有文件都输出到bar. log文件中
>   对于所有的ERROR级别的日志，都输出到  errors.log文件中
> 4. 能对日志进行分级， 有些日志纯属debug ， 在本机或者测试环境使用， 方便程序员的调试， 生产环境完全不需要。有些日志是描述错误(error)的， 在生产环境下出错的话必须要记录下来，帮助后续的分析。

小张仔细看了看，拍着胸脯对老村长说：“没问题， 明天一定让您老看到结果。”

# 3. 小张的设计
老村长走了以后，小张开始分析需求， 祭出“面向对象设计大法”，试图从村长的需求中抽象出一点概念。

首先要记录日志，肯定需要一个类来表达日志的概念，这个类至少应该有两个属性，一个是时间戳，一个是消息本身，把它叫做LoggingEvent吧，记录日志就像记录一个事件嘛。

其次是日志可以输出到不同的地方，控制台、文件、邮件等等， 这个可以抽象一下，不就是写到不同的目的地吗？ 可以叫做LogDestination？ 

嗯， 还是简单一点，叫做Appender吧， 暗含了可以不断追加日志的意思。

![](http://mmbiz.qpic.cn/mmbiz_png/KyXfCrME6ULAiaQPr6M7tZ08qNJMSX8F9jWGmEHaiad3TFKM2dYayxQuTftpcvbOyh9lQmic0vicIoQj5tib8tHJpBg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

至于第二条的日志内容可以格式化，完全可以比葫芦画瓢， 定义一个Formatter接口去格式化消息。

![](http://mmbiz.qpic.cn/mmbiz_png/KyXfCrME6ULAiaQPr6M7tZ08qNJMSX8F9pI3GaoJIGBwdUdHc2wqNjRPJsXh3RtDR30lYyoj6pWRtKsQDt8lfxA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

对了， Appender 应该引用Formatter ，这样以来就可以对LoggingEvent记录格式化以后再发送。 

第三条需求把小张给难住了，不同的class, package 输出的目的地不同？  “目的地”这个概念是由Appender来表达的， 难道让不同的class, package 和Appender关联？ 不不， 不能这样 ！ 

还需要一个新的概念 ， 这个概念是什么？ 

从用户角度想一下， 村民们要想获取日志，必须得先获取个什么东西，这个东西是不是可以称为Logger啊？  灵感的火花就闪了那么一下就被小张抓住了： 获取Logger的时候要传入类名或者包名！ 

![](http://mmbiz.qpic.cn/mmbiz_png/KyXfCrME6ULAiaQPr6M7tZ08qNJMSX8F9c1BuuFnd4h741a4TwH788DX7Y6NR0Ou1UuN6rVwE27Mrwxic6B3LKeg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

这样一来，不同的class, package就区分开了， 然后让Logger 和Appender关联，灵活地设置日志的目的地， 并且一个Logger可以拥有多个Appender，同一条日志消息可以输出到多个地方， 完美！

![](http://mmbiz.qpic.cn/mmbiz_png/KyXfCrME6ULAiaQPr6M7tZ08qNJMSX8F9uh20S0SVwGfryJRPZ5vSN09KSBILQ8ZHcicElFicfZLOVRBkOappmWTQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

小张迅速地画出了核心类的类图：

![](http://mmbiz.qpic.cn/mmbiz_png/KyXfCrME6ULAiaQPr6M7tZ08qNJMSX8F92icibRuRY5taZKkRloU6q13ibcOHrlSVsBN8xshy9NWibh3H8QseoJyyLw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

还算漂亮，小张陶醉着自我欣赏了一下。 

再接再厉， 把第四条需求也设计一下，日志要分级，这个简单， 定义一个Priority的类，里边定义5个常量DEBUG, INFO, WARN, ERROR, FATAL， 表示5个不同的级别就OK了。当然这我5个级别有高低之分， DEBUG级别最低， FATAL级别最高。

还可以给Logger增加一些辅助编程的方法，如Logger.debug(....) , Logger.info(...)  , Logger.warn(...) 等等， 这样村民们将来就可以轻松地输出各种级别的日志了。

等一下， 老村长还说过“对于所有的ERROR级别的日志，都输出到  errors.log文件中” 类似这样的需求， 好像给忽略了。

这也好办嘛， 只要在Appender上增加一个属性，就叫做Priority， 如果用户要输出的日志是DEBUG级别， 但是有个FileAppender的Priority是 ERROR级别，那这个日志就不用在这个FileAppender中输出了 ，因为ERROR级别比DEBUG级别高嘛。

同理， 在Logger类上也可以增加一个Priority的属性，用户可以去设置， 如果一个Logger的Priority是ERROR， 而用户调用了这个Logger的debug方法， 那这个debug 的消息也不会输出。

小张全心全意地投入到设计当中，一看时间， 都快半夜了， 赶紧休息， 明天向村长汇报去。 

# 4. 正交性
第二天， 小张给老村长展示了自己设计的LoggerEvent, Logger , Appender, Formatter, Priority 等类和接口， 老村长捻着胡子满意地点点头：“不错不错，与上一次相比有巨大的进步。你知不知道我在需求中其实给了你引导？”

“引导？ 什么引导？ ”

“就是让你朝着正交的方向去努力啊”

“正交？ ”

‘“如果你把Logger, Appender, Formatter看成坐标系中的X轴，Y轴，Z轴， 你看看，这三者是不是可以独立变化而不互相影响啊？”



![](http://mmbiz.qpic.cn/mmbiz_png/KyXfCrME6ULAiaQPr6M7tZ08qNJMSX8F9sAG7d1AUxdoU4vt94W5ia5DUIZMqkz0lIxg22SCD79h9hogiaSgajo0g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

“我赛，果然如此，我可以任意扩展Appender接口而影响不到Logger和Formatter， 无论有多少个Logger 都影响不了Appender和Formatter ， 这就是正交了？”

“是啊，当你从系统中提取出正交的概念的时候，那就威力无比了，因为变化被封装在了一个维度上，你可以把这些概念任意组合，而不会变成意大利面条似的代码。 ”

听到村长做了理论的升华， 小张兴奋得直搓手。 

“好吧，你把这个设计实现了吧，对了，你打算叫什么名字？ ”  村长问道

“我打算把他叫做Log4j , 意思是Log for Java”

“不错，就这么定了吧”

# 5. Log4j
小张又花了两个月的时间把Log4j 开发了出来， 由于Log4j有着良好的设计，优异的性能， 不仅仅是张家村的人在用， Java帝国的很多村镇、部落都爱上了它。

后来张家村把Log4j 在Apache部落开源了， 这下子吸引了无数的人无偿帮助测试它，扩展它，改进它， 很快就成了帝国最流行的日志工具。

张家村建议帝国把Log4j 纳入到JDK 中， 帝国那效率低下的官僚机构竟然拒绝了。  消息传到了IO大臣的耳朵里，他不由的扼腕叹息： 唉，失去了一次极好的招安机会啊。 现在唯一的办法就是赶紧上奏皇上，在官方也提供一套，争取让臣民们使用官方版本。

到了第四代国王（JDK1.4)，臣民们终于看到了帝国提供的java.util.logging包，也是用来记录日志的，并且其中的核心概念Logger, Formatter, Handler 和 Log4j非常相似，只是为时已晚， Log4j早已深入人心了， 不可撼动了。  

# 6. 尾声
Log4j 在Apache开源以后， 小张也逐渐地有点落寞，他闲不住又写了一个工具，叫做logback, 有了之前的经验，这logback 比log4j 还要快。 

如今的日志世界有了很多的选择 ，除了java.util.logging, log4j 之外，还有logback，tinylog 等其他工具。

小张想了想， 这么多日志工具，用户如果想切换了怎么办？不想用log4j了，能换到logback吗？ 

我还是提供一个抽象层吧， 用户用这个抽象层的API来写日志， 底层具体用什么日志工具不用关心，这样就可以移植了。 

小张把这抽象层就叫做Simple Logging Facade for Java，简称SLF4J。  

![](http://mmbiz.qpic.cn/mmbiz_png/KyXfCrME6ULAiaQPr6M7tZ08qNJMSX8F9yktVnzoUhibpHtXmFx9bCqCyCjXOo1AA6djBRE8oWGx2XWicOfZlibeYA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

对于Log4j , JDK logging, tinylog 等工具， 需要一个适配层， 把SLF4J 的API转化成具体工具的调用接口。 

由于Logback这个工具也是出自小张之手， 直接实现了SLF4J的API，所以连适配层都不需要了， 用起来速度飞快，效率最高，SLFJ4+Logback 成为了很多人的最爱， 大有超越Apache Common Logging + Log4j 之势。 

后记： 本文主要想讲一下日志工具的历史和现状， 尤其是Log4j核心的设计理念。

文中的小张其实就是Ceki Gülcü，他开发了Log4j , logback,以及slfj4， 为Java的日志事业做出了卓越的贡献。

> [推荐阅读](https://mp.weixin.qq.com/s?__biz=MzAxOTc0NzExNg==&mid=2665513504&idx=1&sn=25dd6420e3056101dd3f6fdaedacaa2a&chksm=80d67a63b7a1f37572a5159ff6f53810467c15c8beec94770e8360c45f45036360d77755ee78&scene=21#wechat_redirect)




