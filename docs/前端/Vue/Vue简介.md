### Vue.js 是什么

Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与现代化的工具链以及各种支持类库 结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

### Vue3 介绍

- 2020年9月18日，Vue.js 发布3.0版本，代号：One Piece（海贼王）

- 耗时2年多、2600+次提交、30+个RFC、600+次PR、99位贡献者

- github 上的 tags 地址：https://github.com/vuejs/vue-next/releases/tag/v3.0.0

### Vue 渐进式框架 

Vue 是一个框架，也是一个生态。其功能覆盖了大部分前端开发常见的需求。但 Web 世界是十分多样化的，不同的开发者在 Web  上构建的东西可能在形式和规模上会有很大的不同。考虑到这一点，Vue  的设计非常注重灵活性和“可以被逐步集成”这个特点。根据你的需求场景，你可以用不同的方式使用 Vue：

- 无需构建步骤，渐进式增强静态的 HTML
- 在任何页面中作为 Web Components 嵌入
- 单页应用 (SPA)
- 全栈 / 服务端渲染 (SSR)
- Jamstack / 静态站点生成 (SSG)
- 开发桌面端、移动端、WebGL，甚至是命令行终端中的界面

### Vue3 升级内容

#### 性能的提升

- 打包大小减少41%
- 初次渲染快55%, 更新渲染快133%
- 内存减少54%

#### 源码的升级

- 使用 Proxy 代替 defineProperty 实现响应式
- 重写虚拟 DOM 的实现和 Tree-Shaking

#### 拥抱 TypeScript

- Vue3 可以更好的支持 TypeScript

### Vue 的特点

1. 采用组件化模式，提高代码复用率、且让代码更好维护
2. 声明式编码，无需直接操作 DOM，提高开发效率
3. 使用虚拟 DOM 和优秀的Diff算法，尽量复用 DOM 节点
4. 编码简洁, 体积小, 运行效率高, 适合移动/PC 端开发
5. 遵循 **MVVM** 模式

#### 新的特性

1. Composition API（组合API）
   - setup 配置
   - ref 与 reactive
   - watch 与 watchEffect
   - provide 与 inject
   - ......
2. 新的内置组件
   - Fragment
   - Teleport
   - Suspense
3. 其他改变
   - 新的生命周期钩子
   - data 选项应始终被声明为一个函数
   - 移除 keyCode 支持作为 v-on 的修饰符