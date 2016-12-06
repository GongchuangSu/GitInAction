# 查看提交历史
我们可以通过 `git log` 命令来查看某个项目的提交历史。
下面我会通过一个[开源项目](https://github.com/GongchuangSu/HelloWeb)进行示例：
```
$ git log
commit f886a577726ea88afbae8556370f68ec060d6338
Author: GongchuangSu <sgc0515@gmail.com>
Date:   Mon Dec 5 20:25:07 2016 +0800

    解决用户名重复注册问题

commit 875e76e39d537ccc8bef91a07e967d8dceb5b401
Author: GongchuangSu <sgc0515@gmail.com>
Date:   Mon Dec 5 11:18:55 2016 +0800

    添加正则表达式检验用户输入注册信息

commit 254d361eb465c9675f6d46b35098ddfa28e936d2
Author: GongchuangSu <sgc0515@gmail.com>
Date:   Mon Dec 5 10:09:03 2016 +0800

    更新用户注册处理，自动添加默认角色信息

commit 3581b5e867d4e401c6d9e84c0d203039ee22c503
Author: GongchuangSu <sgc0515@gmail.com>
Date:   Sun Dec 4 22:23:11 2016 +0800

    解决数据写入数据库乱码问题
...
```
默认不用任何参数的话，`git log` 会按提交时间列出所有的更新，最近的更新排在最上面。默认情况下，会列出以下信息：
- 每次提交的 SHA-1 校验和
- 作者的名字
- 电子邮件地址
- 提交时间
- 提交说明

`git log` 有许多选项可以帮助你搜寻你所要找的提交，接下来我们介绍些最常用的。
## 常用选项**-p**
`-p`选项用来显示每次提交的内容的差异，后面可以加上`-2`选项用来显示最近两次的提交。
```
$ git log -p -2
commit f886a577726ea88afbae8556370f68ec060d6338
Author: GongchuangSu <sgc0515@gmail.com>
Date:   Mon Dec 5 20:25:07 2016 +0800

    解决用户名重复注册问题

diff --git a/src/main/java/com/gongchuangsu/helloweb/controller/LoginController.                                                                                                                java b/src/main/java/com/gongchuangsu/helloweb/controller/LoginController.java
index 5538349..a1358d7 100644
--- a/src/main/java/com/gongchuangsu/helloweb/controller/LoginController.java
+++ b/src/main/java/com/gongchuangsu/helloweb/controller/LoginController.java
@@ -1,7 +1,5 @@
 package com.gongchuangsu.helloweb.controller;

-import java.util.List;
-
 import javax.validation.Valid;

 import org.springframework.beans.factory.annotation.Autowired;
@@ -40,6 +38,8 @@ public class LoginController {
                        @Valid User user, Errors errors){
                if(errors.hasErrors())
                        return "register";
+               if(userService.userExists(user.getUsername()))^M
+                       return "redirect:/register?userExists=true";^M
                userService.addUser(user);
                return "redirect:.";
        }
...
```
## 常用选项**--stat**
如果想看到每次提交的简略的统计信息，可以使用`--stat`选项。
```
$ git log --stat -2
commit f886a577726ea88afbae8556370f68ec060d6338
Author: GongchuangSu <sgc0515@gmail.com>
Date:   Mon Dec 5 20:25:07 2016 +0800

    解决用户名重复注册问题

 .../helloweb/controller/LoginController.java          |   4 ++--
 .../java/com/gongchuangsu/helloweb/dao/UserDao.java   |   2 +-
 .../gongchuangsu/helloweb/service/IUserService.java   |   1 +
 .../gongchuangsu/helloweb/service/UserService.java    |   5 +++++
 src/main/webapp/WEB-INF/views/register.jsp            |   7 +++++++
 .../helloweb/controller/LoginController.class         | Bin 1921 -> 2083 bytes
 6 files changed, 16 insertions(+), 3 deletions(-)

commit 875e76e39d537ccc8bef91a07e967d8dceb5b401
Author: GongchuangSu <sgc0515@gmail.com>
Date:   Mon Dec 5 11:18:55 2016 +0800

    添加正则表达式检验用户输入注册信息

 src/main/java/com/gongchuangsu/helloweb/model/User.java | 9 +++++++--
 src/main/webapp/WEB-INF/views/register.jsp              | 1 +
 2 files changed, 8 insertions(+), 2 deletions(-)
```
可以看出，`--stat`选项在每次提交的下面列出了以下信息：
- 所有被修改过的文件
- 被修改过的文件有多少行被删除或添加了
- 有多少文件被修改了
- 最后还有一个总结

## 常用选项**--pretty**
这个选项可以指定使用不同于默认格式的方式展示提交历史。这个选项还有一些内建的自选项可使用。如`oneline` 将每个提交放在一行显示。
```
$ git log --pretty=oneline
f886a577726ea88afbae8556370f68ec060d6338 解决用户名重复注册问题
875e76e39d537ccc8bef91a07e967d8dceb5b401 添加正则表达式检验用户输入注册信息
254d361eb465c9675f6d46b35098ddfa28e936d2 更新用户注册处理，自动添加默认角色信息
3581b5e867d4e401c6d9e84c0d203039ee22c503 解决数据写入数据库乱码问题
2e59b4e6e1bedab65e5889c5aff38a20f79f4bd5 更新表格，从数据库读取数据并显示
89f490bcf4ca9b5a16547e24185164fa8a4d5438 添加注册页面
4ba60a0ac7a69b73b0007765dca1ad1c0448e578 集成Hibernate持久化框架
bcf801a98d29356d85cda0112cc65df4f099e93b 更新登陆界面
7c04deb2466686c6c0495f402d4989c499230c11 添加remember-me功能
c5dda853704738a5d6376655c12d5c6a01dae6bc 更新登陆验证
0207b56879aadbbb5366d3ac99f26d8b450d1ea5 添加Spring Security的JSP标签库
a20fda8dbe190bcb8c625853b2785a3362aefca9 添加Spring Security框架，用于身份验证和请求拦截
```
但最有意思的是 format，可以定制要显示的记录格式。
```
$ git log --pretty=format:"%h - %an, %ar : %s"
f886a57 - GongchuangSu, 20 hours ago : 解决用户名重复注册问题
875e76e - GongchuangSu, 29 hours ago : 添加正则表达式检验用户输入注册信息
254d361 - GongchuangSu, 30 hours ago : 更新用户注册处理，自动添加默认角色信息
3581b5e - GongchuangSu, 2 days ago : 解决数据写入数据库乱码问题
2e59b4e - GongchuangSu, 2 days ago : 更新表格，从数据库读取数据并显示
```
下表列出了常用的格式占位符写法及其代表的意义：

|选项|说明|
|:--|:--|
|%H|提交对象（commit）的完整哈希字串|
|%h|提交对象的简短哈希字串|
|%T|树对象（tree）的完整哈希字串|
|%t|树对象的简短哈希字串|
|%P|父对象（parent）的完整哈希字串|
|%p|父对象的简短哈希字串|
|%an|作者（author）的名字|
|%ae|作者的电子邮件地址|
|%ad|作者修订日期（可以用 --date= 选项定制格式）|
|%ar|作者修订日期，按多久以前的方式显示|
|%cn|提交者(committer)的名字|
|%ce|提交者的电子邮件地址|
|%cd|提交日期|
|%cr|提交日期，按多久以前的方式显示|
|%s|提交说明|

## 其它常用选项
下表列出了 `git log` 命令支持的选项

|选项|说明|
|:--|:--|
|-p|按补丁格式显示每个更新之间的差异|
|--stat|显示每次更新的文件修改统计信息|
|--shortstat|只显示 --stat 中最后的行数修改添加移除统计|
|--name-only|仅在提交信息后显示已修改的文件清单|
|--name-status|显示新增、修改、删除的文件清单|
|--abbrev-commit|仅显示 SHA-1 的前几个字符，而非所有的 40 个字符|
|--relative-date|使用较短的相对时间显示（比如，“2 weeks ago”）|
|--graph|显示 ASCII 图形表示的分支合并历史|
|--pretty|使用其他格式显示历史提交信息。可用的选项包括 oneline，short，full，fuller 和 format（后跟指定格式）|

## 限制输出长度
除了定制输出格式的选项之外，`git log` 还有许多非常实用的限制输出长度的选项，也就是只输出部分提交信息。
例如，我们要搜索出2016.12.01以来的提交历史信息，可进行如下操作：
```
$ git log --pretty="%h - %s" --since="2016-12-01"
f886a57 - 解决用户名重复注册问题
875e76e - 添加正则表达式检验用户输入注册信息
254d361 - 更新用户注册处理，自动添加默认角色信息
3581b5e - 解决数据写入数据库乱码问题
2e59b4e - 更新表格，从数据库读取数据并显示
```
还有一些其他限制`git log` 输出的选项，如下表所示：

|选项|说明|
|:--|:--|
|-(n)|仅显示最近的 n 条提交|
|--since, --after|仅显示指定时间之后的提交|
|--until, --before|仅显示指定时间之前的提交|
|--author|仅显示指定作者相关的提交|
|--committer|仅显示指定提交者相关的提交|
|--grep|仅显示含指定关键字的提交|
|-S|仅显示添加或移除了某个关键字的提交|

# 参考资料
https://git-scm.com/book/zh