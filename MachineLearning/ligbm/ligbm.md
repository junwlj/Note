消除LightGBM训练过程中出现的[LightGBM] [Warning] No further splits with positive gain, best gain: -inf

ac7 2021-05-23 20:26:50  1103  收藏 2
分类专栏： 毕业设计
版权

毕业设计
专栏收录该内容
28 篇文章0 订阅
订阅专栏
1.总结一下Github上面有关这个问题的解释：
这意味着当前迭代中树的学习应该停止，因为不能再分裂了。
"min_data_in_leaf"的设定值太大导致的，应该设一个小一点的。
这不是一个bug或者error，而是一个特性，可以忽略掉(但是本人看着很不爽)。输出消息是警告用户您的参数可能错误，或者您的数据集不容易学习。
2.消除办法：
把"min_data_in_leaf"改小一点。
在参数字典中增加参数"verbose: -1"（新参数与前面的参数之间记得加逗号）


本人采用第二种办法，新的训练过程中不再出现改该警告。
参考：Suppress LightGBM Warning
------------------------------------------------
版权声明：本文为CSDN博主「ac7」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_41904729/article/details/117199788