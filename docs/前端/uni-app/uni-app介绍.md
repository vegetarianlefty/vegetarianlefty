### uni-app 介绍

`uni-app` 是一个使用 [Vue.js](https://vuejs.org/) 开发所有前端应用的框架，开发者编写一套代码，可发布到iOS、Android、Web（响应式）、以及各种小程序（微信/支付宝/百度/头条/飞书/QQ/快手/钉钉/淘宝）、快应用等多个平台。

`uni-app`在开发者数量、案例、跨端抹平度、扩展灵活性、性能体验、周边生态、学习成本、开发成本等8大关键指标上拥有更强的优势。

### 只开发小程序，需要 uni-app 吗

是的，单独开发小程序，也应该使用uni-app。它比其他小程序框架或原生小程序开发更有优势。原因如下：

1. uni-app无需追随微信升级，可不受限在条件编译里使用wx的现在或未来的所有api
2. uni-app的性能比一般人手写的微信原生代码性能更高。就像vue操作比一般人写js操作dom性能更高一样。底层自动diff差量更新数据，比手动setData性能更高。评测数据见下文
3. uni-app是纯vue语法，不必另学一种dsl。开发不同项目时，思维不用切换
4. uni-app的组件、模板非常丰富，插件市场数千款插件。如富文本解析、图表、自定义下拉刷新等组件，uni-app版插件性能均超过了wxparse、wx-echart等微信小程序组件
5. HBuilderX比微信工具更强大，开发效率更高。哪怕使用vscode等工具，由于这些工具对vue的支持强于对wxml的支持，所以开发效果也会更高
6. 微信原生开发对webpack、预编译语言、工程流程管理很多功能都不支持，大公司很少用微信原生开发，都是在用框架来提升开发效率
7. uni-app支持双向数据绑定、vuex状态管理，比小程序原生开发方便的多
8. 迟早会有多端需求，使用`uni-app`再无后续顾虑
9. uni-app并非仅用于做跨端的，只用uni-app做小程序、只做web、只做App的，案例是一样多的，详见：https://uniapp.dcloud.io/case 关于uni-app和微信开发的详细比较评测，参考：https://ask.dcloud.net.cn/article/36484

- 评测1、uni-app和原生wxml开发、wepy、mpvue、taro的对比，https://ask.dcloud.net.cn/article/35867
- 评测2、uni-app和微信原生开发的详细比较评测，参考：https://ask.dcloud.net.cn/article/36484

### 只开发App，需要uni-app吗？

`uni-app`是更好的跨平台开发框架，开发一次iOS、Android都有了。体验好、开发效率高。

`uni-app`在App端，基于能力层/渲染层分离的架构设计（见下图），渲染层是webview和weex二选一，能力调用都是共同的plus api，比如蓝牙、扫码等能力；也就是weex被内置到`uni-app`中，并且被强化了。

过去weex有个很大的问题是api太少，开发时必须iOS、Android原生和前端3拨团队协作开发，实际上react native也如此，因为他们的核心只是高性能渲染器。

uni-app提供了大量的扩展api解决了这个问题，并且发展了成熟多样的插件生态，大多数App的开发不再需要原生介入了，从而把跨平台开发省成本这个核心目的落地了。

`uni-app`在App侧可以使用丰富的小程序sdk，如网易云信、环信、七牛等众多sdk厂商均原厂维护其小程序sdk版本，而这些sdk均可直接用于uni-app并发布为iOS、Android的App。

`uni-app`的插件市场里有非常多的ui库、组件、模板，可以大幅提升开发效率。

相比纯原生开发，`uni-app`体验可商用，也不会限制功能调用，但开发效率和开发成本更优于原生开发。

如果你已经有了原生App，那么可以局部使用`uni-app`，内嵌uni小程序SDK，把部分栏目小程序化，或者直接打造自己的小程序平台。

### uni-app是多端写在一个项目统一升级维护，还是每个端不同的项目，只复用部分代码

`uni-app`是多端写在一个基础项目下，差异使用条件编译来管理。

这有个巨大的好处是一套工程代码，升级时可多端同时更新。

如果把不同端的项目分开，那么维护升级时非常麻烦，无法方便同步升级。

### uni-app 学习成本高吗？基于什么技术栈？

`uni-app`简单来说是 vue的语法 + 小程序的api。

它遵循`Vue.js`语法规范，组件和API遵循`微信小程序命名`，这些都属于通用技术栈，学习它们是前端必备技能，`uni-app`没有太多额外学习成本。

有一定 Vue.js 和微信小程序开发经验的开发者可快速上手 `uni-app` 。

没学过vue的同学，也不用掌握vue的全部，只需了解vue基础语法、虚拟dom、数据绑定、组件、vuex，其他如路由、loader 不用学，cli、node.js、webpack也不需要学。

### uni-app 开发体验如何？支持现代前端开发流程吗？

`uni-app` 积极拥抱社区现有的现代开发流程，包括但不限于：

- 内置了webpack/vite
- NPM 包管理系统，详见[参考](http://uniapp.dcloud.io/tutorial/page-script#npm支持)
- es6+ 语法（发布时会自动编译为es5），详见[参考](http://uniapp.dcloud.io/tutorial/syntax-js#es6-支持)
- 各种预处理器（less、scss、stylus、typescript）
- uni-app的官方ide：HBuilderX，在vue、json、markdown、代码提示、操作效率上，有非常明显的优势，可帮助开发者大幅提高工作效率
- uni-app同时也提供了cli方式，可使用其他开发工具如vscode开发，当然开发效率不如HBuilderX。对比详见https://ask.dcloud.net.cn/article/35451
- HBuilder也提供了cli，[参考](https://hx.dcloud.net.cn/cli/README)

### uni-app 生态开放性如何？能否直接利用现有前端社区资源？

`uni-app` 提供了开放性的生态：

- 丰富的插件市场为开发者提供数千款现成的轮子，[https://ext.dcloud.net.cn](https://ext.dcloud.net.cn/)
- 兼容微信小程序 JS SDK，丰富的小程序生态内容可直接引入uni-app，并且在App侧通用，[参考](http://ask.dcloud.net.cn/article/35070)
- 兼容微信小程序自定义组件，并且App、web侧通用，[参考](http://uniapp.dcloud.io/tutorial/miniprogram-subject#小程序自定义组件支持)
- App和web提供了renderjs，使得浏览器专用的库也可以在App和web里使用，[参考](https://uniapp.dcloud.io/tutorial/renderjs)
- 支持 NPM 包管理系统，[参考](http://uniapp.dcloud.io/tutorial/page-script#npm支持)
- 支持 mpvue 项目及组件，[参考](http://ask.dcloud.net.cn/article/34945)
- 支持原生插件，见插件市场：[https://ext.dcloud.net.cn](https://ext.dcloud.net.cn/)
- 支持App原生工程里嵌入uni小程序sdk。

### uni-app 支持的手机版本最低到多少？

1. Web端：uni-app没有限制，同vue2和vue3自身能支持的浏览器版本
2. 小程序端：uni-app没有限制，同该小程序自身能支持的最低平台
3. App端：
   - Vue2: Android4.4+、iOS9+。Android4.4已经是2013年发布的手机了。
   - Vue3: 支持的范围是：Android >=5 （使用nvue和vue有区别。某些老国产Android5的rom无法动态升级Android system webview，此时如果使用vue页面需搭配x5内核） , iOS >= 10

### 功能框架图

![img](http://image.wangxiaohuan.com/blog/image/202401271603297.png)

![img](http://image.wangxiaohuan.com/blog/image/202401271610669.png)