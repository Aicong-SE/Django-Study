# 跨站请求伪造保护 CSRF

- 跨站请求伪造攻击

  - 某些恶意网站上包含链接、表单按钮或者JavaScript，它们会利用登录过的用户在浏览器中的认证信息试图在你的网站上完成某些操作，这就是跨站请求伪造(CSRF，即Cross-Site Request Forgey)。 

- 说明:

- CSRF中间件和模板标签提供对跨站请求伪造简单易用的防护。 

- 作用:  

  - 不让其它表单提交到此 Django 服务器

- 解决方案:

  1. 取消 csrf 验证(不推荐)

     - 删除 settings.py 中 MIDDLEWARE 中的 `django.middleware.csrf.CsrfViewMiddleware` 的中间件

  2. 通过验证 csrf_token 验证

     ```python
     需要在表单中增加一个标签 
     {% csrf_token %}
     ```

