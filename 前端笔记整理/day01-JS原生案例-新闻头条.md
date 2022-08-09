JS原生案例-新闻头条



## Webpack的基本认知

- 本质: 帮助开发者实现项目工程自动化
- 问题: 为什么要实现项目工成自动化
  - 因为开发版本无法满足线上版本(对于浏览器)的要求，手动实现比较繁琐
  - 因此Webpack可以帮助我们实现这些
- 工程化的项目结构
  - dist：上线版本
  - node_modules: 打包依赖模块集合
  - package-lock.json:  依赖包名称、来源
  - package.json;  打包依赖包记录管理、打包相关命令
  - src:  开发版本
  - webpack.config.js  :   webpack打包配置



## package.json配置

- 

```json
{
    "name": "news",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "webpack": "webpack",
      "dev": "webpack-dev-server"
    },
    "author": "",
    "license": "ISC",
    "devDependencies": {
      "autoprefixer": "^9.5.1",
      "babel-core": "^6.26.3",
      "babel-loader": "^7.1.5",
      "babel-plugin-transform-runtime": "^6.23.0",
      "babel-preset-latest": "^6.24.1",
      "css-loader": "^2.1.1",
      "ejs": "^3.1.5",
      "ejs-loader": "^0.3.3",
      "file-loader": "^3.0.1",
      "html-webpack-plugin": "^3.2.0",
      "node-sass": "^4.11.0",
      "postcss-loader": "^3.0.0",
      "sass-loader": "^7.1.0",
      "style-loader": "^0.23.1",
      "url-loader": "^1.1.2",
      "webpack": "^4.30.0",
      "webpack-cli": "^3.3.0",
      "webpack-dev-server": "^3.7.2"
    }
  }
```

