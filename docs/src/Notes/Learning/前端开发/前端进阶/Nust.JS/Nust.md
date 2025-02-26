---
title: "nuxt/nuxt: The Intuitive Vue Framework."
source: "https://github.com/nuxt/nuxt"
author:
  - "[[GitHub]]"
published:
created: 2024-12-24
description: "The Intuitive Vue Framework. Contribute to nuxt/nuxt development by creating an account on GitHub."
tags:
  - "clippings"
---

## Nuxt 努斯特

Nuxt is a free and open-source framework with an intuitive and extendable way to create type-safe, performant and production-grade full-stack web applications and websites with Vue.js.  
Nuxt 是一个免费的开源框架，具有直观且可扩展的方式，可通过 Vue.js 创建类型安全、高性能和生产级全栈 Web 应用程序和网站。

It provides a number of features that make it easy to build fast, SEO-friendly, and scalable web applications, including:  
它提供了许多功能，可以轻松构建快速、对 SEO 友好且可扩展的 Web 应用程序，包括：

- Server-side rendering, Static Site Generation, Hybrid Rendering and Edge-Side Rendering  
服务器端渲染、静态站点生成、混合渲染和边缘侧渲染
- Automatic routing with code-splitting and pre-fetching  
使用代码拆分和预取进行自动路由
- Data fetching and state management  
数据获取和状态管理
- SEO Optimization and Meta tags definition  
SEO 优化和元标记定义
- Auto imports of components, composables and utils  
自动导入组件、可组合项和实用程序
- TypeScript with zero configuration  
零配置的 TypeScript
- Go fullstack with our server/ directory  
使用我们的 server/ 目录进行全栈
- Extensible with [200+ modules](https://nuxt.com/modules)  
可扩展 200+ 模块
- Deployment to a variety of [hosting platforms](https://nuxt.com/deploy)  
部署到各种托管平台
- ...[and much more](https://nuxt.com/) 🚀  
...以及更多 🚀

### Table of Contents 目录

- 🚀 [Getting Started](https://github.com/nuxt/#getting-started) 🚀 开始
- 💻 [Vue Development](https://github.com/nuxt/#vue-development) 💻 Vue 开发
- 📖 [Documentation](https://github.com/nuxt/#documentation) 📖 文档
- 🧩 [Modules](https://github.com/nuxt/#modules) 🧩 模块
- ❤️ [Contribute](https://github.com/nuxt/#contribute) ❤️ 贡献
- 🏠 [Local Development](https://github.com/nuxt/#local-development) 🏠 本地开发
- ⛰️ [Nuxt 2](https://github.com/nuxt/#nuxt-2) ⛰️ 努克斯特 2
- 🛟 [Professional Support](https://github.com/nuxt/#professional-support) 🛟 专业支持
- 🔗 [Follow Us](https://github.com/nuxt/#follow-us) 🔗 关注我们
- ⚖️ [License](https://github.com/nuxt/#license) ⚖️ 许可证

---

## 🚀 Getting Started 🚀 开始

Use the following command to create a new starter project. This will create a starter project with all the necessary files and dependencies:  
使用以下命令创建新的 Starter 项目。这将创建一个包含所有必要文件和依赖项的 starter 项目：

```
npx nuxi@latest init <my-project>
```

Tip 提示

Discover also [nuxt.new](https://nuxt.new/): Open a Nuxt starter on CodeSandbox, StackBlitz or locally to get up and running in a few seconds.  
另请发现 nuxt.new：在 CodeSandbox、StackBlitz 或本地打开 Nuxt 启动器，在几秒钟内启动并运行。

## 💻 Vue Development 💻 Vue 开发

Simple, intuitive and powerful, Nuxt lets you write Vue components in a way that makes sense. Every repetitive task is automated, so you can focus on writing your full-stack Vue application with confidence.  
简单、直观且功能强大，Nuxt 让你以有意义的方式编写 Vue 组件。每个重复性任务都是自动化的，因此您可以放心地专注于编写全栈 Vue 应用程序。

Example of an `app.vue`:  
的示例 `app.vue` ：

```
<script setup lang="ts">
useSeoMeta({
  title: 'Meet Nuxt',
  description: 'The Intuitive Vue Framework.'
})
</script>

<template>
  <div id="app">
    <AppHeader />
    <NuxtPage />
    <AppFooter />
  </div>
</template>

<style scoped>
#app {
  background-color: #020420;
  color: #00DC82;
}
</style>
```

## 📖 Documentation 📖 文档

We highly recommend you take a look at the [Nuxt documentation](https://nuxt.com/docs) to level up. It’s a great resource for learning more about the framework. It covers everything from getting started to advanced topics.  
我们强烈建议您查看 Nuxt 文档以升级。这是了解有关框架的更多信息的绝佳资源。它涵盖了从入门到高级主题的所有内容。

## 🧩 Modules 🧩 模块

Discover our [list of modules](https://nuxt.com/modules) to supercharge your Nuxt project, created by the Nuxt team and community.  
发现我们由 Nuxt 团队和社区创建的模块列表，为您的 Nuxt 项目提供动力。

## ❤️ Contribute ❤️ 贡献

We invite you to contribute and help improve Nuxt 💚  
我们邀请您做出贡献并帮助改进 Nuxt 💚

Here are a few ways you can get involved:  
以下是您可以参与的几种方式：

- **Reporting Bugs:** If you come across any bugs or issues, please check out the [reporting bugs guide](https://nuxt.com/docs/community/reporting-bugs) to learn how to submit a bug report.  
报告 Bug：如果您遇到任何 Bug 或问题，请查看 报告 Bug 指南以了解如何提交 Bug 报告。
- **Suggestions:** Have ideas to enhance Nuxt? We'd love to hear them! Check out the [contribution guide](https://nuxt.com/docs/community/contribution) to share your suggestions.  
建议：有增强 Nuxt 的想法吗？我们很想听听他们怎么说！查看贡献指南以分享您的建议。
- **Questions:** If you have questions or need assistance, the [getting help guide](https://nuxt.com/docs/community/getting-help) provides resources to help you out.  
问题：如果您有任何疑问或需要帮助，获取帮助指南提供了帮助您的资源。

## 🏠 Local Development 🏠 本地开发

Follow the docs to [Set Up Your Local Development Environment](https://nuxt.com/docs/community/framework-contribution#setup) to contribute to the framework and documentation.  
按照文档设置您的本地开发环境 为框架和文档做出贡献。

## 🛟 Professional Support 🛟 专业支持

- Technical audit & consulting: [Nuxt Experts](https://nuxt.com/enterprise/support)  
技术审计和咨询：Nuxt Experts
- Custom development & more: [Nuxt Agencies Partners](https://nuxt.com/enterprise/agencies)  
定制开发及更多：Nuxt Agencies 合作伙伴



