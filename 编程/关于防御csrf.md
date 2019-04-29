---
csrf跨站请求伪造
- 盗取cookie然后伪造用户发送请求
- 直接在受害者浏览器上执行脚本或者修改dom是受害者在不知情的情况下发送get/post请求
---

# 几种防御csrf的策略
## 验证 HTTP Referer 字段（）