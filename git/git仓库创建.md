[toc]
# github 仓库创建
本地仓库初始化
cd ~/Document/dockerfiles
git init

 

对本地仓库进行更改（在 ～/Document/dockerfiles 目录下执行)
例如，添加一个Readme文件

touch Readme
1.
对刚刚的更改进行提交
该步不可省略！

git add Readme
git commit -m 'add readme file'
1.
2.
push
首先，需要将本地仓库与github仓库关联
注： https://github.com/你的github用户名/你的github仓库.git 是github上仓库的网址

git remote add origin https://github.com/你的github用户名/你的github仓库.git
1.
然后，push，此时，可能需要输入github账号和密码，按要求输入即可

git push origin master
1.
注：有时，在执行git push origin master时，报错：error:failed to push som refs to…….，那么，可以执行

git pull origin master
1.
 

如何推送本地内容到github上已有的仓库
从github上将该仓库clone下来
git clone https://github.com/你的github用户名/github仓库名.git
1.
 
对clone下来的仓库进行更改（在仓库目录下进行）
例如，添加一个新的文件

touch Readme_new
1.
 
对刚刚的更改进行提交
该步不可省略！(其实是提交到git缓存空间)

git add Readme_new
git commit -m 'add new readme file'
1.
2.
 

push
首先，需要将本地仓库与github仓库关联
注： https://github.com/你的github用户名/你的github仓库.git 是github上仓库的网址

git remote add origin https://github.com/你的github用户名/你的github仓库.git
1.
有时，会出现fatal: remote origin already exists.，那么，需要输入git remote rm origin 解决该问题

然后，push，此时，可能需要输入github账号和密码，按要求输入即可

git push origin master
1.
注：有时，在执行git push origin master时，报错：error:failed to push som refs to…….，那么，可以执行

git pull origin master
1.
至此，github上已有的仓库的便有了更新

如果需要添加文件夹，有一点需要注意：该文件夹不能为空！否则不能成功添加

操作命令小结
克隆github上已有的仓库
git clone https://github.com/你的github用户名/github仓库名.git
1.
 

或者是在github上新建仓库并且在本地新建同名的仓库
cd ~/Document/dockerfiles
git init
1.
2.
 

对本地仓库内容进行更改（如果是多次对本地的某个仓库进行这样的操作，直接从此步开始即可，不要前面的操作了，因为本地仓库已有具有了github仓库的.git文件了）

对更改内容进行提交

git add 更改文件名或者是文件夹名或者是点"."
git commit -m "commit内容标注"
1.
2.
 

本地仓库与github仓库关联
git remote add origin https://github.com/你的github用户名/你的github仓库.git
1.
 

push
git push origin master
1.
 

注：另外可能用到的命令

git remote rm origin
git pull origin master
1.
2.
查看当前git缓存空间状态
git status
-----------------------------------
ubuntu下安装及配置git的方法（最全超详细教程github）
https://blog.51cto.com/u_15242250/2856081
