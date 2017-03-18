---
title: ObjectDataSource数据源控件（1）手动实现
tags:
  - CSharp
categories: CSharp
date: 2017-03-18 06:13:13
---

ASP.NET中，数据源控件是所有显示控件和数据库绑定的媒介，因此相当重要。数据源控件实际上是帮助绑定动作完成数据库的增、删、改、查功能。
与SqlDataSource相比，ObjectDataSource更适用于三层架构（UI、BLL、DAL）的开发。用户可以自行定义访问数据库接口。
ObjectDataSource控件实际上是通过一个BLL层类完成对某一个特定数据库的操作。

# 1.建立三层架构
这里，我们用DAL和BLL两个类来代表这两个层。至于UI层，自然是ASPX页面了。

# 2.BLL层
	public class BLL{
	 [DataObjectMethod(DataObjectMethodType.Insert)]
	    public static int Add(string strInfo)
	    {
	      DAL da = new DAL();
	      return da.Add(strInfo);
	    }
	
	    [DataObjectMethod(DataObjectMethodType.Delete)]
	    public static void Delete(int nID)
	    {
	      DAL da = new DAL();
	      da.Delete(nID)
	    }
	
	    [DataObjectMethod(DataObjectMethodType.Update)]
	    public static void Update(int nID, string strInfo)
	    {
	      DAL da = new DAL();
	      da.Update(nID, strInfo);
	    }
	
	    [DataObjectMethod(DataObjectMethodType.Select)]
	    public static SqlDataReader GetAll()
	    {
	        DAL da = new DAL();
	        return da.GetAll();
	    }
	}

注意：中括号里的语句用了声明这四个函数的意义，方便后面绑定控件时指定相关方法。

# 3.DAL层
	public class DAL{
	    public int Add(string strInfo)
	    {
	      // 连接Database, 调用Insert语句
	    }
	
	    public void Delete(int nID)
	    {
	      // 连接Database, 调用Delete语句
	    }
	
	    public void Update(int nID, string strInfo)
	    {
	      // 连接Database, 调用update语句
	    }
	
	    public SqlDataReader GetAll()
	    {
	      // 连接Database, 调用select语句
	    }
	}

# 4.绑定ObjectDataSource控件

## 1) 从工具箱中拖拽一个ObjectDataSource控件到需要的ASPX页面中

## 2)配置数据源

![ObjectDataSource数据源控件](http://upload-images.jianshu.io/upload_images/145902-19523683853a78ca?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 3）选择业务对象：BLL类

![ObjectDataSource数据源控件](http://upload-images.jianshu.io/upload_images/145902-747944ec56309e7d?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

下一步

## 4）定义数据方法

![ObjectDataSource数据源控件](http://upload-images.jianshu.io/upload_images/145902-5023cad227dd544a?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

分别给增删改查方法指定BLL类中的方法。

点击“完成”。这样一个ObjectDataSource控件就定义完成了，之后可以使用它为其他控件提供数据服务。
