---
layout: post

title: 使用shell生成随机数-w-

date: 2016-11-30

categories: blog

tags: [shell]

description: -w-

---

虽然之前就看过一些shell的教程不过没有真正用过。。算法实验需要生成随机数的时候一直都是用Python。。今天想到了有shell这个东西。。于是上网搜了一发。。果然很方便，想用的时候随时用上就可以了-w- 希望以后不需要搜过才知道用法啦-w-

生成0~49的随机数：
```bash
for i in `seq 10`
do
    echo $(($RANDOM % 50))
done
```
看来还是要多学习一个，这么强大的工具要熟练掌握才好-w-