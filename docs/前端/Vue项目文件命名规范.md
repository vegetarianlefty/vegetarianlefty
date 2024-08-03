### 文件夹命名规范

#### 属于`components`文件夹下的子文件夹，使用大写字母开头的`PascalBase`风格

1. 全局通用的组件放在 /src/components下
2. 其他业务页面中的组件，放在各自页面下的 ./components文件夹下
3. 每个components文件夹下最多只有一层文件夹，且文件夹名称为组件的名称，文件夹下必须有`index.vue`或`index.js`，其他.vue文件统一大写开头（Pascal case），`components`下的子文件夹名称统一大写开头（PascalCase）

#### 其他文件夹统一使用`kebab-case`的风格

##### 全局公共组件：`/src/components`示例

```
  - [components]
    - [Breadcrumb]
      - index.vue
    - [Hamburger]
      - index.vue
    - [SvgIcon]
      - index.vue
```

##### 业务页面内部封装的组件：以 `/src/views/layout/components`示例

```
-[src]
  - [views]
    - [layout]
      - [components]
        - [Sidebar]
          - index.vue
          - Item.vue
          - SidebarItem.vue
        - AppMain.vue
        - index.js
        - Navbar.vue`
```

index.js 中导出组件方式如下：

```
export { default as AppMain } from './AppMain'
export { default as Navbar } from './Navbar'
export { default as Sidebar } from './Sidebar'
```

> 看index.js中最后一行代码，不难发现，为什么components下的子文件夹要使用`PascalCase`命名：
>
> ```
> export { default as Sidebar } from './sidebar' // 使用kebab-case命名的文件夹
> export { default as Sidebar } from './Sidebar' // 使用 PascalCase命名的文件夹
> ```
>
> 对于组件的导出/导入，我们一般都是使用大写字母开头的`PascalCase`风格，
> 以区别于.vue组件内部的其他`camelCase`声明的变量，
> `[Sidebar]`作为**【侧边栏组件】**的一个整体被导出，文件夹的命名也采用`PascalCase`，
> 有利于index.js中export时的前后统一，避免很多情况下不注意区分大小写

###  文件命名规范

####  `*.js`文件命名规范

1. 属于类的.js文件，除index.js外，使用`PascalBase`风格
2. 其他类型的.js文件，使用`kebab-case`风格
3. 属于Api的，统一加上`Api`后缀

#### `*.vue`文件命名规范

除index.vue之外，其他.vue文件统一用`PascalBase`风格

#### `*.less`文件命名规范

统一使用`kebab-case`命名风格