# flask框架和其session漏洞

flask是一个使用 Python 编写的轻量级 Web 应用框架，其 session 存储在客户端中，也就是说其实只是将相关内容进行了加密保存到session中。和服务端的session不同，服务端的session保存在服务端中，依靠客户端cookie值中的sessionId来进行识别。本身sessionId是没有价值的，而客户端的session是可以被截取破解后得到有价值的原文
