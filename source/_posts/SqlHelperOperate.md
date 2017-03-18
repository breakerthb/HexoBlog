---
title: SqlHelper.cs使用方法
tags:
  - CSharp
categories: CSharp
date: 2017-03-18 06:09:56
---

传说中，微软有一个专为.NET提供的数据库访问类。之前也见过很多版本，大多是经过网友修改过的。我在官网上貌似也没找到专门的下载链接，应该是从PetShop一类的Demo工程中得到的。于是过来发一个能找到的相对原汁原味的版本，仅供参考。
下载路径：<http://www.kuaipan.cn/file/id_61213523170038890.htm>
带中文注释的版本网上也有很多，有兴趣的可以自己下载。

下面说说如何使用这个类进行简单的增删改查。我们依然使用之前案例中创建的数据库进行实验。
<http://blog.sina.com.cn/s/blog_6849d1c201017mo7.html>

首先，我们要定义链接字符串和连接对象：

	// 连接字符串
	string strConn = "Initial Catalog='数据库名称';Server='127.0.0.1,1433';User ID='sa';Password='XXXXXX';Persist Security Info=True";
	// SQL 连接对象
	SqlConnection Connection = new SqlConnection(strConn);

这里不对连接字符串做更多解释了。

# 一，增加新纪录

	string strSQL = "insert into TSchool values('TEST','')";
        SqlHelper.ExecuteNonQuery(strConn, CommandType.Text, strSQL);

插入成功。
这里我们调用了这个接口：

	public static int ExecuteNonQuery(string connectionString, // 连接字符串
	  CommandType commandType, // 命令类型，CommandType.Text表示传入的参数是SQL语句
	  string commandText // SQL语句
	 )

返回值是被修改的记录个数。

# 二，删除记录

	string strSQL = "delete from TSchool where Id=25";
    SqlHelper.ExecuteNonQuery(strConn, CommandType.Text, strSQL);

调用同样的方法，只要会写SQL语句，完全没有难度啊。

# 三，修改记录

	string strSQL = "update TSchool set Name=123 where Id=26";
	SqlHelper.ExecuteNonQuery(strConn, CommandType.Text, strSQL);

依然只是SQL的变化。

# 四，查找记录
  
	string strSQL = "select * from TSchool";
	SqlDataReader read = SqlHelper.ExecuteReader(strConn, CommandType.Text, strSQL);

	while (read.Read())
	{
	    string str0 = read[0].ToString();
	    string str1 = read[1].ToString();
	    Console.WriteLine("{0}__{1}", str0, str1);
	}

这里调用了另外一个方法：

	public static SqlDataReader ExecuteReader(string connectionString, 
	  CommandType commandType, 
	    string commandText
	)

参数和之前的ExecuteNonQuery相同，只不过返回值是一个SqlDataReader对象，用来获得结果集。

OK，就是这么简单。如有问题，欢迎提出探讨。