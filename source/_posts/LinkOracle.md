---
title: CSharp连接Oracle方法
tags:
  - CSharp
  - Oracle
categories: CSharp
date: 2017-03-18 06:08:28
---

首先，要在本机安装Oracle为VS2010提供的访问程序。从下面路径下载：
<http://www.oracle.com/technetwork/articles/dotnet/vs2010-oracle-dev-410461.html>

安装了Oracle Client端程序后，重启VS2010.

在VS2010中，引入Oracle访问库，如图：
![VS2010下C#连接远端Oracle数据库方法](http://i2.piimg.com/612ba597c1212226.png)

之后可以在程序中做如下测试：

	 string strConn = "Data Source=127.0.0.1/bxcorcl;User ID=examtitlebanktw;PassWord=bestabxcexamtitlebankrd02";
	
	 string strSQL = "SELECT * FROM IMAGE";
	
	 OracleConnection myConnection = new OracleConnection(strConn);
	
	 OracleCommand myORACCommand = myConnection.CreateCommand();
	
	 myORACCommand.CommandText = strSQL;
	
	 myConnection.Open();
	
	 OracleDataReader myDataReader = myORACCommand.ExecuteReader();
	
	 while (myDataReader.Read())
	 {
	       Console.WriteLine("LINE: " + myDataReader[0]);
	 }
	
	 myDataReader.Close();
	 myConnection.Close();