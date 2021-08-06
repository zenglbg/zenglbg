---
title: vue 多页面开发构建独立项目
tags: [vue, webpack, javascript, typescript]
categories:
  - vue
comments:
  - true
keywords: zenglbg, vue, javascript, typescript, webpack
image: https://pic.zenglbg.com/images/girl/girl2/photo_2021-07-28 18.55.29.jpeg
---

# vue 多页面开发构建独立项目

## 新建项目



>  直接使用vue脚手架新建项目

```shell
vue create test
```

## 调整目录结构

将`src`中的文件移动至`src/pages/index`中

![目录接口示意图](https://pic.zenglbg.com/images/vue/2021-08-06 5.32.54.png)

## 调整webpack配置

新建配置文件`vue.config.js`

```javascript
onst path = require("path");
const os = require("os");
// 获取命令行输入的第三个参数
const projectName = process.argv[3];

// 获取src/pages中所有的项目名
const entrys = fs.readdirSync(path.resolve(__dirname, "./src/pages/")).reduce((acc, curr) => {
  acc[curr] = {
    entry: `./src/pages/${curr}/main.js`,
    template: "public/index.html",
    title: curr,
    filename: `${curr}.html`,
    chunks: ["chunk-vendors", "chunk-common", curr],
  };
  return acc;
}, {});



// 导出webpack.config.js 配置
module.exports = {
  // 配置webpack入口
  pages: projectName
    ? {
        index: {
          entry: `./src/pages/${projectName}/main.js`,
          template: "public/index.html",
          title: 'zenglbg.com--vue多页面配置',
          filename: "index.html",
          chunks: ["chunk-vendors", "chunk-common", "index"],
        },
      }
    : entrys,
  // 配置webpack出口
 	outputDir: projectName ? `dist/${projectName}` : "dist",
  productionSourceMap: false, // 生产禁止显示源代码

}
```

## 新建构建脚本

在项目目录新建`script/build.js`文件

```javascript
// script/build.js

const util = require("util");
const path = require("path");
const fs = require("fs");
const child_process = require("child_process");
const appPath = path.resolve(__dirname, "..");
const exec = util.promisify(child_process.exec);


const runBuild = function () {
  fs.readdirSync(path.join(appPath, "src/pages")).forEach(
    async (projectName) => {
      await exec(`npm run build ${projectName}`, { cwd: appPath });
    }
  );
};

runBuild();

```

## 新建构建命令

在package.json 中新建npm命令` "bd": "rm -rf dist && node script/build.js",`

![](https://pic.zenglbg.com/images/vue/2021-08-06 5.39.21.png)

