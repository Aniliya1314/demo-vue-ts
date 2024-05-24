# 使用vite + vue + ts + pinia + primevue 构建的vue项目

## 本地运行环境
- windows 10
- node版本 v16.13.0
- npm版本 v8.1.0

## 构建命令

### 直接构建
```shell
# https://cn.vitejs.dev/guide/
$ npm create vite@latest
```
选择使用vue以及typescript。

### 快速构建
```shell
$ npm create vite@latest <project-name> --template vue-ts
```

### 项目依赖安装与启动
```shell
$ npm install
$ npm run dev
```

## 使用ESLint约束代码风格
通过下列命令安装初始化ESlint配置
```shell
$ npx eslint --init
```
根据自身项目情况进行选择，此处选择problem、esm、vue、typescript、browser与node、JavaScript。
> 此处选择自动安装依赖时出现问题，不知为何自动安装的最新依赖中**eslint@9.3.0**版本与@typescript-eslint/eslint-plugin@^8.56.0版本出现依赖冲突。随后选择降级eslint版本为^8.56.0版本后解决该问题。

随后在**package.json scripts**中新建**lint:fix**命令检测是否运行成功。

## 使用Prettier格式化代码样式
通过下列命令安装依赖
```shell
$ npm install prettier -D
```
随后手动创建.prettierrc.cjs文件（[为什么不是js?](https://github.com/prettier/prettier/issues/12701)）,根据自身需求进行配置。

之后在**package.json scripts**中新建**lint:prettier**命令检测是否运行成功。

## 解决ESLint与Prettier冲突
通过下列命令安装依赖
```shell
$ npm install eslint-config-prettier eslint-plugin-prettier -D
```
随后在.eslintrc.cjs中extends的最后添加配置
```js
{
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:vue/vue3-essential",
    // 必须放在后面
    "plugin:prettier/recommended",
  ],
}
```

## husky与lint-staged配置
通过husky可以用来管理**git hook**,在代码上传时可执行自定义的一些命令,而list-staged是一个提交代码前只检查提交的文件的工具，通过与husky配合使用，可以实现仅对提交的文件运行脚本检测，而不是整个项目。

首先通过下列命令安装依赖
```shell
$ npm install husky lint-staged -D
```
在**package.json**中添加**prepare**钩子，用于在**npm install**之后自动启用husky。

随后在**package.json**中添加**lint-staged**配置
```json
"lint-staged":{
  "src/**/*.{vue,js,ts}":[
    "npm run lint:prettier",
    "npm run lint:fix"
  ]
}
```
安装完成后通过以下命令添加**pre-commit**钩子，并且执行上诉配置的两个脚本命令。
```shell
npx husky init
```
