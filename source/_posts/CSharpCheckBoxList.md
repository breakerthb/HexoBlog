---
title: C# CheckBoxList控件
tags:
  - CSharp
categories: CSharp
date: 2017-03-18 06:16:55
---

单个的CheckBox控件对于一两个复选框的使用是比较方便的，然而，如果想要在一个页面中排放n个复选框，而且这些复选框的内容需要从数据库中动态获取时，CheckBoxList控件就非常有用。它能够通过控件数组的方法操作一系列的复选框。

下面介绍CheckBoxList控件的一些简单用法。

# 1.与ObjectDataSource绑定

## 1）创建一个与数据库关联好的ObjectDataSource控件。如有问题请参考：
 <http://blog.sina.com.cn/s/blog_6849d1c201017ryg.html>

## 2）为CheckListBox选择新数据源

![CheckBoxList控件](http://upload-images.jianshu.io/upload_images/145902-e143c24a765906d6?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 3）选择

![CheckBoxList控件](http://upload-images.jianshu.io/upload_images/145902-0582b394e8ad890c?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

数据源选择ObjectDataSource即可。后面两项一个是显示内容，一个是值内容（Value属性的值）。这两项都要填数据表中的字段。
这样就绑定成功了。

# 2.添加项

	checkedListBox1.Items.Add("蓝色");
	checkedListBox1.Items.Add("红色");
	checkedListBox1.Items.Add("黄色");

# 3.判断第i项是否选中,选中为true,否则为false

	if（checkedListBox1.GetItemChecked(i)）
	{
	    return true;
	}
	else
	{
	    return false;
	}

# 4.设置第i项是否选中

	checkedListBox1.SetItemChecked(i, true); //true改为false为没有选中。
	checkedListBox1.SetItemCheckState(i, CheckState.Checked);

# 5.设置全选

	if(true)
	{
	    for (int j = 0; j < checkedListBox1.Items.Count; j++)
	        checkedListBox1.SetItemChecked(j, true);
	}
	else
	{
	    for (int j =0; j < checkedListBox1.Items.Count; j++)
	        checkedListBox1.SetItemChecked(j, false);
	}

# 6.得到全部选中的值 ，并将选中的项的文本组合成为一个字符串。

	string strCollected = string.Empty;
	for (int i = 0; i < checkedListBox1.Items.Count; i++)
	{
	    if (checkedListBox1.GetItemChecked(i))
	    {
	        if (strCollected == string.Empty)
	        {
	            strCollected = checkedListBox1.GetItemText(checkedListBox1.Items[i]);
	        }
	        else
	        {
	            strCollected = strCollected + "/" + checkedListBox1.GetItemText(checkedListBox1.Items[i]);
	         }
	     }
	}

# 7.清除checkedListBox中所有的选项

	for (int i = 0; i < checkedListBox1.Items.Count; i++)
	{
	    checkedListBox1.Items.Clear();
	}

# 8.设置索引为index的项为选中状态

	for (int i = 0; i < checkedListBox1.Items.Count; i++)
	{
	    checkedListBox1.SetItemChecked(i, true);
	}

# 9.选中checkedListBox1所有的选项

	for (int i = 0; i < checkedListBox1.Items.Count; i++)
	{
	    checkedListBox1.SetItemCheckState(i, CheckState.Checked);
	}

# 10.checkedListBox1中选定的项->checkedListBox2

	for (int i = 0; i < checkedListBox1.CheckedItems.Count; i++)
	{
	checkedListBox2.Items.Add(this.checkedListBox1.CheckedItems);
	
	//remove是除去一个具体的值，不是index，注意了
	checkedListBox1.Items.Remove(this.checkedListBox1.CheckedItems);
	}

PS: CheckBoxList控件中的每个CheckBox的排列方法都可以通过属性界面进行调整，请参考：
<http://msdn.microsoft.com/zh-tw/library/wffkd2c9.aspx>