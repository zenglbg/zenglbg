---
title: vue中使用mockjs最完美方案
tags: [vue, webpack, javascript, node, mockjs]
categories:
  - vue
comments:
  - true
keywords: zenglbg, vue,  webpack, javascript, node, mockjs
image: https://pic.zenglbg.com/images/girl/girl4/photo_2021-08-11 11.05.45.jpeg
---


# vue中使用mockjs最完美方案

我们开发常遇到后端接口没有开发完成，单给出了接口文档这种情况。我们可以使用mockjs自己启动服务完成接口对接。

常用mock资源

- [中文官网示例](http://mockjs.com/examples.html)
- [官方代码github](https://github.com/nuysoft/Mock/tree/refactoring)

## 第一步 安装依赖

安装mockjs制造数据。body-parser 处理post请求。

```shell
yarn add -D body-parser mockjs chokidar chalk


# npm i -D body-parser mockjs
```



## 第二步 新建mockjs-server

```javascript
// mock/mockjs-server

module.exports = (app) => {
  console.log('打印express实例', app)
}

```



## 第三步 使用mockjs-server

```javascript
// vue.config.js
// 在vue.config.js中配置node服务devServer启动前执行的中间件

module.exports = {
    devServer:{
          before: require("./mock/mock-server.js"),
    } 
}

```

## 第四步 完善mockjs-server

1. 对post请求的请求体进行解析
2. 注册mockjs请求api地址
3. 使用chokidar对mock目录进行文件变化监控

```javascript
// mock/mockjs-server
const chokidar = require("chokidar");
const bodyParser = require("body-parser");
const chalk = require("chalk");
const path = require("path");
const Mock = require("mockjs");

const mockDir = path.join(process.cwd(), "mock");

// 注册mockjs请求api地址的函数
function registerRoutes(app) {
  let mockLastIndex;
  const {mocks} = require('./index.js');
  const mocksForServer = mocks.map(route => {
    // 适配器-对mocks接口数据进行适配
    return responseFake(route.url, route.type, route.response);
  })
  for (const mock of mocksForServer) {
    app[mock.type](mock.url, mock.response);
    mockLastIndex = app._router.stack.length;
  }
  const mockRoutesLength = Object.keys(mocksForServer).length;
  return {
    mockRoutesLength: mockRoutesLength,
    mockStartIndex: mockLastIndex - mockRoutesLength,
  };
}

function unregisterRoutes() {
  Object.keys(require.cache).forEach(i => {
    if(i.includes(mockDir)) {
      delete require.cache[require.resolve(i)]
    }
  })
}

function responseFake(url, type, respond) {
  return {
    // 添加环境变量——请求的url
    url: new RegExp(`${process.env.VUE_APP_BASE_API}${url}`),
    type: type || 'get',
    response(req, res) {
      console.log("request invoke:" + req.path);
      res.json(Mock.mock(respond instanceof Function ? respond(req, res) : respond));
    }
  }
  
}

module.exports = (app) => {
  console.log('打印express实例', app)
  
  // 1. 
  app.use(bodyParser.json());
  app.use(
    bodyParser.urlencoded({
      extended: true,
    })
  );
  
  // 2. 
  const mockRoutes = registerRoutes(app);
  var mockRoutesLength = mockRoutes.mockRoutesLength;
  var mockStartIndex = mockRoutes.mockStartIndex;

  // 3. 
  chokidar
  	.watch(mockDir, {
    	ignored: /mockjs-server/,
    	ignoreInitial: true,
  }).on('all', (event, path) => {
    if (event === "change" || event === "add") {
       try {
          // 请求mock路由栈
          app._router.stack.splice(mockStartIndex, mockRoutesLength);

          // 清除路由缓存
          unregisterRoutes();

          const mockRoutes = registerRoutes(app);
          mockRoutesLength = mockRoutes.mockRoutesLength;
          mockStartIndex = mockRoutes.mockStartIndex;

          console.log(chalk.magentaBright(`\n > Mock Server hot reload success! changed  ${path}`));
        } catch (error) {
          console.log(chalk.redBright(error));
        }Ï
      
    }
    
  })
}

```

## 第五步 添加mock

1. 添加user.js 制造user数据
2. 添加mock入口index

```javascript
// mock/modules/user.js
const Mock = require("mockjs");

module.exports = [
  {
    url: "/user/login",
    type: 'post',
    response: (config) => {
      const {username} = config.body;
      if(username === admin) {
        return {
          code: 200,
          data: 'token'
        }
      }else {
        return {
          code: 400,
          message: "账户或密码不正确！"
        }
      }
    }
  },
  {
    url: "/user/list",
    type: 'post',
    response: (config) => {
      return {
        code: 200,
        data: mock({
          "list|10": [
            {
              "id|+1": 1,
              username: "@string",
              address: "@string",
              age: "@natural(100)",
            },
          ],
        })
      }
    }
  }
]

const Mock = require("mockjs");

```
- 添加mock入口

```javascript
// mock/modules/index.js

const Mock = require("mockjs");
const fs = require("fs");
const path = require("path");


function param2Obj(url) {
  const search = decodeURIComponent(url.split('?')[1]).replace(/\+/g, ' ')
  if (!search) {
    return {}
  }
  const obj = {}
  const searchArr = search.split('&')
  searchArr.forEach(v => {
    const index = v.indexOf('=')
    if (index !== -1) {
      const name = v.substring(0, index)
      const val = v.substring(index + 1, v.length)
      obj[name] = val
    }
  })
  return obj
}



// 自动注册所有在modules中的mock文件
function getMocks() {
  const urls = fs.readdirSync(path.resolve(__dirname, "./modules")).reduce((acc, curr) => {
    const data = require(path.resolve(__dirname, `./modules/${curr}`));
    acc = acc.concat(data);

    return acc;
  }, []);
}

const mocks = getMocks();


function mockXHR() {
  // mock patch
  // https://github.com/nuysoft/Mock/issues/300
  Mock.XHR.prototype.proxy_send = Mock.XHR.prototype.send;
  Mock.XHR.prototype.send = function () {
    if (this.custom.xhr) {
      this.custom.xhr.withCredentials = this.withCredentials || false;

      if (this.responseType) {
        this.custom.xhr.responseType = this.responseType;
      }
    }
    this.proxy_send(...arguments);
  };

  function XHR2ExpressReqWrap(respond) {
    return function (options) {
      let result = null;
      if (respond instanceof Function) {
        const { body, type, url } = options;
        // https://expressjs.com/en/4x/api.html#req
        result = respond({
          method: type,
          body: JSON.parse(body),
          query: param2Obj(url),
        });
      } else {
        result = respond;
      }
      return Mock.mock(result);
    };
  }

  for (const i of mocks) {
    Mock.mock(new RegExp(i.url), i.type || "get", XHR2ExpressReqWrap(i.response));
  }
}

module.exports = {
  mocks,
  mockXHR,
};


```

## 第六步添加环境变量文件.env.development

> 我们在mockjs-server使用了环境变量`VUE_APP_BASE_API`。通过在vue项目根目录中新建.env.development文件，vue将自动注入该环境变量。

```shel
## .env.development

NODE_ENV=development

# base api
VUE_APP_BASE_API=/dev-api
```



