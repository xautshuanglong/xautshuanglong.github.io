---
title: Hexo 命令集
date: 2016-04-02 19:02:42
categories: Hexo
tags: Hexo
toc: true
---

### 常用命令：

| hexo 命令|范例|说明|
|:-----:|:-----:|:-----:|
|new "title"|hexo new "title"|创建一篇新文章|
|new draft "title"|hexo new draft "title"|创建一篇新草稿|
|publish [layout] "title"|hexo publish post "title"|发布一篇草稿 layout=post\page|


### 相关 node_modules
```bash
# 命令行工具，全局安装。
npm install hexo-cli -g

# 页面生成所需模块，安装在站点目录下(仓库根目录)
npm install hexo --save
npm install hexo-deployer-git --save
npm install hexo-deployer-heroku --save
npm install hexo-deployer-rsync --save
npm install hexo-filter-flowchart --save
npm install hexo-filter-sequence --save
npm install hexo-filter-mermaid-diagrams --save
npm install hexo-fs --save
npm install hexo-generator-archive --save
npm install hexo-generator-category --save
npm install hexo-generator-feed --save
npm install hexo-generator-index --save
npm install hexo-generator-search --save
npm install hexo-generator-sitemap --save
npm install hexo-generator-tag --save
npm install hexo-renderer-ejs --save
npm install hexo-renderer-marked --save
npm install hexo-renderer-stylus --save
npm install hexo-server --save
npm install hexo-tag-bootstrap --save
```

{% alert warning %}
如果出现不识别命令，尝试将全局路径 node_global 加入到环境变量，其中包含 hexo 和 hexo.cmd。
{% endalert %}

{% alert danger %}
hexo-filter-mermaid-diagrams markdown渲染问题
{% endalert %}
ejs 渲染时将 `-->` 替换成 `-&gt`，导致`mermaid.min.js`无法识别该语法。
将脚本语句：（node_modules\hexo-filter-mermaid-diagrams\lib\render.js line:19）
``` js
return `${start}<pre class="mermaid">${content}</pre>${end}`;
```
替换为：
``` js
return `${start}<div class="mermaid">{% raw %}${content}{% endraw %}</div>${end}`;
```

### 参考网址
[hexo常用命令笔记](https://segmentfault.com/a/1190000002632530)
[mermaidjs.github.io](https://mermaidjs.github.io)
