title: 为yilia 添加评论系统
author: xiang
date: 2018-01-04 23:16:56
tags: livere 
---

主流blog评论系统有很多： 多说  畅言  网易云 来必力

多说：已经停止更新了，这个直接pass

畅言：souhu出品、颜值高、留言顺畅，但需要备案，有被监管的风险，个人爱自由不想被约束，所以不考虑。

网易云： 可以使用 ，但也停止更新了，技术支持无保障。

综上所叙楼主采用来必力。

1. ###### 首先在官网注册账号，得到js

2. ######  修改yiya主题下配置文件 ，开启对应字段。
  ##### livere: true
    
3. ######  在 themes/yilia/layout/_partial/post/ 中添加 livere.ejs （之前官网得到的）
```
<!-- 来必力City版安装代码 -->
<div id="lv-container" data-id="city" data-uid="youid">
	<script type="text/javascript">
   (function(d, s) {
       var j, e = d.getElementsByTagName(s)[0];

       if (typeof LivereTower === 'function') { return; }

       j = d.createElement(s);
       j.src = 'https://cdn-city.livere.com/js/embed.dist.js';
       j.async = true;

       e.parentNode.insertBefore(j, e);
   })(document, 'script');
	</script>
<noscript> 为正常使用来必力评论功能请激活JavaScript</noscript>
</div>
<!-- City版安装代码已完成 -->
```

4. ######  修改themes/yilia/layout/_partial/article.ejs 增加如下代码
```
<% if (theme.livere){ %>
<%- partial('post/livere', {
    key: post.slug,
    title: post.title,
    url: config.url+url_for(post.path)
  }) %>
<% } %>
```

5. ###### 重启hexo 服务
