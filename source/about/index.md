---
title: 关于我
date: {{date}}
type: about 
layout: about
comments: true
---

南京大学准研究生
qq: 1950495382
email: beiwang121@163.com

## 个人简历（待更新）

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gf7yonfc6uj30iy0oj45s.jpg)

欢迎推荐工作/实习机会

<!-- <div class="comments" id="comments">
                                
  <script defer src="https://utteranc.es/client.js" repo="wangbei98/hexo-comments" issue-term="pathname" theme="github-light" crossorigin="anonymous">
  </script>

</div> -->

<!-- Comments -->

<!-- <div class="comments" id="comments">         
  <div id="gitalk-container"></div>
  <link rel="stylesheet" href="https://unpkg.com/gitalk/dist/gitalk.css">
  <script defer src="https://unpkg.com/gitalk/dist/gitalk.min.js"></script>
  <script defer src="https://cdn.jsdelivr.net/npm/blueimp-md5@2.11.1/js/md5.min.js"></script>
  <script type="text/javascript">
    var oldLoad = window.onload;
    window.onload = function () {
      var gitalk = new Gitalk({
        clientID: 'fadee00c47c3179316e9',
        clientSecret: 'dd2e0d89ce4037c84b48f6a15e748a6ecadb10f3',
        repo: 'https://github.com/wangbei98/hexo-comments',
        owner: 'wangbei98',
        admin: 'wangbei98',
        id: md5(location.pathname),
        language: 'zh-CN',
        perPage: 15,
        pagerDirection: 'last',
        createIssueManually: 'false',
        distractionFreeMode: 'false'
      });

      gitalk.render('gitalk-container')
      oldLoad && oldLoad();
    }
  </script>
</div> -->


<div class="comments" id="comments">
  <div id="vcomments"></div>
  <script defer src="//cdn1.lncld.net/static/js/3.0.4/av-min.js"></script>
  <script defer src="//unpkg.com/valine/dist/Valine.min.js"></script>

  <script type="text/javascript">
    var notify = 'false' === 'true';
    var verify = 'false' === 'true';
    var oldLoad = window.onload;
    window.onload = function () {
      new Valine({
        el: '#vcomments',
        notify: notify,
        verify: verify,
        app_id: "YY2AwNypNnAzBznWy6LlhIS2-gzGzoHsz",
        app_key: "9C5ifON1dTJQ1Yp4jw4QrYVJ",
        placeholder: "说点什么",
        avatar: "/retro",
        meta: ['nick', 'mail', 'link'],
        pageSize: "10",
      });
      oldLoad && oldLoad();
    };
  </script>
  <noscript>Please enable JavaScript to view the <a href="https://valine.js.org" rel="nofollow noopener">comments
      powered by Valine.</a></noscript>
</div>