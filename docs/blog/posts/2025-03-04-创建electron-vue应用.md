---
date:
  created: 2025-03-04
  updated: 2025-03-07
authors:
  - Rexyz
categories:
  - 技术
tags:
  - 环境配置
---
  
# 创建electron-vue应用

<!-- more -->

!!! warning "注意"
    不要使用pnpm等其他包管理工具，应该使用官方文档中使用的npm，否则极有可能导致electron-squirrel-startup安装失败。

## 无法正确安装electron模块

可以直接修改Electron 和 Electron Builder 的二进制文件的镜像源

1. 打开npm配置：npm config edit
2. 在弹出的文件空白处配置Electron 和 Electron Builder 的二进制文件的镜像源

将下面的内容添加到打开的.npmrc文件末尾：

```
electron_mirror=https://cdn.npmmirror.com/binaries/electron/
electron_builder_binaries_mirror=https://npmmirror.com/mirrors/electron-builder-binaries/
```

## 安装Vue CLI 脚手架

用淘宝镜像源cnpm安装Vue CLI脚手架：

```powershell
npm install -g cnpm --registry=https://registry.npmmirror.com  
cnpm install -g @vue/cli
```

## 创建 Vue 项目

```powershell
vue create my-project
```

## Electron 依赖安装

```powershell
cd my-electron-app
npm install electron electron-builder --save-dev
```

推荐同时安装 `vue-cli-plugin-electron-builder` 插件实现更深度集成：

```powershell
vue add electron-builder
```

此时如果在babel.config.js文件中module报错，是因为当前VSCode打开的并非Vue项目根目录，需要进入刚刚创建的新文件夹下。

如果更改工作目录后依然报错，尝试运行

```powershell
npm install @vue/cli-plugin-babel @babel/preset-env --save-dev
```

## 开发与调试流程

### 启动开发模式

```powershell
npm run electron:serve  # 自动启动 Vue 开发服务器 + Electron 窗口
```

### 生产环境构建

打包配置优化:更改vue.config.js文件，添加以下内容：

```javascript
module.exports = {
  pluginOptions: {
    electronBuilder: {
      nodeIntegration: true,
      builderOptions: {
        appId: 'com.example.app',
        win: { target: 'nsis' },
        mac: { category: 'public.app-category.utilities' }
      }
    }
  }
}
```

### 执行构建命令

```powershell
npm run electron:build
```

生成的构建文件在dist_electron目录下的win-unpacked文件夹下，可以直接运行。
