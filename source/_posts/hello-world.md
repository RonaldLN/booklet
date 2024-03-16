---
layout: post
title: hello
# date: 2024-03-16 23:39:59
# updated: 2024-03-16 23:39:59
# cover: /images/vitepress-logo-large.webp
author:
  - ronald-luo
---

# 第一次使用vitepress搭建笔记/说明文档

## 起因

由于想在[CS61A完成作业的仓库]中把笔记放上，

并且之前又看到了不同的几个网站上都使用了很相似的网页的框架

>   -   [快速上手 | Hexo Aurora Docs (tridiamond.tech)](https://aurora.tridiamond.tech/cn/guide/getting-started.html)
>   -   [Mermaid User Guide | Mermaid](https://mermaid.js.org/intro/getting-started.html)
>   -   [COMPOSING PROGRAMS (csfive.works)](https://sicp.csfive.works/)

(由于在这些网站上都没有找到关于生成的框架的信息)于是开始搜索可能的框架，

<!-- more -->

## 发现vitepress

由于之前有同学和我提到了vue，于是先去搜索了一下vue，发现好像vue用于写界面的东西，

然后发现了vuepress这个东西，

[首页 | VuePress (vuejs.org)](https://v2.vuepress.vuejs.org/zh/)

然后看了一下官方说明文档显示的效果，感觉有点类似 但又好像不完全和上面那几个网页一样，

于是我以为是有其他的主题，于是在github上找到了hope这个主题

[主页 | vuepress-theme-hope (vuejs.press)](https://theme-hope.vuejs.press/zh/)

但尝试安装时试了好一会都没有成功能够预览，并且也似乎和那几个网站有不一样的地方(感觉不是用这个框架生成)，所以开始继续查看其他的框架。

然后在搜索时就发现了vitepress这个东西，并从某篇文章上了解到vitepress和vuepress都是出自同一个大佬之手，

于是开始查看vitepress的官网，

[VitePress | 由 Vite 和 Vue 驱动的静态站点生成器](https://vitepress.dev/zh/)

然后发现，那几个网站应该就是**用vitepress生成**的，于是就按照 [快速开始](https://vitepress.dev/zh/guide/getting-started) 尝试安装，并很容易就安装上并且能预览初始的页面了，

所以打算尝试用vitepress来搭建笔记的网站。

## 摸索基本的设置

按照 [站点配置 | VitePress](https://vitepress.dev/zh/reference/site-config) 开始进行配置，

>   如果是部署在github上，需要将 [`base`](https://vitepress.dev/zh/reference/site-config#base) 改成仓库名，如
>
>   ```ts
>   export default {
>     ...
>     base: '/CS-61A-Fall-2020/'
>   }
>   ```

然后继续按照官方文档配置了导航栏和侧边栏

### 中文显示文字设置

在配置网站搜索功能的时候，由于默认显示的文字都是英文， 想改成中文，

但是按照 [这里](https://vitepress.dev/zh/reference/default-theme-search#local-search-i18n) 的示例配置，发现**不能修改默认的显示文字**(应该是能修改多语言版本的显示效果)，然后想到了官方文档中是已经显示成中文了，所以前去官方文档的[github仓库的设置](https://github.com/vuejs/vitepress/tree/main/docs/.vitepress/config)**查看作者的现成的设置**😉，

然后，在 [`index.ts`](https://github.com/vuejs/vitepress/blob/main/docs/.vitepress/config/index.ts) 中查看，发现了**设置默认网页的显示文字需要使用 `root`** ，

在 [`zh.ts`](https://github.com/vuejs/vitepress/blob/main/docs/.vitepress/config/zh.ts) 中找到了相应的[搜索的文字设置](https://github.com/vuejs/vitepress/blob/main/docs/.vitepress/config/zh.ts#L162)，但作者用的搜索引擎不是 `local` 所以他的这些配置我不能用，但是他[其他的网页文字的配置](https://github.com/vuejs/vitepress/blob/main/docs/.vitepress/config/zh.ts#L29)感觉可以直接用，于是我就复制到了我的 `config.mts` 中，而搜索的文字就使用了官方文档中的设置，最后

```ts
export default defineConfig({
  ...
  themeConfig: {
    ...
    
    footer: {
      message: '基于 <a href="https://creativecommons.org/licenses/by-nc-sa/4.0/">CC BY-NC-SA 4.0</a> 许可发布',
      copyright: `版权所有 © 2024-${new Date().getFullYear()} <a href="https://github.com/RonaldLN">Ronald Luo</a>`
    },

    docFooter: {
      prev: '上一页',
      next: '下一页'
    },

    outline: {
      label: '页面导航'
    },

    lastUpdated: {
      text: '最后更新于',
      formatOptions: {
        dateStyle: 'short',
        timeStyle: 'medium'
      }
    },

    langMenuLabel: '多语言',
    returnToTopLabel: '回到顶部',
    sidebarMenuLabel: '菜单',
    darkModeSwitchLabel: '主题',
    lightModeSwitchTitle: '切换到浅色模式',
    darkModeSwitchTitle: '切换到深色模式',

    search: {
      provider: 'local',
      options: {
        locales: {
          root: {
            translations: {
              button: {
                buttonText: '搜索文档',
                buttonAriaLabel: '搜索文档'
              },
              modal: {
                noResultsText: '无法找到相关结果',
                resetButtonTitle: '清除查询条件',
                footer: {
                  selectText: '选择',
                  navigateText: '切换'
                }
              }
            }
          }
        }
      }
    }
  },
})
```

### 设置网页图标(摸索出了亮暗模式的设置方法)

从官方文档上了解到，

网页页面左上角的图标 logo 是用 [`logo`](https://vitepress.dev/zh/reference/default-theme-config#logo) 设置，

而网页标签上的图标，是在 [`head`](https://vitepress.dev/zh/reference/site-config#example-adding-a-favicon) 中设置，

然后下载了一个svg图标

[notebook-outline.svg](https://raw.githubusercontent.com/squidfunk/mkdocs-material/master/material/templates/.icons/material/notebook-outline.svg)

尝试按照官方文档进行设置，但是发现，由于我的浏览器设置的是暗模式，而这个图标本身也是黑色的，所以在网页上显示不太明显，

摸索尝试了一下，发现 `logo` 中有 `light` 和 `dark` 的设置，能设置不同模式下的图标，

于是开始搜索如何修改svg图片的颜色，然后在[这篇文章](https://juejin.cn/post/7298329919390269455#heading-3)中找到了修改svg颜色的方法，

**修改颜色需要在 `<path>` 中添加 `fill` 参数**，即

::: info 修改后

```xml
<svg 
    xmlns="http://www.w3.org/2000/svg" 
    viewBox="0 0 24 24"
>
    <path 
    d="M17 4v6l-2-2-2 2V4H9v16h10V4h-2M3 7V5h2V4a2 2 0 0 1 2-2h12c1.05 0 2 .95 2 2v16c0 1.05-.95 2-2 2H7c-1.05 0-2-.95-2-2v-1H3v-2h2v-4H3v-2h2V7H3m2-2v2h2V5H5m0 14h2v-2H5v2m0-6h2v-2H5v2Z" 
    fill="#ffffff" 
    />
</svg>
```

:::

::: details 修改前

```xml
<svg 
    xmlns="http://www.w3.org/2000/svg" 
    viewBox="0 0 24 24"
>
    <path 
    d="M17 4v6l-2-2-2 2V4H9v16h10V4h-2M3 7V5h2V4a2 2 0 0 1 2-2h12c1.05 0 2 .95 2 2v16c0 1.05-.95 2-2 2H7c-1.05 0-2-.95-2-2v-1H3v-2h2v-4H3v-2h2V7H3m2-2v2h2V5H5m0 14h2v-2H5v2m0-6h2v-2H5v2Z" 
    />
</svg>
```

:::

>   ```ts
>   export default defineConfig({
>     themeConfig: {
>       logo: {
>         light: '/notebook-outline.svg',
>         dark: '/notebook-outline-white.svg'
>       },
>     }
>   })
>   ```

---

而网页标签的图标，由于根据官方文档中的解释，

```ts
head: [['link', { rel: 'icon', href: '/favicon.ico' }]]
```

会被渲染成

```html
<link rel="icon" href="/favicon.ico">
```

所以我认为应该是需要添加html的参数，于是进行搜索，然后在这篇文章中找到了相应的方法

[Favicon That Works for Light and Dark Mode (joyofcode.xyz)](https://joyofcode.xyz/dark-mode-favicon#using-the-media-attribute)

>   ```html
>   <link
>     href="favicon-light.png"
>     rel="icon"
>     media="(prefers-color-scheme: light)"
>   />
>   <link
>     href="favicon-dark.png"
>     rel="icon"
>     media="(prefers-color-scheme: dark)"
>   />
>   ```

html中是需要添加 `media="(prefers-color-scheme: [light | dark])"`

那么在 `config.mts` 中就应该写成

```ts
export default defineConfig({
  ...
  head: [
    ['link', { rel: 'icon', href: '/CS-61A-Fall-2020/notebook-outline.ico', media: '(prefers-color-scheme: light)' }],
    ['link', { rel: 'icon', href: '/CS-61A-Fall-2020/notebook-outline-white.ico', media: '(prefers-color-scheme: dark)' }],
  ],
})
```

然后进行预览，发现成功能显示不同的颜色了😄

### 发现home主页 `features` 能添加链接

想在 `features` 对应的卡片上，添加能跳转的链接，于是就尝试了看看能不能添加 `link` 参数，发现居然真的可以😮

>   ```yaml
>   ---
>   features:
>     - title: Part 1
>       details: 从第一节课到 Midterm 1
>       link: /part1/lab00-lec3qa
>   ---
>   ```

### 配置mermaid

由于官方基本的设置好像并没有提供mermaid画图的支持，所以搜索了一下如何在vitepress中使用mermaid，然后找到了

[VitePress Plugin Mermaid | Mermaid support for vitepress (emersonbottero.github.io)](https://emersonbottero.github.io/vitepress-plugin-mermaid/)

但是在看这个插件的说明文档的 [Setup it up](https://emersonbottero.github.io/vitepress-plugin-mermaid/guide/getting-started.html#setup-it-up) 没看懂要如何设置，然后想到这个官方文档上就使用了mermaid，所以可以参考它的配置，

于是去它的[github仓库的配置文件](https://github.com/emersonbottero/vitepress-plugin-mermaid/blob/main/docs/.vitepress/config.js#L6)查看，然后发现，是需要将原来的

```ts
import { defineConfig } from 'vitepress'

export default defineConfig({
  ...
})
```

改成下面这样即可

```ts
import { defineConfig } from 'vitepress'
import { withMermaid } from "vitepress-plugin-mermaid"

export default withMermaid(
  defineConfig({
    ...
  })
)
```

然后预览发现能显示mermaid的图了😄

### 需要修改的格式

在预览时发现，vitepress不支持用 `==` 来显示高亮，于是搜索html的高亮方法，发现需要使用 `<mark>` 标签( `<mark>...</mark>` )

## 完成的笔记网页

-   [Notes of CS61A 20Fa (ronaldln.github.io)](https://ronaldln.github.io/CS-61A-Fall-2020/)
-   [Notes of CS61A 20Fa (gitee.io)](https://ronald-luo.gitee.io/cs-61a-fall-2020/)

## 后记

闲来无事搜索vitepress能不能写blog(本来以为不能，因为官方文档上并没有关于blog的相关设置)，然后发现了vitepress blog，

[VitePress Blog | VitePress Blog Theme](https://vitepressblog.dev/)

似乎是基于vitepress开发的一个能写blog的主题(或者说扩展模块)，

这是官方文档中的blog首页

[VitePress Blog | VitePress Blog](https://vitepressblog.dev/blog/)

![vitepress_blog_homepage](/images/vitepress_blog_homepage.png){ loading=lazy }

![vitepress_blog_postpage](/images/vitepress_blog_postpage.png){ loading=lazy }

感觉效果看起来还蛮不错，打算之后试试
