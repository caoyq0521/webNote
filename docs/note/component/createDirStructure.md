# createDirStructure插件

利用[Vant cli](/note/component/vantCli)构建组件库，每个组件都有若干了文件，包含：组件文件、测试文件、样式文件、组件文档、组件示例文件等，为了方便开发，提供该组件，使用该插件可以自动生成这些文件。

## 介绍

根据`组件名`自动生成如下结构，并写入`基本代码`

![](../../media/images/component1.png)

该组件接受两个参数：

* name：组件名

* path：要在哪个文件夹写生成相关文件，默认`src`文件夹下

## 使用

1、在`package.json`的`scripts`中新增脚本

````js
"create": "webpack --config webpack.create.js"
````

2、新建`webpack.create.js`文件，内容如下：

````js
const { resolve } = require("path");
const CreateDirStructure = require("./plugins/createDirStructure");

module.exports = {
  mode: "development",
  plugins: [
    new CreateDirStructure({
      name: "button",
      path: resolve(__dirname, "src")
    })
  ]
}
````

3、在`src`下新建空的index.js文件，因为webpack没有entry文件会报错；新建完后更改`name`值，运行create命令即可

## 源码

在`plugins`目录下新建`createDirStructure`目录，包含`index.js`以及`createFileContent.js`，内容如下：

### index.js

````js
const { writeFileSync, mkdirSync, statSync } = require('fs');
const createFileContent = require('./createFileContent');

class CreateDirStructure {
  constructor({ name, path = 'src' }) {
    if(!name) {
      throw new Error('name必填');
    }
    this.name = name;
    this.path = path;
    this.halfPath = `${path}/${name}`;
  }

  apply() {
    const { name, halfPath } = this;
    const { comptContent, mdContent, exportJsContent, styleContent, varContent, testSpecJsContent, demoContent } = createFileContent(name);

    try {
      var stat = statSync(halfPath);
      if(stat.isDirectory()) {
        console.log('\x1B[31m%s\x1B[0m', '文件夹已存在');
        return;
      }
    } catch (error) {
      mkdirSync(halfPath, () => {});
      writeFileSync(`${halfPath}/${name}.vue`, comptContent);
      writeFileSync(`${halfPath}/README.md`, mdContent);
      writeFileSync(`${halfPath}/index.js`, exportJsContent);
      writeFileSync(`${halfPath}/index.less`, styleContent);
      writeFileSync(`${halfPath}/var.less`, varContent);
  
      mkdirSync(`${halfPath}/test`);
      writeFileSync(`${halfPath}/test/index.spec.js`, testSpecJsContent);
  
      mkdirSync(`${halfPath}/demo`);
      writeFileSync(`${halfPath}/demo/index.vue`, demoContent);
      console.log('\x1B[34m%s\x1B[0m', '写入成功');
    }
  }
}

module.exports = CreateDirStructure;
````

### createFileContent.js

````js
const prefix = 'tm';
const upperPrefix = 'Tm';

function getUpperName(name) {
  return name.replace(/^([a-z])/, a => {
    return a.toUpperCase();
  });
}

function getCancelToLine(name) {
  return name.replace(/([A-Z])/g,"-$1").toLowerCase();
};

function getComptTemplate(upperName) {
  const str =  `///js
  import Vue from 'vue';
  import { ${upperName} } from 'tanma-design';
  
  Vue.use(${upperName});
///`

return str.replace(/\//g,'`');
}

function createComptContent(name, upperName) {
  return `<template>
  <div class="${prefix}-${name}">

  </div>
</template>

<script>
  export default {
    name: "${prefix}${upperName}"
  }
</script>`
}

function createMDContent(name, upperName) {
  const comptTemplate = getComptTemplate(upperName);
  return `# ${name}

### 介绍

组件介绍

### 引入

${comptTemplate}

## 代码演示

### 基础用法

<demo-code>./demo/index.vue</demo-code>

### Props

### Events

### Slots

### 样式变量

#### Less 变量

组件提供了下列 Less 变量，可用于自定义样式，使用方法请参考[定制主题 less](#/theme)。

名称 | 默认值 | 描述
-- | -- | --

#### Css 变量

组件提供了下列 Css 变量，可用于自定义样式，使用方法请参考[定制主题 css](#/theme2)。

名称 | 默认值 | 描述
-- | -- | --
`
}

function createStyleContent(name) {
  return `@import '../style/var';
@import './var';
  
:root {

}

.${prefix}-${name} {

}
  `
}

function createExportJsContent(name, upperName) {
  return `import ${upperName} from './${name}.vue';

${upperName}.install = function (Vue) {
  Vue.component(${upperName}.name, ${upperName});
};

export default ${upperName};
export { ${upperName} };`
}

function createVarContent() {
  return `@import '../style/var';`
}

function createTestSpecJsContent(name, upperName) {
  return `import { mount } from '@vue/test-utils';
import ${upperPrefix}${upperName} from '../${name}.vue';

describe('tm${upperName}', () => {
  it('render ${name}', () => {
    const wrapper = mount(Tm${upperName});
    expect(wrapper).toMatchSnapshot();
  });
})
  `
}

function createDemoContent(name) {
  return `<template>
  <${prefix}-${name} />
</template>`
};

function createFileContent(n) {
  const upperName = getUpperName(n);
  const name = getCancelToLine(n);
  const comptContent = createComptContent(name, upperName);
  const mdContent = createMDContent(name, upperName);
  const styleContent = createStyleContent(name);
  const exportJsContent = createExportJsContent(n,  upperName);
  const varContent = createVarContent();
  const testSpecJsContent = createTestSpecJsContent(name, upperName);
  const demoContent = createDemoContent(name);

  return {
    comptContent,
    mdContent,
    styleContent,
    exportJsContent,
    varContent,
    testSpecJsContent,
    demoContent
  }
}

module.exports = createFileContent;
````