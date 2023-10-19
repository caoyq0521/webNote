# Vant cli

!> 有赞出品 Vue 组件库构建工具

[Vant Cli](https://github.com/youzan/vant/blob/main/packages/vant-cli/README.zh-CN.md) 是一个 Vue 组件库构建工具，通过 Vant Cli 可以快速搭建一套功能完备的 Vue 组件库。

## 特性

* 提供丰富的命令，涵盖从开发测试到构建发布的完整流程

* 基于约定的目录结构，自动生成优雅的文档站点和组件示例

* 内置 ESlint、Stylelint 校验规则，提交代码时自动执行校验

* 构建后的组件库默认支持按需引入、主题定制、Tree Shaking

## 快速上手

执行以下命令可以快速创建一个基于 Vant Cli 的项目：

````js
yarn create vant-cli-app
````

## 手动安装

````js
npm i @vant/cli -D
or
yarn add @vant/cli -D
````

安装完成后，请将以下配置添加到`package.json`文件中

````json
{
  "scripts": {
    "dev": "vant-cli dev",
    "test": "vant-cli test",
    "lint": "vant-cli lint",
    "build": "vant-cli build",
    "prepare": "husky install",
    "release": "vant-cli release",
    "build-site": "vant-cli build-site"
  },
  "lint-staged": {
    "*.md": "prettier --write",
    "*.{ts,tsx,js,vue,less,scss}": "prettier --write",
    "*.{ts,tsx,js,vue}": "eslint --fix",
    "*.{vue,css,less,scss}": "stylelint --fix"
  },
  "eslintConfig": {
    "root": true,
    "extends": ["@vant"]
  },
  "stylelint": {
    "extends": ["@vant/stylelint-config"]
  },
  "prettier": {
    "singleQuote": true
  },
  "browserslist": ["Chrome >= 51", "iOS >= 10"]
}
````

## 命令

Vant cli中内置了一系列的命令，可以将命令添加到`npm scripts`中进行使用。

````json
// package.json
{
  "scripts": {
    "dev": "vant-cli dev",
    "test": "vant-cli test",
    "lint": "vant-cli lint",
    "release": "vant-cli release",
    "build-site": "vant-cli build-site"
  }
}
````

也可以通过 npm 自带的 [npx](https://github.com/npm/npx) 直接执行某个命令：

````bash
npx vant-cli dev
````

## dev

> 运行本地开发环境。

运行dev命令时，Vant cli会通过[webpacl-dev-server](https://github.com/webpack/webpack-dev-server)启动一个本地服务器，用于再开发过程中对文档和示例进行预览。

## build

> 构建组件库

运行build命令会在`es`和`lib`目录下生成可用于生产环境的组件代码，详见[目录结构](https://github.com/youzan/vant/blob/main/packages/vant-cli/docs/directory.md)

发布npm时，请将以下配置加入到`package.json`中，使npm包能被正确识别：

````json
// package.json
{
  "main": "lib/index.js",
  "module": "es/index.js",
  "files": ["es", "lib"]
}
````

## build-site

构建文档站点，在 `site` 目录生成可用于生产环境的文档站点代码。

## release

发布组件库，发布前会自动执行 build 和 changelog 命令，并通过 [release-it](https://github.com/release-it/release-it) 发布 npm 包。

## changelog

基于 commit 记录生成更新日志，基于 [conventional-changelog](https://github.com/conventional-changelog/conventional-changelog) 实现。

## commit-lint

校验 commit message 的格式是否符合规范，需要配合 `husky` 在提交 commit 时触发。

````js
npx husky add .husky/commit-msg 'npx --no-install vant-cli commit-lint $1'
````

## 配置指南

### vant.config.js

`vant.config.js`中包含了`vant-cli`的打包配置和文档站点配置，请创建此文件并置于项目根目录下。下面是一份基本配置的示例：

````js
module.exports = {
  // 组件库名称
  name: 'demo-ui',
  // 构建配置
  build: {
    site: {
      publicPath: '/demo-ui/',
    },
  },
  // 文档站点配置
  site: {
    // 标题
    title: 'Demo UI',
    // 图标
    logo: 'https://img.yzcdn.cn/vant/logo.png',
    // 描述
    description: '示例组件库',
    // 左侧导航
    nav: [
      {
        title: '开发指南',
        items: [
          {
            path: 'home',
            title: '介绍',
          },
        ],
      },
      {
        title: '基础组件',
        items: [
          {
            path: 'my-button',
            title: 'MyButton 按钮',
          },
        ],
      },
    ],
  },
};
````

#### name

* Type: string

* Default: ''

组件库名称，建议使用中划线分割，如`demo-ui`。

#### build.css

* Type: object
* Default: { preprocessor: 'less' }

CSS 预处理器配置，目前支持`less`和`sass`两种预处理器，默认使用less。

````js
module.exports = {
  build: {
    css: {
      preprocessor: 'sass',
    },
  },
};
````

#### build.site.publicPath

* Type: string

* Default: /

等价于 vite 的 `build.outDir` 配置。

一般来说，我们的文档网站会部署在一个域名的子路径上，如 `https://my.github.io/demo-ui/`，这时候 `publicPath` 需要跟子路径保持一致，即 `/demo-ui/`。

````js
module.exports = {
  build: {
    site: {
      publicPath: '/demo-ui/',
    },
  },
};
````

#### build.srcDir

* Type: string

* Default: src

````js
module.exports = {
  build: {
    srcDir: 'myDir',
  },
};
````

#### build.namedExport

* Type: boolean

* Default: false

是否通过 Named Export 对组件进行导出。

未开启此选项时，会通过 `export default from 'xxx'` 导出组件内部的默认模块。

开启此选项后，会通过 `export * from 'xxx'` 导出组件内部的所有模块、类型定义。

#### site.title

* Type: string

* Default: ''

文档站点的标题。

#### site.logo

* Type: string

* Default: ''

文档站点的 Logo。

#### site.description

* Type: string

* Default: ''

标题下方的描述文案。

#### site.nav

* Type: object[]

* Default: undefined

文档站点的左侧导航，数组中的每个对象表示一个导航分组。

````js
module.exports = {
  site: {
    nav: [
      {
        // 分组标题
        title: '开发指南',
        // 导航项
        items: [
          {
            // 导航项路由
            path: 'home',
            // 导航项文案
            title: '介绍',
            // 是否隐藏当前页右侧的手机模拟器（默认不隐藏）
            hideSimulator: true,
          },
        ],
      },
    ],
  },
};
````

#### site.versions

* Type: object[]

* Default: undefined

文档站点多版本配置，当组件库存在多个版本的文档时，可以通过`site.versions`在顶部导航配置一个版本切换按钮。

````js
module.exports = {
  site: {
    versions: [
      {
        label: 'v1',
        link: 'https://youzan.github.io/vant/v1/',
      },
    ],
  },
};
````

#### site.baiduAnalytics

* Type: object

* Default: undefied

文档网站的百度统计配置，添加这项配置后，会自动在构建文档网站时加载百度统计的脚本。

````js
module.exports = {
  site: {
    baiduAnalytics: {
      // 打开百度统计 ->『管理』->『代码获取』
      // 找到下面这串 URL: "https://hm.baidu.com/hm.js?xxxxx"
      // 将 `xxxxx` 填写在 seed 中即可
      seed: 'xxxxx',
    },
  },
};
````

#### site.searchConfig

* Type: object

* Default: undefined

文档网站的搜索配置，基于 algolia 提供的 docsearch 服务实现。

#### site.hideSimulator

* Type: boolean

* Default: false

是否隐藏所有页面右侧的手机模拟器，默认不隐藏

#### site.simulator.url

* Type: string

* Default: -

自定义手机模拟器的 iframe URL 地址。

#### site.htmlMeta

* Type: Record<string, string>

* Default: undefined

配置 HTML 中的 meta 标签，对象的 key 为 name，value 为 content。

#### site.enableVConsole

* Type: boolean

* Default: false

是否在 dev 时开启 vConsole 调试，用于移动端 debug。

### Babel

通过根目录下的`babel.config.js`文件可以对 Babel 进行配置。

#### 默认配置

推荐使用`vant-cli`内置的 preset，配置如下：

````js
module.exports = {
  presets: ['@vant/cli/preset'],
};
````

`@vant/cli/preset`中默认包含了以下插件：

* @babel/preset-env（不含 core-js）
* @babel/preset-typescript
* @babel/plugin-transform-object-assign
* @babel/plugin-proposal-nullish-coalescing-operator
* @babel/plugin-proposal-optional-chaining
* @vue/babel-preset-jsx

### Postcss

通过根目录下的`postcss.config.js`文件可以对 Postcss 进行配置。

#### 默认配置

`vant-cli`中默认的 Postcss 配置如下：

````js
module.exports = {
  plugins: {
    autoprefixer: {},
  },
};
````

### browserslist

推荐在`package.json`文件里添加 browserslist 字段，这个值会被`@babel/preset-env`和`autoprefixer`用来确定目标浏览器的版本，保证编译后代码的兼容性。

在移动端浏览器中使用，可以添加如下配置：

````js
{
  "browserslist": ["Chrome >= 51", "iOS >= 10"]
}
````

## 目录结构

### 源代码目录

基于 Vant Cli 搭建的组件库的基本目录结构如下所示：

````
project
├─ src                # 组件源代码
│   ├─ button        # button 组件源代码
│   └─ dialog        # dialog 组件源代码
│
├─ docs               # 静态文档目录
│   ├─ home.md       # 文档首页
│   └─ changelog.md  # 更新日志
│
├─ babel.config.js    # Babel 配置文件
├─ vant.config.js     # Vant Cli 配置文件
├─ package.json
└─ README.md
````

单个组件的目录如下：

````
button
├─ demo             # 示例目录
│   └─ index.vue   # 组件示例
├─ index.vue        # 组件源码
└─ README.md        # 组件文档
````

使用 .vue 文件编写组件时，编译后会生成对应的 JS 和 CSS 文件，且 JS 文件中会自动引入 CSS 文件。

如果需要将 JS 和 CSS 解耦，实现主题定制等功能，在编写代码时就要使用独立的 JS 和 CSS 文件，如下所示：

````
button
├─ demo             # 组件示例
│   └─ index.vue   # 组件示例入口
├─ index.js         # 组件入口
├─ index.less       # 组件样式，可以为 less 或 scss
└─ README.md        # 组件文档
````

采用这种目录结构时，组件的使用者需要分别引入 JS 和 CSS 文件，也可以通过 `babel-plugin-import` 自动引入样式。

通过引入样式源文件（less 或 scss）并修改样式变量，可以实现主题定制功能。

### 构建结果目录

运行 build 命令会在 es 和 lib 目录下生成可用于生产环境的组件代码，结构如下：

````
project
├─ es                   # es 目录下的代码遵循 esmodule 规范
│   ├─ button          # button 组件编译后的代码目录
│   ├─ dialog          # dialog 组件编译后的代码目录
│   └─ index.js        # 引入所有组件的入口 (ESModule)
│
└─ lib                  # lib 目录下的代码遵循 commonjs 规范
    ├─ button           # button 组件编译后的代码目录
    ├─ dialog           # dialog 组件编译后的代码目录
    ├─ index.js         # 引入所有组件的入口
    ├─ index.less       # 所有组件未编译的样式入口
    ├─ index.css        # 打包后的组件样式，用于 CDN 引入
    ├─ [name].js        # 打包后的组件脚本，UMD 格式
    ├─ [name].es.js     # 打包后的组件脚本，ESModule 格式
    ├─ [name].min.js    # 打包和压缩后的组件脚本，UMD 格式
    └─ [name].es.min.js # 打包和压缩后的组件脚本，ESModule 格式
````

单个组件编译后的目录如下：

````
button
├─ index.js         # 组件编译后的 JS 文件
├─ index.css        # 组件编译后的 CSS 文件
├─ index.less       # 组件编译前的 CSS 文件，可以为 less 或 scss
└─ style            # 按需引入样式的入口
    ├─ index.js     # 按需引入编译后的样式
    └─ less.js      # 按需引入未编译的样式，可用于主题定制
````

#### 生成类型声明

当组件库使用 TS 编写，且根目录下存在 `tsconfig.declaration.json` 文件，Vant Cli 会自动生成 `.d.ts` 类型声明文件。

`tsconfig.declaration.json` 的参考格式如下：

````js
{
  "extends": "./tsconfig.json",
  "compilerOptions": {
    "declaration": true,
    "declarationDir": ".",
    "emitDeclarationOnly": true
  },
  "include": ["es/**/*", "lib/**/*"],
  "exclude": ["node_modules", "**/test/**/*", "**/demo/**/*"]
}
````

成功生成类型声明后，请在 `package.json` 中添加类型入口声明：

````js
{
  "typings": "lib/index.d.ts"
}
````

## 关于桌面端组件

Vant Cli 也支持预览桌面端组件，你可以在组件的 `demo` 目录下新建一个 `.vue` 文件，并在组件的 `README` 中按如下格式声明要预览的组件：

````html
<demo-code>./demo/MyDemo.vue</demo-code>
````

`demo-code` 标签中间的文本为 README 到 demo 文件的相对路径。

````
button
├─ demo             # 组件示例
│   └─ MyDemo.vue   # 要预览的 demo 文件
├─ index.js         # 组件入口
├─ index.less       # 组件样式
└─ README.md        # 组件文档
````

`demo-code` 标签支持以下属性：

|名称|类型|描述|
|-|-|-|
compact|boolean|紧凑模式
transform|boolean|防止预览区内 fixed 定位的元素飞出预览区
inline|boolean|只显示组件本身，不显示预览区边框和代码

## 去除手机模拟器

对于 PC 端的组件，如果不需要右侧的手机模拟器，可以在 `vant.config.js` 文件中设置 `site.hideSimulator` 为 `true`，这样在所有页面都会隐藏手机模拟器，也可以只针对具体页面设置。

````js
module.exports = {
  site: {
    defaultLang: 'zh-CN',
    hideSimulator: true, // 所有页面都不显示
    locales: {
      'zh-CN': {
        title: 'Vant',
        description: '轻量、可靠的移动端 Vue 组件库',
        hideSimulator: true, // 中文下所有页面都不显示
        nav: [
          {
            title: '基础组件',
            items: [
              {
                path: 'button',
                title: 'Button 按钮',
                hideSimulator: true, // 只针对某个页面不显示
              },
            ],
          },
        ],
      },
    },
  },
};
````