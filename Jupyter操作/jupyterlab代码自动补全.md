解决问题——jupyter 代码自动补全设置


在使用jupyter的时候，代码提示每次都要Tab键，感觉有点麻烦，索性就研究了一下。果然，jupyter也可以和PyCharm一样强大。

第一步：打开Anaconda Prompt，因为我创建了tensorflow2.0虚拟环境，所以一切的安装操作都在虚拟环境中执行。

第二步：依次执行下面的四行代码。（此处使用镜像安装）

```python
pip install jupyter_contrib_nbextensions 

jupyter contrib nbextension install --user

pip install jupyter_nbextensions_configurator 

jupyter nbextensions_configurator enable --user


# 解决jupyter lab 打开提示 
pip install nbconvert==5.6.1
```



安装完成之后重新打开jupyter，会看到菜单栏出现 Nbextensions。点击进入。

将Hinterland选项勾上。（因为我使用的是tf2.0,当我打开Nbextensions时，Hinterland选项已经勾上。随后又在tf1.14虚拟环境中进行安装，发现Hinterland需要手动勾选，这是两者无关紧要的区别。）

创建一个文件进行验证，成功。

哈哈 今天又get一个新技能，每天进步一点点。加油！！！
------------------------------------------------
版权声明：本文为CSDN博主「小曾同学.com」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_42182599/article/details/106191732