# ssti-server-side template injection

## 简单说,就是服务端模板注入,服务器用的模板在对用户输入过滤不到位时,使用户输入被闭合当做代码执行



## 判断模板注入的注入点:

### 找这样的场景:你输入了什么,就输出了什么

遇到这种点,基本就两个漏洞:xss或者ssti



## 判断是哪种框架

![image-20230129001550563](C:\Users\我坐摇摇车\AppData\Roaming\Typora\typora-user-images\image-20230129001550563.png)



## tornado模板ssti注入举例

例题:buuctf:easy_tornado

题解:https://www.bilibili.com/read/cv18113781/

ssti的模板注入一般使用双括号：

下面是tornado的文档节选:

    Tornado templates support control statements and expressions. Control statements are surrounded by {% %}, e.g. {% if len(items) > 2 %}. Expressions are surrounded by {{ }}, e.g. {{ items[0] }}. 

控制语句用{% %}包裹,表达式用{{ }}包裹