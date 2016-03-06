# 前端性能优化--yahoo前端性能团队总结的35条黄金定律

标签（空格分隔）： 其他

---

<h3>网页内容</h3>
    1. 减少http请求次数
    2. 减少DNS查询次数
    3. 避免页面跳转
    4. 缓存Ajax
    5. 延迟加载
    6. 提前加载
    7. 减少DOM元素数量
    8. 根据域名划分内容
    9. 减少iframe数量
    10. 避免404
<h3>服务器</h3>
    11. 使用CDN
    12. 添加Expires和Cache-Control报文头
    13. Gzip压缩传输文件
    14. 配置ETags
    15. 尽早flush输出
    16. 使用GET Ajax请求
    17. 避免空的图片src
<h3>Cookie</h3>
    18. 减少Cookie大小
    19. 页面内容使用无cookie域名
<h3>CSS</h3>
    20. 将样式表置顶
    21. 避免CSS表达式
    22. 用<link>代替@import
    23. 避免使用Filters
<h3>Javascript</h3>
    24. 将脚本置底
    25. 使用外部Javascript和CSS文件
    26. 精简Javascript和css
    27. 去除重复脚本
    28. 减少DOM访问
    29. 使用智能事件处理
<h3>图片</h3>
    30. 优化图像
    31. 优化 CSS Sprite
    32. 不要再HTML中缩放图片
    33. 使用小且可缓存的favicon.ico
<h3>移动客户端</h3>
    34. 保持单个内容小于25KB
    35. 打包组建成复合文档

对每个性能优化的详细解释：http://www.cnblogs.com/lei2007/archive/2013/08/16/3262897.html
