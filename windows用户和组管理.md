## 查看系统中的用户

net user

## 安全标识符sid

- 查看当前用户的sid

  ```
  whoami /user
  ```

- 使用注册表查看 打开注册表命令 regedit

  HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList\

  **注意管理员账户sid是500,普通用户是1000开始**



## 自定义账户

- 账户已锁定状态:输错密码太多次,可以由管理员解开

- 创建用户的指令:

  ```
  #创建用户不指定密码
  net user zhangsan /add
  #创建用户指定明文密码(可能被监听)
  net user zhangsan p-0p-0 /add
  #创建用户指定密文密码
  net user zhangsan /add *         #*代表密码占位符
  #创建隐藏用户(net user无法看到)
  在上面的用户名后面加个$符号,例如zhangsan$
  ```

- 修改用户密码

  ```
  net user zhangsan p-0p-0p-0
  net user zhangsan *
  ```

## windows内置用户账户

1. 与使用者关联的
   - 管理员administrator:最高权限
   - 普通用户:具有一般读取权限
   - 来宾guest:权限最低,默认禁用,黑客可能利用
2. 与windows组件相关的
   - system本地系统,拥有最高权限,相当于不能登陆的root用户
   - local service本地服务,权限比普通用户组略低
   - network service网络服务,权限和普通用户一样



# windows组管理

## 用户组

一组用户的集合,组中所有用户拥有组的权限

### 命令操作

```
net localgroup group1 /add
net localgroup group1 /del
net localgroup group1 zhangsan /add   #加入用户
net localgroup group1 zhangsan /del   #删除用户
```

## 内置组账户

### 1.需要人为添加的组

- administrator:管理员组
- guest:来宾组
- power users:向下兼容的组,现在一般不用
- users:标准用户组,用户创建后默认在此组中

### 2.动态包含成员的组

- interactive:动态包含在本地登录的用户
- authenticated users:动态包含通过验证的用户
- everyone:所有人包括来宾