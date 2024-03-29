### 为什么要规范提交代码

无规矩不成方圆，编程也一样,提交代码也一样。

Git是现在市面上最流行的版本控制工具，团队协作中，大家都张扬个性，那么代码将会是一团糟，好好的项目就被糟践了。不管是开发还是日后维护，都将是灾难。书

写良好的commit message能大大提高代码维护的效率。但是在日常开发中由于缺少对于commit message的约束，导致填写内容随意、质量参差不齐，可读性低亦难以维护。

好的提交说明可以提升代码可读性，同时也是提高代码审查效率的利器。通过制定严格的提交说明格式来规范其质量，可以方便审查者查理解被审查代码的意图、实现思路，并通过测试情况，加快对代码的理解，提高对代码质量的信心，从而大大提高审查者的效率。同时，严格的提交说明格式及好的说明质量也可以督促开发者提高代码质量。

在项目中引入commit message规范已是迫在眉睫。

### 代码提交原子性规范

代码提交的原子性，是指一个提交包含一个不可分割的特性、修复或者优化，同时这个提交要尽可能小。如果用一个提交完成一个功能，这个提交还是会比较大的话，我们需要把这个功能再拆分为子功能。

原子性提交的优点：

1. 可以让代码结构更清晰、更容易理解；
2. 出了问题之后方便定位，并可以针对性地对问题提交进行“回滚”；
3. 在功能开关的协助下，可以让开发者尽快把代码推送到 develop/master 上进行合并。这正是持续集成的基础。

规范要求：

1. 一个提交包含一个不可分割的特性、修复或者优化，同时这个提交要尽可能小。

代码审查标准

1. 不符合原子性提交的代码可以直接打回。
2. 逐步要求提交的原子性，不要求一次性到位，但必须逐步改进。


### 用什么规范

#### [Angular JS 规范](https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit#)

##### 格式如下 

```
<type>(<scope>): <subject> //消息头 # 50个字符以内，描述主要变更内容
<BLANK LINE> //空行
<body> //正文
# 主体内容：更详细的说明文本，建议72个字符以内。 需要描述的信息包括:
#
# * 为什么这个变更是必须的? 它可能是用来修复一个bug，增加一个feature，提升性能、可靠性、稳定性等等
# * 他如何解决这个问题? 具体描述解决问题的步骤
# * 是否存在副作用、风险
<BLANK LINE> //空行
<footer> //消息尾 #如果需要的化可以添加一个链接到issue地址或者其它文档，或者关闭某个issue。
```

*为了兼容多种工具方便阅读git message，禁止每一行超过100个字符！*

*一条commit message包含了header、body和footer，它们之间用空行分隔。*

**消息头**的格式指定为提交类型(`type`)、作用域(`scope`，可选)和主题(`subject`)

###### type 提交类型

1. `build`：对构建系统或者外部依赖项进行了修改。
2. `ci`：对CI配置文件或脚本进行了修改。
3. `docs`：对文档进行了修改。
4. `feat`：增加新的特征。
5. `fix`：修复`bug`。
6. `pref`：提高性能的代码更改。
7. `refactor`：既不是修复`bug`也不是添加特征的代码重构。
8. `style`：不影响代码含义的修改，比如空格、格式化、缺失的分号等。
9. `test`：增加确实的测试或者矫正已存在的测试。

###### 作用域

scope 是表明改动的区域，用于说明 commit 影响的范围，可以按项目模块或者功能，框架分层（数据层、控制层、视图层）。

###### 主题

scope 是commit 目的的简短描述。

###### 正文

body  部分是对本次 commit 的详细描述，可以分成多行。

###### 消息尾

footer 部分主要用于两种情况。

1. 不兼容变动
2. 关闭 Issue

*如果当前代码与上一个版本不兼容，则 footer 部分以`BREAKING CHANGE`开头，后面是对变动的描述、以及变动理由和迁移方法。*

*如果当前 commit 针对某个issue，那么可以在 footer 部分关闭这个 issue 。*

##### 回滚

如果当前提交回滚了上一次的提交，它的头部应该以`revert:`开头，紧接着是被回滚的提交记录的头部。body应该描述成`This reverts commit <hash>.`，hash是被回滚的提交记录的SHA值。

```
revert:<type>(<scope>): <subject>
<BLANK LINE>
This reverts commit hash
<other-body>
<BLANK LINE>
<footer>
```

### 按规范提交代码的好处

1. 提供更多的历史信息，方便快速浏览。
2. 可以过滤某些commit（比如文档改动），便于快速查找信息。
3. 可以直接从commit生成Change log。

