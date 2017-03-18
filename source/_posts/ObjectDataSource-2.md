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
 
![ObjectDataSource数据源控件（2）自动实现](http://upload-images.jianshu.io/upload_images/145902-eae44f2c4c6bd344?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

之后点击添加按钮。

2.打开该数据集的设计界面，在工具箱中拖入TableAdapter控件

![ObjectDataSource数据源控件（2）自动实现](http://upload-images.jianshu.io/upload_images/145902-32ad87184012e6e0?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3.接着会自动弹出“TableAdapter配置向导”，选择连接字符串，点“下一步”
注意：若没有现成的数据链接供选择，可以点击“新建连接”按钮通过新建向导来创建。这里不具体介绍了。

![ObjectDataSource数据源控件（2）自动实现](http://upload-images.jianshu.io/upload_images/145902-8dea8f15681442f8?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4.选择"使用SQL语句"，点“下一步”

![ObjectDataSource数据源控件（2）自动实现](http://upload-images.jianshu.io/upload_images/145902-5e2f8db1e8dfea01?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5.点击“选择查询生成器”-> 选择表 -> 所有列(*) -> 点击“确定”

![ObjectDataSource数据源控件（2）自动实现](http://upload-images.jianshu.io/upload_images/145902-3bc4b58fe0bbbefe?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

6.点击“高级选项” -> “生成Insert，Update，Delete语句” -> “确定”

![ObjectDataSource数据源控件（2）自动实现](http://upload-images.jianshu.io/upload_images/145902-bdc0e19383c96926?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

7.完成
重新编译一下网站。

# 二，验证ObjectDataSource

1.新建网页ObjectDataSourceDemo.aspx -> 把ObjectDataSource拖入设计界面 -> 配置数据源 -> 选择业务对象（刚刚建的DataSet类） -> 下一步 -> 完成

2.拖入GridView控件 -> 选择数据源 ->ObjectDataSource1

表中的数据就能在GrideView控件中正确显示了。

![ObjectDataSource数据源控件（2）自动实现](http://s3.sinaimg.cn/mw690/6849d1c2g0a031d779442&690)

4.选择"使用SQL语句"，点“下一步”

![ObjectDataSource数据源控件（2）自动实现](http://s7.sinaimg.cn/mw690/6849d1c2gdcd1d29b78d6&690)

5.点击“选择查询生成器”-> 选择表 -> 所有列(*) -> 点击“确定”

![ObjectDataSource数据源控件（2）自动实现](http://s11.sinaimg.cn/mw690/6849d1c2gdcd1e351f53a&690)

6.点击“高级选项” -> “生成Insert，Update，Delete语句” -> “确定”

![ObjectDataSource数据源控件（2）自动实现](http://s6.sinaimg.cn/mw690/6849d1c2gdcd1ed970d85&690)

7.完成
重新编译一下网站。

# 二，验证ObjectDataSource

1.新建网页ObjectDataSourceDemo.aspx -> 把ObjectDataSource拖入设计界面 -> 配置数据源 -> 选择业务对象（刚刚建的DataSet类） -> 下一步 -> 完成

2.拖入GridView控件 -> 选择数据源 ->ObjectDataSource1

表中的数据就能在GrideView控件中正确显示了。