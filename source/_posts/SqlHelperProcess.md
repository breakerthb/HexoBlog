---
title: SqlHelper调用存储过程的方法
tags:
  - CSharp
categories: CSharp
date: 2017-03-18 06:19:06
---

ASP.NET中调用存储过程经常遇到这样那样的问题，特别是在有返回值参数的情况下。其实，并没有想象中的复杂。我们以之前博文中定义的存储过程为例。

<http://blog.sina.com.cn/s/blog_6849d1c201017mo7.html>

SqlHelper下载路径：<http://www.kuaipan.cn/file/id_61213523170038890.htm>

首先是定义链接字符串：

	// 连接字符串
	string strConn = "Initial Catalog='Students';Server='127.0.0.1,1433';User ID='sa';Password='XXXXXX';Persist Security Info=True";
	// SQL 连接对象
	SqlConnection Connection = new SqlConnection(strConn);

# 一，插入新纪录存储过程

    int ret = 0;
    SqlParameter[] arParms = new SqlParameter[2];
    
    // @Name Input Parameter
    // NChar 类型需要指定Size
    arParms[0] = new SqlParameter("@Name", SqlDbType.NChar, 50);
    arParms[0].Value = "HHEE";

    // @NewID Output Parameter 
    // 输出参数不需要赋值
    arParms[1] = new SqlParameter("@NewID", SqlDbType.Int);
    arParms[1].Direction = ParameterDirection.Output;

    SqlHelper.ExecuteNonQuery(strConn, CommandType.StoredProcedure, "proc_school_insert", arParms);

    ret = Int32.Parse(arParms[1].Value.ToString());

# 二，删除记录存储过程
    SqlHelper.ExecuteNonQuery(strConn, CommandType.StoredProcedure, "proc_school_del",
                new SqlParameter("@ID", 4));

# 三，修改记录存储过程
    SqlParameter[] para = { new SqlParameter("@ID", 5), 
                                    new SqlParameter("@Name", "1234") };
    SqlHelper.ExecuteNonQuery(strConn, CommandType.StoredProcedure, "proc_school_update",
                para);

# 四，查询存储过程
    SqlDataReader read = SqlHelper.ExecuteReader(strConn, CommandType.StoredProcedure, "proc_school_select", new SqlParameter("@ID", 5));

    while (read.Read())
    {
        string str0 = read[0].ToString();
        string str1 = read[1].ToString();
    }

PS：对于调用存储过程时参数的传递方法，其实比较灵活。有兴趣的可以自己多多实验。