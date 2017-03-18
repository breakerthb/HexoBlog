---
title: ListBox&DropDownList控件（省、市选择）
tags:
  - CSharp
categories: CSharp
date: 2017-03-18 06:15:31
---

# 涉及技术点：

- 1. ListBox控件的用法
- 2. DropDownList控件的用法
- 3. 控件联动
- 4. ObjectDataSource绑定控件后的数据刷新
- 5. 给Select方法带参数的ObjectDataSource控件传参


在信息填写的表单中，常会需要填写省市之类的信息，如果能够选取会比较方便。这里就涉及一个控件联动的问题。下面我们就具体了解一下如何利用前面提到的技术设计这么一个从后台管理到前台应用的程序。

# 一，数据库设计

需要两张表，一张保存省的名称，另一张保存市的名称。两张表是1：n的级联关系。下面是创建数据库脚本：

      create table TProvince
      (
        Id int primary key identity,
        Name varchar(50) not null
      )
	
      create table TCity
      (
        Id int primary key identity,
        Name varchar(50) not null,
        ProvinceId int references TProvince(Id)
      )

在表中添加一条基本信息，用来初始显示。

	insert into TProvince values('北京')
	insert into TCity values('北京', 1)

这样，数据库就建立好了。

# 二，新建数据集

我们这次完全使用控件绑定来完成数据库的访问，以数据集为基础。
1.新建一个数据集（DataSet）,命名为DSProvinceCity.xsd
2.从工具箱中添加两个TableAdapter，分别与 TProvince 和 TCity 表关联。具体方法之前的日志已经讲过。添加后，会生成两个DataTable对象（TProvice和TCity）还有两个TableAdapter对象(TProvinceTableAdapter和TCityAdaper)。如图所示。

![ListBox&DropDownList控件（省、市选择）](http://upload-images.jianshu.io/upload_images/145902-195631dbbbafbbe3?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 三，UI设计

页面分为两部分，后台管理部分和前台应用部分。
后台管理部分：用于帮助管理员上传各个省、市名称和对应关系的数据
前台应用部分：用户上传数据时使用

# 四，后台管理部分（ListBox联动）

1.在页面中添加两个ListBox控件，分别命名为lbProvince和lbCity
2.添加一个ObjectDataSource控件，命名为ODSProvince，配置数据源
3.选择业务对象

![ListBox&DropDownList控件（省、市选择）](http://upload-images.jianshu.io/upload_images/145902-699fad3cf510beb0?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4.重复2、3添加另一个ObjectDataSource控件（ODSCity）。并配置数据源
5.为lbProvince选择数据源

![ListBox&DropDownList控件（省、市选择）](http://upload-images.jianshu.io/upload_images/145902-d44522b13fcded85?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

6.为lbCity选择数据源

![http://s9.sinaimg.cn/large/6849d1c2g0a4382bbc698&690](http://upload-images.jianshu.io/upload_images/145902-4efed013f80c2c11?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

运行一下看看是否能够正常显示数据。下面我们看看如何设计一个添加“省”的功能。
7.在页面上添加一个TextBox控件，命名为txtProvinceAdd。再添加一个Button，命名为btnProvinceAdd。并添加如下代码：

	    protected void btnProvinceAdd_Click(object sender, EventArgs e)
	    {
	        TProvinceTableAdapter province = new TProvinceTableAdapter();
	        province.Insert(txtProvinceAdd.Text.ToString().Trim());
	        txtProvinceAdd.Text = "";
	        lbProvince.DataBind(); // 重新绑定数据源
	    }

给没有引用的命名空间加入using语句，这里可以用VS中的自动添加功能。这样我们就能通过TProvinceTableAdapter 提供的方法将新的省名写进数据库了。
lbProvince.DataBind();
这句话很重要，如果没有它，添加了省名的lbProvince控件不会有任何变化。重新绑定后能实现数据的刷新。
运行一下程序，添加几个省。

8.重复7的动作，添加txtCityAdd和btnCityAdd。并添加如下代码：

    protected void btnCityAdd_Click(object sender, EventArgs e)
    {
        TCityTableAdapter city = new TCityTableAdapter();
        city.Insert(txtCityAdd.Text.ToString().Trim(), Convert.ToInt16(lbProvince.SelectedValue));
        txtCityAdd.Text = "";
        lbCity.DataBind();
    }

由于市名需要和省名进行关联，因此我们在添加市之前要先选择一个省名，lbProvince控件获得焦点后，lbProvince.SelectedValue语句能够得到选中省名的ID号。
为两个ListBox添加初始焦点位置，代码如下：

    protected void Page_Load(object sender, EventArgs e)
    {
        if (!IsPostBack) // 只有第一次加载时付初值
        {
            lbProvince.SelectedIndex = 0;
            lbCity.SelectedIndex = 0;
        }
    }

运行一下程序，给某个省添加几个市吧。
貌似有点问题，好像添加之后省和市并不对应。那是因为两个控件的联动没有做好。

9.将lbProvince控件的AutoPostBack属性设为True,并添加点击事件。代码如下：
  
    protected void lbProvince_SelectedIndexChanged(object sender, EventArgs e)
    {
        lbCity.DataBind();
    }

10.修改TCityTableAdapter的Select方法

![ListBox&DropDownList控件（省、市选择）](http://upload-images.jianshu.io/upload_images/145902-2e150dd4f5687713?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在DataSet数据集界面中，选择TCityTableAdapter,在它的SelectCommand子属性中，选择CommandText。弹出查询生成器。

![ListBox&DropDownList控件（省、市选择）](http://upload-images.jianshu.io/upload_images/145902-9b4652f912fbefb6?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在SQL语句中添加“where ProvinceId=@ProvinceId”语句。
这是，运行程序会报错，因为ODSCity控件没有给TCityTableAdapter传参。

11.修改ODSCity

双击ODSCity控件，添加如下代码：

    protected void ODSCity_Selecting(object sender, ObjectDataSourceSelectingEventArgs e)
    {
        e.InputParameters["ProvinceId"] = lbProvince.SelectedValue; // 传参方法
    }

这下再运行试试。是不是一切都对了。
如果需要添加删除方法，可以继续下面的步骤。

12.添加两个Button，分别命名为btnProvinceDel和btnCityDel。实现代码如下：

    protected void btnProvinceDel_Click(object sender, EventArgs e)
    {
        int nProvinceID = Convert.ToInt16(lbProvince.SelectedValue);

        TCityTableAdapter city = new TCityTableAdapter();
        DSProvinceCity.TCityDataTable cityTable = city.GetCityData(nProvinceID);

        if (cityTable.Rows.Count > 0)
        {
            Response.Write("<script type="text/javascript">alert('这个省不能删除!');</script>");
        }
        else
        {
            TProvinceTableAdapter province = new TProvinceTableAdapter();
            province.Delete(nProvinceID);
        }
    }
    protected void btnCityDel_Click(object sender, EventArgs e)
    {
        TCityTableAdapter city = new TCityTableAdapter();
        city.Delete(Convert.ToInt16(lbCity.SelectedValue));

        lbCity.DataBind();
    }
由于省表的ID是市表的外键，因此在删除省中的记录时要先判断是否已经将级联关系删除完全。

这样，后台管理程序就OK了。

# 五，前台管理程序

1.在页面中添加两个DropDownList控件，分别命名为ddlProvince和ddlCity
2.创建一个新ObjectDataSource控件，命名为ODSCityAnother与TCityTableAdaper关联
3.与ListBox控件类似，让ddlProvince控件与ODSProvince控件绑定，让ddlCity控件与ODSCityAnother控件绑定
4.双击ODSCityAnother控件，添加代码如下：

    protected void ODSCityAnother_Selecting(object sender, ObjectDataSourceSelectingEventArgs e)
    {
        e.InputParameters["ProvinceId"] = ddlProvince.SelectedValue; 
    }

5.双击ddlProvince控件，添加代码如下：

    protected void ddlProvince_SelectedIndexChanged(object sender, EventArgs e)
    {
        ddlCity.DataBind();
    }

6.ddlProvince控件AutoPostBack属性设为True

运行一下，感受前台程序的体验吧。是不是很爽啊，哈哈~

# 附录：
ASPX页面代码：

	<html xmlns="http://www.w3.org/1999/xhtml">
	<head id="Head1" runat="server">
	    <title>ProvinceCity</title>
	    <style type="text/css">
	        .style1
	        {
	            width: 500;
	            border-width:1px;
	            border-style:solid;
	            border-color:Black;
	            margin:auto;
	        }
	        .Title
	        {
	            width:204px;
	            background-color:Blue;
	            color:White;
	        }
	        .style2
	        {
	            width: 204px;
	            border-width:1px;
	            border-style:solid;
	            border-color:Black;
	        }
	    </style>
	</head>
	<body>
	    <form id="form1" runat="server">
	
	    <div style=" width:100%; text-align:center;">
	        <div style="height:30px;">后台管理程序</div>
	        <table class="style1">
	            <tr>
	                <td class="Title">
	                    省</td>
	                <td class="Title">
	                    市</td>
	            </tr>
	            <tr>
	                <td class="style2">
	                    <asp:ListBox ID="lbProvince" runat="server"  
	                        DataSourceID="ODSProvince" DataTextField="Name" DataValueField="Id" 
	                        Height="220px" 
	                        Width="200px" onselectedindexchanged="lbProvince_SelectedIndexChanged"></asp:ListBox>
	                </td>
	                <td class="style2">
	                    <asp:ListBox ID="lbCity" runat="server" DataSourceID="ODSCity" 
	                        DataTextField="Name" DataValueField="Id" Height="220px" Width="200px">
	                    </asp:ListBox>
	                </td>
	            </tr>
	            <tr>
	                <td class="style2">
	                    <asp:Button ID="btnProvinceDel" runat="server"  
	                        Text="删除" onclick="btnProvinceDel_Click" />
	                </td>
	                <td class="style2">
	                    <asp:Button ID="btnCityDel" runat="server" Text="删除" onclick="btnCityDel_Click" 
	                         />
	                </td>
	            </tr>
	            <tr>
	                <td class="style2">
	                    <asp:TextBox ID="txtProvinceAdd" runat="server"></asp:TextBox>
	                    <asp:Button ID="btnProvinceAdd" runat="server" Text="添加" 
	                        onclick="btnProvinceAdd_Click" />
	                </td>
	                <td class="style2">
	                    <asp:TextBox ID="txtCityAdd" runat="server"></asp:TextBox>
	                    <asp:Button ID="btnCityAdd" runat="server" Text="添加" 
	                        onclick="btnCityAdd_Click" />
	                </td>
	            </tr>
	            <tr>
	                <td class="style2">
	                    &nbsp;</td>
	                <td class="style2">
	                    &nbsp;</td>
	            </tr>
	        </table>
	    </div>
	
	    <asp:ObjectDataSource ID="ODSProvince" runat="server" DeleteMethod="Delete" 
	        InsertMethod="Insert" OldValuesParameterFormatString="original_{0}" 
	        SelectMethod="GetProvinceData" 
	        TypeName="DSProvinceCityTableAdapters.TProvinceTableAdapter" 
	        UpdateMethod="Update">
	        <DeleteParameters>
	            <asp:Parameter Name="Original_Id" Type="Int32" />
	        </DeleteParameters>
	        <InsertParameters>
	            <asp:Parameter Name="Name" Type="String" />
	        </InsertParameters>
	        <UpdateParameters>
	            <asp:Parameter Name="Name" Type="String" />
	            <asp:Parameter Name="Original_Id" Type="Int32" />
	        </UpdateParameters>
	    </asp:ObjectDataSource>
	    <asp:ObjectDataSource ID="ODSCity" runat="server" DeleteMethod="Delete" 
	        InsertMethod="Insert" OldValuesParameterFormatString="original_{0}" 
	        SelectMethod="GetCityData" 
	        TypeName="DSProvinceCityTableAdapters.TCityTableAdapter" 
	        UpdateMethod="Update" onselecting="ODSCity_Selecting">
	        <DeleteParameters>
	            <asp:Parameter Name="Original_Id" Type="Int32" />
	        </DeleteParameters>
	        <InsertParameters>
	            <asp:Parameter Name="Name" Type="String" />
	            <asp:Parameter Name="ProvinceId" Type="Int32" />
	        </InsertParameters>
	        <UpdateParameters>
	            <asp:Parameter Name="Name" Type="String" />
	            <asp:Parameter Name="ProvinceId" Type="Int32" />
	            <asp:Parameter Name="Original_Id" Type="Int32" />
	        </UpdateParameters>
	    </asp:ObjectDataSource>
	    <hr/>
	
	    <div style=" width:100%; text-align:center;">
	        <div style="height:30px;">前台应用程序</div>
	         <table class="style1">
	            <tr>
	                <td class="Title">
	                    省</td>
	                <td class="Title">
	                    市</td>
	            </tr>
	            <tr>
	                <td class="style2">
	                    <asp:DropDownList ID="ddlProvince" runat="server"  
	                        DataSourceID="ODSProvince" DataTextField="Name" DataValueField="Id" onselectedindexchanged="ddlProvince_SelectedIndexChanged" 
	                        >
	                    </asp:DropDownList>
	                </td>
	                <td class="style2">
	                    <asp:DropDownList ID="ddlCity" runat="server" DataSourceID="ODSCityAnother" 
	                        DataTextField="Name" DataValueField="Id">
	                    </asp:DropDownList>
	                </td>
	            </tr>
	          </table>
	    </div>
	 
	    <asp:ObjectDataSource ID="ODSCityAnother" runat="server" DeleteMethod="Delete" 
	        InsertMethod="Insert" OldValuesParameterFormatString="original_{0}" 
	        onselecting="ODSCityAnother_Selecting" SelectMethod="GetCityData" 
	        TypeName="DSProvinceCityTableAdapters.TCityTableAdapter" UpdateMethod="Update">
	        <DeleteParameters>
	            <asp:Parameter Name="Original_Id" Type="Int32" />
	        </DeleteParameters>
	        <InsertParameters>
	            <asp:Parameter Name="Name" Type="String" />
	            <asp:Parameter Name="ProvinceId" Type="Int32" />
	        </InsertParameters>
	        <SelectParameters>
	            <asp:Parameter Name="ProvinceId" Type="Int32" />
	        </SelectParameters>
	        <UpdateParameters>
	            <asp:Parameter Name="Name" Type="String" />
	            <asp:Parameter Name="ProvinceId" Type="Int32" />
	            <asp:Parameter Name="Original_Id" Type="Int32" />
	        </UpdateParameters>
	    </asp:ObjectDataSource>
	    <hr/>
	    </form>
	</body>
	</html>