# MarkdownPlanner

在平时做项目的过程中，我们需要制定项目计划，跟踪项目进度，有一些常见的软件比如Microsoft Project, OminiPlan，这些软件都很棒。但是也有一些小问题:

* 需要不菲的Money
* 无法多人同时编辑 -- 这个在项目开始之初大家一起分工制定项目计划很有必要
* 项目计划无法通过浏览器方便的访问。
  * 因为这些软件的项目计划都是二进制文件，需要特定的软件才能打开。

而随着 Markdown 这种既可以用普通文本编辑器(Vim/Emacs)编辑, 又有一定样式的文件格式的流行，我一直想着用Markdown文件记录项目计划的
内容，因为Markdown文件的编辑非常的方便，这样就必须要有一个软件对这个项目文件进行解析展示, 因为我希望我只指定最少的信息，其它信息
需要由这个软件自动计算出来。

在我的脑海里，这个软件应该符合以下几个条件：

* 它要是基于Web的。
* 它允许多个人同时对项目文件进行编辑。
* 它对Markdown文件的格式要求要尽量的少。
  * Markdown文件的内容应该本身就是可读的，即使不使用这个软件辅助也可以看出项目的大概情况。
* 我不需要手动指定每个任务的开始时间，结束时间，而是自动计算出来。
  * 否则一旦中间某个任务发生变化，会影响这个任务后面所有其它任务的开始和结束时间。
* 除了基本的跟踪每个任务进展的功能，还要可以反映项目整体的进展情况，是否有延期，延期是否严重?
* 项目过程中，可能会有人请假，我应该只需要把请假的信息写进计划，而不需要因此调整很多任务的开始、结束时间。

总之一句话：

> 我只需要指定必要的信息: 项目什么时候开始；要做哪些事情；每件事情需要的人日、由谁负责；哪些人在哪些日子里面要请假，我便可以得到一个详细的计划。

经过几年的打磨(一点不夸张，换了至少三个方案)，目前这个版本我觉得已经比较满意和趁手，因此分享给大家。废话不多说，先上图:

一个测试项目所有任务的列表:

![任务列表](/images/task-list.png)

对任务的进度进行更新:

![任务的进度进行更新](/images/update-task-progress.png)

整个项目的一些统计信息:

![项目统计信息](/images/project-stat.png)

看了有这么多的功能，你可能会以为这背后一定有一个MySQL数据库吧? 没有，**所有这些信息都是来自一个普通的Markdown文件**，上面这个测试的项目文件在这里: [test.plan.md](/test.plan.md)。

## 下载、编译、运行

``` bash
# 下载
git clone git@github.com:xumingming/MarkdownPlanner.git
cd MarkdownPlanner
# 编译
mvn package
```

下面复制测试项目文件: `docs/test.plan.md` 到任意目录(比如`/tmp/test`), 然后到这个目录下面启动`MarkdownPlanner`:

``` bash
mkdir -vp /tmp/test
cp test.plan.md /tmp/test
cd /tmp/test
java -jar /path/to/your/MarkdownPlanner/target/MarkdownPlanner-0.1.0.jar --server.port=9999
```

现在打开浏览器访问 [http://localhost:9999/test.plan.md](http://localhost:9999/test.plan.md) 就可以访问到这个项目的页面了，祝你用得开心 :)。

## 语法

> 为了能够让我们MarkdownPlanner把你的markdown文件识别成一个项目文件，你的文件名必须以 `.plan.md` 结尾。

Markdown文件里面跟任务制定相关的一些小 **"语法"** :

指定项目从 `2018-01-22` 开始:

``` bash
* ProjectStartDate: 2018-01-22
```


一个由`詹姆斯`负责的、需要`1个人日`的、目前完成了 `80%` 的、名为 `数据的填充` 的任务:

``` bash
* 数据的填充 -- 1[詹姆斯][80%]
```

指定 `川普` 同学, `2018-01-22` 至 `2018-01-26` 期间不参与这个项目:

``` bash
* 川普 -- 2018-01-22 - 2018-01-26
```

Happy planning!
