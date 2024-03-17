---
title: "Twikoo 评论"
date: 2024-02-02T09:57:23+08:00
draft: false
tags: ["技术"]
keywords:
- waline
---
一个简洁、安全、免费的静态网站评论系统

## 部署

[Twikoo官方文档](https://twikoo.js.org/)有详细的部署过程，本文不再重复！

## 引入Hugo

我们在Hugo根目录\layouts\partials目录下创建`comments.html`，填入下面代码并设置`serverURL`！

{{< alert icon="fire" cardColor="#e63946" iconColor="#1d3557" textColor="#f1faee" >}}
请勿在主题目录下操作，以后更新主题会重新覆盖掉此文件！
{{< /alert >}}

```html
<div id="tcomment"></div>
<script src="https://cdn.staticfile.org/twikoo/1.6.31/twikoo.all.min.js"></script>
<script>
twikoo.init({
  envId: '您的环境id', // 腾讯云环境填 envId；Vercel 环境填地址（https://xxx.vercel.app）
  el: '#tcomment', // 容器元素
  // region: 'ap-guangzhou', // 环境地域，默认为 ap-shanghai，腾讯云环境填 ap-shanghai 或 ap-guangzhou；Vercel 环境不填
  // path: location.pathname, // 用于区分不同文章的自定义 js 路径，如果您的文章路径不是 location.pathname，需传此参数
  // lang: 'zh-CN', // 用于手动设定评论区语言，支持的语言列表 https://github.com/twikoojs/twikoo/blob/main/src/client/utils/i18n/index.js
})
</script>
```

打开`config/params.toml`,在`article`中加入`showComments = true`

大功告成！
