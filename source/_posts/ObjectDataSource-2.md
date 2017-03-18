---
title: ObjectDataSource数据源控件（2）自动实现
tags:
  - None
categories: Nome
date: 2017-03-18 06:14:10
---

ObjectDataSource可在多个页面中重复使用，这点和SqlDataSource不同。

我们可以通过VS向导自动创建访问数据库的类，这样就可以省去（1）中提到的自行定义BLL层接口的动作。

设置步骤如下：

# 一，TableAdapter控件

1.“添加新项”中选择“数据集”
 
![](http://breakerthb.github.io/PIC/2016/2c4c6bd344_1.png)

之后点击添加按钮。

2.打开该数据集的设计界面，在工具箱中拖入TableAdapter控件

![](http://breakerthb.github.io/PIC/2016/145902-2.png)

3.接着会自动弹出“TableAdapter配置向导”，选择连接字符串，点“下一步”
注意：若没有现成的数据链接供选择，可以点击“新建连接”按钮通过新建向导来创建。这里不具体介绍了。

![](http://breakerthb.github.io/PIC/2016/145902-3.png)

4.选择"使用SQL语句"，点“下一步”

![](http://breakerthb.github.io/PIC/2016/145902-4.png)

5.点击“选择查询生成器”-> 选择表 -> 所有列(*) -> 点击“确定”

![](http://breakerthb.github.io/PIC/2016/145902-5.png)

6.点击“高级选项” -> “生成Insert，Update，Delete语句” -> “确定”

![](http://breakerthb.github.io/PIC/2016/145902-6.png)

7.完成
重新编译一下网站。

# 二，验证ObjectDataSource

1.新建网页ObjectDataSourceDemo.aspx -> 把ObjectDataSource拖入设计界面 -> 配置数据源 -> 选择业务对象（刚刚建的DataSet类） -> 下一步 -> 完成

2.拖入GridView控件 -> 选择数据源 ->ObjectDataSource1

表中的数据就能在GrideView控件中正确显示了。

3.选择"使用SQL语句"，点“下一步”

4.点击“选择查询生成器”-> 选择表 -> 所有列(*) -> 点击“确定”

5.点击“高级选项” -> “生成Insert，Update，Delete语句” -> “确定”

6.完成
重新编译一下网站。

# 二，验证ObjectDataSource

1.新建网页ObjectDataSourceDemo.aspx -> 把ObjectDataSource拖入设计界面 -> 配置数据源 -> 选择业务对象（刚刚建的DataSet类） -> 下一步 -> 完成

2.拖入GridView控件 -> 选择数据源 ->ObjectDataSource1

表中的数据就能在GrideView控件中正确显示了。