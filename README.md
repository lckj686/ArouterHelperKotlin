### 一、概述
代码组件化，就会牵扯到代码隔离。 跨组件方案在充分使用代码隔离后都会出现一个问题。业务组件间需要相互调用，但由于使用了跨组件方案处理，代码隔离后 相关联的代码就失去了自动跳转的联系。阅读代码，和操作代码带来了一定程度的不便。

之前给公司内部的跨组件库写过一个idea插件，来帮助两个组件之间关联的代码进行导航。也有想过给[Aroute](https://github.com/alibaba/ARouter) 写一个，迟迟没有整理。Arouter有个官方的ArouterHelper 插件，提供了java代码的单向导航。但是在Kotlin里是失效的。so 写了一个支持Kotlin版本支持双向导航。

### 二、上效果图
- 使用 -> 箭头作为跳转到：Activity fragment 接口实现类
- 使用 <- 箭头作为反向跳转，反向跳转做了1跳多处理
#### 2.1 ARouter.getInstance().build("/xx/xxx").navigation(context) 和  @Route(path = "/xx/xxx") 之间的导航
- 使用 "/xx/xxx" 版本可以支持
![在这里插入图片描述](https://img-blog.csdnimg.cn/aebcd29d0c704c298914987fef2effb6.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xja2o2ODY=,size_16,color_FFFFFF,t_70#pic_center)
- 使用变量也可以支持
![在这里插入图片描述](https://img-blog.csdnimg.cn/c1874a0e52ff4bd3b472ee66296599de.png#pic_center)
####  2.2 @Autowired注解有支持
![在这里插入图片描述](https://img-blog.csdnimg.cn/d0d268b5c2b64ad59b18cda66a4f751f.png#pic_center)


#### 2.3 一对多时候的支持，是都列出来自己选择点击跳转
![在这里插入图片描述](https://img-blog.csdnimg.cn/94cdaacfab214b31a093474f480aab67.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xja2o2ODY=,size_16,color_FFFFFF,t_70#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/ef1b1edc83b543e1b2f995530f48cbf5.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xja2o2ODY=,size_16,color_FFFFFF,t_70#pic_center)
### 三、原理简单描述
Android studio plugin 使用的是idea plugin 的插件开发的同一套。
具体的开发流程可以参见：
-  [官方文档](https://plugins.jetbrains.com/docs/intellij/welcome.html)

- [(一)Idea 插件开发之环境搭建](https://blog.csdn.net/lckj686/article/details/106590205)
- [(二)Idea 插件开发之简单示例](https://blog.csdn.net/lckj686/article/details/106590928)
- [idea plugin 工程导入](https://blog.csdn.net/lckj686/article/details/106764074)

这里主要介绍一个与导航插件相关的类：LineMarkerProvider  
可以理解为一个行解析器，在代码文件被打开的时候，视觉当下的文件会从头到尾进行逐个元素psiElement 进行解析。

#### 导航器思路
1、捕捉当前文件下Arouter相关的点（这里叫它 src点）：
- @Route  
- @Autowired  
- ARouter.getInstance().build("/xx/xxx").navigation 

2、用src点去做全工程匹配，匹配到其它的关联点（这里叫它 target点），
再把目标点位置 写入到点击跳转中就可以了

#### 四、其它
#### 4.1 girhub地址：
[https://github.com/lckj686/ArouterHelperKotlin](https://github.com/lckj686/ArouterHelperKotlin)

#### 4.2 插件位置：
[https://github.com/lckj686/ArouterHelperKotlin/tree/master/release](https://github.com/lckj686/ArouterHelperKotlin/tree/master/release)
#### 4.3 idea plugin仓库：
[https://plugins.jetbrains.com/plugin/17334-arouterhelperkotlion?breakdown=by-time&measure=downloads-unique&period=day&update=131100](https://plugins.jetbrains.com/plugin/17334-arouterhelperkotlion?breakdown=by-time&measure=downloads-unique&period=day&update=131100)

