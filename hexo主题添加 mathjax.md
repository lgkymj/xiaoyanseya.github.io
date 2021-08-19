---
title: hexo 主题 添加 mathjax
mathjax: true
date: 2021-01-02 17:16:47
link_c: Hexo
link_name: Mathjax
top: 
tags: Hexo
categories: Hexo
top_img: https://cdn.jsdelivr.net/gh/xiaoyanseya/picture/boke-img/bg.jpg
cover:
---


$hexo$自带的$markdown$不支持$mathjax$,因此我们要添加$mathjax$

<!-- more -->

首先，
```
npm uninstall hexo-renderer-marked
```

然后
```
npm install hexo-renderer-markdown-it
npm install hexo-renderer-kramed --save
```

最后，新建$layout/$_$markdown/mathjax.ejs:$

```ejs
<script>
    if (typeof MathJax === 'undefined') {
      window.MathJax = {
        loader: {
          source: {
            '[tex]/amsCd': '[tex]/amscd',
            '[tex]/AMScd': '[tex]/amscd'
          }
        },
        tex: {
          inlineMath: {'[+]': [['$', '$']]},
          tags: 'ams'
        },
        options: {
          renderActions: {
            findScript: [10, doc => {
              document.querySelectorAll('script[type^="math/tex"]').forEach(node => {
                const display = !!node.type.match(/; *mode=display/);
                const math = new doc.options.MathItem(node.textContent, doc.inputJax[0], display);
                const text = document.createTextNode('');
                node.parentNode.replaceChild(text, node);
                math.start = {node: text, delim: '', n: 0};
                math.end = {node: text, delim: '', n: 0};
                doc.math.push(math);
              });
            }, '', false],
            insertedScript: [200, () => {
              document.querySelectorAll('mjx-container').forEach(node => {
                let target = node.parentNode;
                if (target.nodeName.toLowerCase() === 'li') {
                  target.parentNode.classList.add('has-jax');
                }
              });
            }, '', false]
          }
        }
      };
      (function () {
        var script = document.createElement('script');
        script.src = 'https://cdn.jsdelivr.net/npm/mathjax@3.0/es5/tex-mml-chtml.js';
        script.defer = true;
        document.head.appendChild(script);
      })();
    } else {
      MathJax.startup.document.state(0);
      MathJax.texReset();
      MathJax.typeset();
    }
  </script>
```
