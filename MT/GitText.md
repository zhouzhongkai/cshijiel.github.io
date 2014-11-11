# 马涛 #
					
					
1. 在本地创建git版本库
2. 在本地工作空间中修改文件后add到暂存区 然后commit到master分支上
3. 查看版本库中的状态 <br>   `git status`
4. 查看文件状态 <br> 	 `git giff`
5. 显示最近提交的文件状态	<br> `git log`		
6. 通过SSH协议连接GitHub，获取SSHKey <br>`ssh-keygen -t rsa -C"自己的邮箱地址"`<br>获取后根据提示在Users/user中找到SSHKey 在GitHub中获得相应远程版本库的认可
7. 关联本地版本库与远程版本库 <br> `git remote add origin 通常是SSH协议的版本库的地址`
8. 把本地版本库中的内容推送到远程版本库master分支上 <br>`git push -u origin master 一般只有在第一次推送的时候才使用-u连接下master分支` <br>
9. 从远程版本库中clone所有内容 <br> `git clone "远程版本库的地址"`
10. 创建分支 <br>  `git checkout -b 分支名称`
11. 查看所有分支 <br> `git branch`
12. 切换到某一分支 <br> `git checkout 分支名称`
						
						
					
