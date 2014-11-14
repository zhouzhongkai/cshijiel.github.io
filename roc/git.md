# Git学习
**陈鹏** 2014年11月7日 17:41:03 [返回首页][1]

---

## 整个过程

1. 在本地创建git版本库
2. 在github上创建空库，类似于username.github.io
3. 在本地创建ssh-keygen，如下:
    ssh-keygen -t rsa -C "user.email"
4. 得到ssh-key后填入github.com,
5. 验证:
    ssh -T git@github.com
6. 在本地执行
    git clone ...
克隆整个项目
7. 在本地编辑修改/增加 index.html文件，
8. 提交
    git push origin master
9. 以后更新先使用
    git fetch origin master
    git pull origin master
10. 然后本地编辑修改提交后再push到远程库

  [1]: http://cshijiel.github.io