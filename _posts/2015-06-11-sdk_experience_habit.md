---
layout: post
title: SDK开发经验之开发习惯
category: 经验总结
tags: SDK
keywords: keywords
description: desc
---
昨天写架构和资源的时候才发现已经写了两个月了～～～决定最近要尽快把这些长文都写完。这样才好安心看Android traning相关的内容。关于开发习惯这部分内容，其实跟SDK关系不大，只是在SDK开发中逐渐沉淀下来的，而且有些也已经纳入到我们的流程中了，所以就简单汇总说明一下。

### TODO

#### TODO有什么用

TODO顾名思义就是要做的事情，一般你当前做不了但是迟早需要做的事，你都可以用TODO标签标记了。尤其是在代码中有改动但是没写完临时有事走开等时候。通过TODO可以高效的找到你认为后续还要确认的代码位置。

#### TODO怎么用

TODO标签是目前个人感觉作用最大的一个功能。一般的每个的IDE都会有tasklist。例如下面是eclipse的：

![eclipse todo](./../public/images/sdk_eclipse_todo.png)

当你在代码中添加注释的时候以大写的TODO开始，这个标记就会出现在tasklist里面。例如下面的TODO就对应tasklist里面的第一条TODO信息：

	// TODO GAME 初始化MSDK

#### TODO哪里用：

1. 每次版本提测前都会有一些重点检查项。例如DB版本、SDK版本等内容。将这些要检查的位置加上TODO，每次提测前只需要把TODO过一遍就过了所有的要检查项目。
- 开发中有时候为了配合测试一些新功能会对一些参数作调整，例如
	- 定时任务的间隔时间
	- 一些配置开关，例如加密不加密等
	- 一些特定逻辑，可能会写死为false或者true强制走进分支方便测试等 

	有时候开发周期比较长，很容易忘了最开始的修改，也容易出问题，如果在修改的时候就加个TODO，等到最终提测版本时同样能够发现。例如下面这样：
	
		// TODO hardyshi 为了查看请求内容，临时解密，最终发布时修改回去
    	public static final Boolean isEncode = false;

- 开发中代码写到一半被更高优先级的事情打断，为了临时标注一下，后续处理。例如：

		//TODO hardyshi 暂时写到这里，走开一下，回来继续完成数据落地到DB的逻辑
		
- 开发中有时候逻辑比较多，或者比较复杂，可能会优先完成主体部分而遗漏一些待处理的分支，例如：
	- 一些简单的分支逻辑
	- 一些异常逻辑处理
	- 一些特殊数据上报等为接口等服务的逻辑等
	
	这些点同样属于重要但是容易遗漏的点，尤其是如果开发周期比较久的时候，使用TODO就可以轻松避过这些问题了。

- 对于SDK的功能，开发者怎么去接入，其实我们也用TODO标签标注，开发者接入某一个功能，只需要处理了对应的TODO即可完成接入，不过貌似没有人关注。

#### TODO怎么写

好的TODO标签可以起到上面描述的各种问题，但是不好的TODO其实没什么作用，搞不好还会让别人更困惑。我们的TODO的格式是：“**`TODO 负责人 信息`**”三部分组成，具体的例如：

- 临时类的： 

		// TODO hardyshi（负责人，如果最后看不懂这个TODO，就找他） 具体要做什么事情 
- 提醒类的：

		//TODO GAME 游戏需要在onCreate里面完成初始化

切记尤其是个人临时添加的TODO，一定要加上负责人，不然最终会跪了～～～
	
### 无用代码的处理

任何SDK都会不断的更新和迭代，带来最直接的问题就是不断的有新代码，不断的有老代码被替换。代码中那些被注释了毫无用处的代码该如何处理呢？

在已经有SVN，git这样异常成熟的版本控制工具的今天就不要再用注释来保留哪些无用的代码了。第一个把代码注释了的人最可恨。因为后面的人看到了一般不敢删除，虽然知道他是已经被注释的。**对于没有的代码，测试验证OK了以后就请删除吧，别留下来了，就算将来有用，也删了吧，实在不行先SVN提交一个版本，然后你再删了。**

我们有一个开发哥哥，在修改逻辑的时候喜欢把新写的和老的逻辑放在一起来对比验证。但是每次对比完了总是不删，还加一个注释：这个地方有点问题，暂时先放着，后买呢在看～～其实他已经看完了，并且验证没有问题了。但是无数个人看到这个以后都不敢动，怕改错，因为说了有问题！最后不得不动的时候一问他，这个地方他改好了，忘了删注释和肥代码！！！！折磨人啊。

解决这个问题其实并不难，就是两点：

- 无用代码及时删除，不要保留
- 还需要验证的地方加个TODO标签，最终回头再来看。

### 注释

关于注释，永远有说不完的。就简单总结一下吧：

- **没有用的注释还不如没有**
- 注释和代码不要写一行。例如下面这种：
		
		private int a = 1;//这个参数是用来XXXXXX
	
	这种注释被一些代码格式化工具处理以后会很难看。

之前本来这部分计划要写的东西比较多，但是目前忽然觉得写不写的意义不大，那就暂时到此了。后续如果有想到更多需要特别说明的再来追加。
