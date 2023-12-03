---
title: "Hugo的基本使用"
date: 2023-11-28T10:48:36+08:00
draft: false
author: "则成"
weight: 2
---
基本配置

- 将draft默认配置为false

```
修改archetypes目录下的default.md文件
draft: false
```

- 修改默认的作者

```
在主题下的hugo.toml文件中修改
author = "yzchugo.toml"
```

- 在新标签页打开超链接

  可在超链接添加title标签（通过实践，好像这个"Title"不加在超链接后面也无所谓）

  ```
  [Text](https://www.gohugo.io "Title")
  ```

  增加下面模板文件 layouts/_default/_markup/render-link.html

  ```html
  <a href="{{ .Destination | safeURL }}"{{ with .Title}} title="{{ . }}"{{ end }}{{ if strings.HasPrefix .Destination "http" }} target="_blank" rel="noopener"{{ end }}>{{ .Text | safeHTML }}</a>
  ```

