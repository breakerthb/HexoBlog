---
title: C# RadioButtonList控件（在线投票功能）
tags:
  - CSharp
categories: CSharp
date: 2017-03-18 06:17:53
---

# 涉及技术点：

- RadioButtonList控件的使用
- 三层架构
- ObjectDataSource绑定控件
- 触发器的使用

RadioButtonList实际上是RadioButton控件的一个容器，能够通过一个对象处理N个单选控件。
我们要建立一个能够随时更改投票内容的动态投票系统。维护人员能够通过简单的数据库修改发起一个新投票。在这个例子中，我们会有两个投票项目，可以动态切换。

# 一，建立数据库

	create table TVote
	(
		Id int NOT NULL primary key,
		Title nchar(100) NULL, 
		TotleCnt int NULL, 
	);
	
	insert into TVote values(1,'今天下班去哪儿吃饭?', 0);
	insert into TVote values(2,'先进员工评选', 0);
	
	create table TVoteDetail
	(
		Id int IDENTITY(1,1) NOT NULL primary key, 
		Detail nchar(100) NULL, 
		VoteId int references TVote(Id), 
		Cnt int NULL 
	);
	
	insert into TVoteDetail values('食堂',1,0);
	insert into TVoteDetail values('小吃城',1,0);
	insert into TVoteDetail values('快餐店',1,0);
	insert into TVoteDetail values('大排档',1,0);
	insert into TVoteDetail values('张三',2,0);
	insert into TVoteDetail values('李四',2,0);
	insert into TVoteDetail values('王五',2,0);
	insert into TVoteDetail values('赵六',2,0);

我们总共建立了两张表，TVote作为主表，保存每个投票项目，TVoteDetail表保存投票的具体内容。
每当TVoteDetail中的得票数修改，TVote表中的全部票数也要跟着修改。这里用触发器来实现。脚本如下：

	if exists(select name from sysobjects where name='TVoteDetail_Update' and type='tr')
		drop trigger TVoteDetail_Update
		go
		create trigger TVoteDetail_Update
		on TVoteDetail
		for UPDATE
		as
		if(update(Cnt))
		begin
		declare @voteid int 
		declare @totalcnt int
		select @voteid = VoteId from Inserted
		
		select @totalcnt = sum(cnt) from TVoteDetail 
		where VoteId = @voteid
		group by VoteId
		update TVote set TotleCnt=@totalcnt where Id=@voteid
	end

这样数据库部分完成。

# 二，DAL层代码

    ///
    /// 得到投票标题
    ///
    /// 投票项ID
    /// 投票项标题
    public SqlDataReader GetVoteTitle(int nVoteID)
    {
        string strConn = SqlHelper.GetConnSting();
        string strSQL = "select Title from TVote where Id=" + nVoteID.ToString();
        return SqlHelper.ExecuteReader(strConn, CommandType.Text, strSQL);
    }

    ///
    /// 得票数加1
    ///
    /// 投票项目ID
    public void UpdateVoteCnt(int nID)
    {
        string strConn = SqlHelper.GetConnSting();
        string strSQL = "update TVoteDetail set Cnt=Cnt+1 where Id=" + nID;
        SqlHelper.ExecuteNonQuery(strConn, CommandType.Text, strSQL);
    }

SQLHelper类的使用前面的本文已经介绍了，这里不再赘述。

# 三，BLL代码

    [DataObjectMethod(DataObjectMethodType.Select)]
    public static SqlDataReader GetVoteDetail(int nVoteID)
    {
        DAL da = new DAL();
        return da.GetVoteDetail(nVoteID);
    }
    public static void UpdateVoteCnt(int nID)
    {
        DAL da = new DAL();
        da.UpdateVoteCnt(nID);
    }

# 四，UI层

## 1.在APSX页面中添加如下代码

	<html xmlns="http://www.w3.org/1999/xhtml">
	<head runat="server">
	    <title>投票系统</title>   
	    <style type="text/css">
	        .style1
	        {
	            width: 500px;
	            margin:auto;
	            border-style:solid;
	            border-color:Black;
	            border-width:1px;
	        }
	        .style2
	        {
	            height: 35px;
	            background-color:#0066CC;
	            color:White;
	        }
	    </style>
	</head>
	<body>
	    <form id="form1" runat="server">
	    <div style="width:100%; text-align:center;">
	        <table class="style1">
	            <tr>
	                <td class="style2">
	                    <strong>投票系统</strong></td>
	            </tr>
	            <tr>
	                <td>
	                    <asp:Label ID="lblTitle" runat="server" Text="Label"></asp:Label>
	                    <asp:RadioButtonList ID="rblVote" runat="server" 
	                        DataSourceID="ObjectDataSource1" DataTextField="Detail" DataValueField="Id">
	                    </asp:RadioButtonList>
	                </td>
	            </tr>
	            <tr>
	                <td class="style2">
	                    <asp:Button ID="btnSubmit" runat="server" onclick="btnSubmit_Click" Text="提交" />
	&nbsp;<asp:Button ID="btnResult" runat="server" onclick="btnResult_Click" Text="结果" />
	                </td>
	            </tr>
	        </table>
	        <asp:ObjectDataSource ID="ObjectDataSource1" runat="server" 
	            DeleteMethod="Delete" InsertMethod="Add" 
	            onselecting="ObjectDataSource1_Selecting" SelectMethod="GetVoteDetail" 
	            TypeName="BLL" UpdateMethod="Update" 
	            OldValuesParameterFormatString="original_{0}">
	            <DeleteParameters>
	                <asp:Parameter Name="nFavorID" Type="Int32" />
	            </DeleteParameters>
	            <InsertParameters>
	                <asp:Parameter Name="strFavor" Type="String" />
	            </InsertParameters>
	            <SelectParameters>
	                <asp:Parameter Name="nVoteID" Type="Int32" />
	            </SelectParameters>
	            <UpdateParameters>
	                <asp:Parameter Name="nFavorID" Type="Int32" />
	            </UpdateParameters>
	        </asp:ObjectDataSource>
	    </div>
	    </form>
	</body>
	</html>

## 2.后台代码

	public const int nVoteID = 2; // 设置投票ID
	
    protected void Page_Load(object sender, EventArgs e)
    {
        lblTitle.Text = BLL.GetVoteTitle(nVoteID);
    }

    protected void btnSubmit_Click1(object sender, EventArgs e)
    {
        int nID = Convert.ToInt32(rblVote.SelectedValue);

        BLL.UpdateVoteCnt(nID);
    }
    protected void btnLook_Click(object sender, EventArgs e)
    {
        
    }
    protected void ObjectDataSource1_Selecting(object sender, ObjectDataSourceSelectingEventArgs e)
    {
		// ObjectDataSource 指定的是带有参数的select方法
        e.InputParameters["nVoteID"] = nVoteID;  
    }

注意，ObjectDataSource控件需要指定BLL中的GetVoteDetail方法作为Select方法。
选择了需要投票的项目之后，提交。数据库中相应的项目会改变。自己试试吧。