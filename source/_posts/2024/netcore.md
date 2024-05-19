---
title: .netcore
date: 2023-09-21 22:32:05
tags: 工程
hidden: true
typora-root-url: ./netcore

---

## 酒店管理系统(ASP+SQL)

> 采用ASP.NET开发，一个简单的新手项目，作用是前端和数据库通讯。

## 前端部分

创建的程序是前后端分离的（应该），选定创建完成我们的项目

![image-20240516143804408](/image-20240516143804408.png)

创建完成之后，在右侧会有对于的解决方案，

![image-20240516144907192](/image-20240516144907192.png)

在指定文件夹中会看到这个带有.sln后缀的文件，这个是指定配置文件

![image-20240516144859201](/image-20240516144859201.png)

构造和这里文件夹相关联，解决方案管理器是控制项目文件夹的图像化显示。就是用这个控制我们的文件。在解决方案中，创建几个文件夹存储我们的项目资源

![image-20240516152120048](/image-20240516152120048.png)

- 这里引入了一个`bootstrap`包，是开源的前端框架
- 创建了一个页面`login.aspx`

```asp
<%@ Page Language="C#" AutoEventWireup="true" CodeBehind="login.aspx.cs" Inherits="Blide.View._1" %>

<!DOCTYPE html>

<html xmlns="http://www.w3.org/1999/xhtml">
<head runat="server">
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <title></title>
    <%-- 1.链接css - 开源的css --%>
    <%-- 内部的css有两种格式 - 一种是压缩的一种是原装 --%>
    <link rel="stylesheet" href="../Assets/Library/bootstrap-4.6.2-dist/css/bootstrap.min.css" />
    <style>
        body {
            /*这里图片有问题*/
            background-image: url(../Assets/Images/hotel3.jpg);
            background-size: cover;
            background-repeat: no-repeat;
        }
        /*这里链接不要用括号*/
        /*前面加.表示类选择器*/
        .container-fluid {
            /*透明度*/
            opacity: 0.9;
        }
    </style>
</head>
<body>
    <%-- 2.锁定网页 md表示是中等屏幕 这里构造了网格系统
         这里有规则--%>
    <div class="container-fluid">
        <div class="row" style="height: 200px"></div>
        <div class="row">
            <div class="col-md-4"></div>
            <%-- 特殊按钮 --%>
            <div class="col-md-4 bg-light rounded-3">
                <%-- 标题 这里圆角失效了不知道为什么 还有居中--%>
                <h1 class="text-success text-center">假日酒店</h1>
                <%-- 粘贴框架html --%>
                <form>
                    <%-- 模块1 --%>
                    <div class="mb-3">
                        <label for="exampleInputEmail1" class="form-label">邮箱</label>
                        <input type="email" class="form-control" id="EmailTb" />
                    </div>
                    <%-- 模块2 --%>
                    <div class="mb-3">
                        <label for="exampleInputPassword1" class="form-label">密码</label>
                        <input type="password" class="form-control" id="PasswordTb" />
                    </div>
                    <%-- 模块3 --%>
                    <div class="mb-3 form-check">
                        <%-- 用户选项 --%>
                        <input type="checkbox" class="form-radio-input" id="AdminCb"/>
                        <label class="text-success"> 管理员 </label>
                        <%-- 加空格 --%>
                        &nbsp&nbsp
                        <input type="checkbox" class="form-radio-input" id="UserCb" />
                        <label class="text-success"> 用户 </label>
                    </div>
                    <%-- 3.加个类控制按钮 --%>
                    <div class="d-grip">
                        <button type="submit" class="btn btn-success btn-block">登录</button>
                    </div>
                    <br/>

                </form>
            </div>
            <div class="col-md-4"></div>
        </div>
    </div>
    <form id="form1" runat="server">
        <div>
        </div>
    </form>
</body>
</html>
```

引用的前端组件,是bootstrap中找的

![image-20240516154510722](/image-20240516154510722.png)

登录页面的效果是这样的

![image-20240517091633339](/image-20240517091633339.png)

然后创建一个web窗口母版页，作为其他页面的模板和跳转页面

![image-20240517091103855](/image-20240517091103855.png)

插入的导航栏，也是在`bootstrap`找的

![image-20240516214441681](/image-20240516214441681.png)

利用母版页创建其他的页面效果，这里是前期的基本样例

![image-20240517091303941](/image-20240517091303941.png)

- `Categories`

```asp
<%@ Page Title="" Language="C#" MasterPageFile="~/View/Admin/AdminMaster.Master" AutoEventWireup="true" CodeBehind="Categories.aspx.cs" Inherits="Blide.View.Admin.Categories" %>
<asp:Content ID="Content1" ContentPlaceHolderID="head" runat="server">
</asp:Content>
<asp:Content ID="Content2" ContentPlaceHolderID="MyBody" runat="server">
        <%----%>

    <%-- 写一些内容 --%>
    <div class="container-fluid">
        <div class="row">
            <div class="col-4"></div>
            <div class="col-4">
                <h1 class="text-success text-center">房型管理</h1>
            </div>
            <div class="col-4"></div>
        </div>

        <div class="row">
            <%--  --%>
            <div class="col-md-3">
                <form>
                    <%-- 模块1 --%>
                    <div class="mb-3">
                        <label for="CatNameTb" class="form-label">房型名称</label>
                        <input type="text" class="form-control" id="CatNameTb" />
                    </div>

                    <%-- 模块3 --%>
                    <div class="mb-3">
                        <label for="RemarkTb" class="form-label">标签</label>
                        <input type="text" class="form-control" id="RemarkTb">
                    </div>

                    <%-- 3.加个类控制按钮 --%>
                    <div class="d-grip">
                        <button type="submit" class="btn btn-success btn-block">保存</button>
                    </div>
                    <br />
                </form>
            </div>

            <%-- 第二层  --%>
            <div class="col-md-9">
                <%-- 数据id - 给与间隔 在设计上可以用格式--%>
                <asp:GridView ID="RoomsGV" runat="server" class="table" CellPadding="4" ForeColor="#333333" GridLines="None">
                    <AlternatingRowStyle BackColor="White" />
                    <EditRowStyle BackColor="#7C6F57" />
                    <FooterStyle BackColor="#1C5E55" Font-Bold="True" ForeColor="White" />
                    <HeaderStyle BackColor="#1C5E55" Font-Bold="True" ForeColor="White" />
                    <PagerStyle BackColor="#666666" ForeColor="White" HorizontalAlign="Center" />
                    <RowStyle BackColor="#E3EAEB" />
                    <SelectedRowStyle BackColor="#C5BBAF" Font-Bold="True" ForeColor="#333333" />
                    <SortedAscendingCellStyle BackColor="#F8FAFA" />
                    <SortedAscendingHeaderStyle BackColor="#246B61" />
                    <SortedDescendingCellStyle BackColor="#D4DFE1" />
                    <SortedDescendingHeaderStyle BackColor="#15524A" />
                </asp:GridView>
            </div>
        </div>
    </div>
</asp:Content>

```

- `Rooms`

```asp
<%-- 包含母版页的页面 --%>

<%@ Page Title="" Language="C#" MasterPageFile="~/View/Admin/AdminMaster.Master" AutoEventWireup="true" CodeBehind="Rooms.aspx.cs" Inherits="Blide.View.Admin.WebForm1" %>

<asp:Content ID="Content1" ContentPlaceHolderID="head" runat="server">
</asp:Content>

<%-- 需要引入样式 --%>
<asp:Content ID="Content2" ContentPlaceHolderID="MyBody" runat="server">
    <%----%>

    <%-- 写一些内容 --%>
    <div class="container-fluid">
        <div class="row">
            <div class="col-4"></div>
            <div class="col-4">
                <h1 class="text-success text-center">客房管理</h1>
            </div>
            <div class="col-4"></div>
        </div>

        <div class="row">
            <%--  --%>
            <div class="col-md-3">
                <form>
                    <%-- 模块1 --%>
                    <div class="mb-3">
                        <label for="RNameTb" class="form-label">客房名称</label>
                        <input type="text" class="form-control" id="RNameTb" />
                    </div>
                    <%-- 模块2 --%>
                    <div class="mb-3">
                        <label for="CatCb" class="form-label">房型</label>
                        <%-- 这里是列表 - 下拉 --%>
                        <asp:DropDownList ID="CatCb" runat="server" class="form-control"></asp:DropDownList>
                    </div>
                    <%-- 模块3 --%>
                    <div class="mb-3">
                        <label for="LocationTb" class="form-label">位置</label>
                        <input type="text" class="form-control" id="LocationTb">
                    </div>

                    <%-- 模块4 --%>
                    <div class="mb-3">
                        <label for="CostTb" class="form-label">价格</label>
                        <input type="text" class="form-control" id="CostTb">
                    </div>

                    <%-- 模块5 --%>
                    <div class="mb-3">
                        <label for="CostTb" class="form-label">标签</label>
                        <input type="text" class="form-control" id="RemaksTb">
                    </div>

                    <%-- 3.加个类控制按钮 --%>
                    <div class="d-grip">
                        <button type="submit" class="btn btn-success btn-block">保存</button>
                    </div>
                    <br />

                </form>
            </div>

            <%-- 第二层  --%>
            <div class="col-md-9">
                <%-- 数据id - 给与间隔 在设计上可以用格式--%>
                <asp:GridView ID="RoomsGV" runat="server" class="table" CellPadding="4" ForeColor="#333333" GridLines="None">
                    <AlternatingRowStyle BackColor="White" />
                    <EditRowStyle BackColor="#7C6F57" />
                    <FooterStyle BackColor="#1C5E55" Font-Bold="True" ForeColor="White" />
                    <HeaderStyle BackColor="#1C5E55" Font-Bold="True" ForeColor="White" />
                    <PagerStyle BackColor="#666666" ForeColor="White" HorizontalAlign="Center" />
                    <RowStyle BackColor="#E3EAEB" />
                    <SelectedRowStyle BackColor="#C5BBAF" Font-Bold="True" ForeColor="#333333" />
                    <SortedAscendingCellStyle BackColor="#F8FAFA" />
                    <SortedAscendingHeaderStyle BackColor="#246B61" />
                    <SortedDescendingCellStyle BackColor="#D4DFE1" />
                    <SortedDescendingHeaderStyle BackColor="#15524A" />
                </asp:GridView>
            </div>
        </div>
    </div>
</asp:Content>

```

- `Users`

```asp
<%@ Page Title="" Language="C#" MasterPageFile="~/View/Admin/AdminMaster.Master" AutoEventWireup="true" CodeBehind="Users.aspx.cs" Inherits="Blide.View.Admin.Users" %>

<asp:Content ID="Content1" ContentPlaceHolderID="head" runat="server">
</asp:Content>
<asp:Content ID="Content2" ContentPlaceHolderID="MyBody" runat="server">
    <%-- 需要引入样式 --%>
    <%-- 写一些内容 --%>
    <div class="container-fluid">
        <div class="row">
            <div class="col-4"></div>
            <div class="col-4">
                <h1 class="text-success text-center">用户管理</h1>
            </div>
            <div class="col-4"></div>
        </div>

        <div class="row">
            <%--  --%>
            <div class="col-md-3">
                <form>
                    <%-- 模块1 --%>
                    <div class="mb-3">
                        <label for="UNameTb" class="form-label">用户名</label>
                        <input type="text" class="form-control" id="UNameTb" />
                    </div>
                    <%-- 模块2 --%>
                    <div class="mb-3">
                        <label for="PhoneTb" class="form-label">电话号码</label>
                        <input type="text" class="form-control" id="PhoneTb">
                    </div>
                    <%-- 模块3 --%>
                    <div class="mb-3">
                        <label for="CenCb" class="form-label">性别</label>
                        <%-- 这里是列表 - 下拉 --%>
                        <asp:DropDownList ID="DropDownList1" runat="server" class="form-control"></asp:DropDownList>
                    </div>

                    <%-- 模块4 --%>
                    <div class="mb-3">
                        <label for="AddressTb" class="form-label">地址</label>
                        <input type="text" class="form-control" id="AddressTb">
                    </div>

                    <%-- 模块5 --%>
                    <div class="mb-3">
                        <label for="PasswordTb" class="form-label">密码</label>
                        <input type="text" class="form-control" id="PasswordTb">
                    </div>

                    <%-- 3.加个类控制按钮 --%>
                    <div class="d-grip">
                        <button type="submit" class="btn btn-success btn-block">保存</button>
                    </div>
                    <br />
                </form>
            </div>

            <%-- 第二层  --%>
            <div class="col-md-9">
                <%-- 数据id - 给与间隔 在设计上可以用格式--%>
                <asp:GridView ID="RoomsGV" runat="server" class="table" CellPadding="4" ForeColor="#333333" GridLines="None">
                    <AlternatingRowStyle BackColor="White" />
                    <EditRowStyle BackColor="#7C6F57" />
                    <FooterStyle BackColor="#1C5E55" Font-Bold="True" ForeColor="White" />
                    <HeaderStyle BackColor="#1C5E55" Font-Bold="True" ForeColor="White" />
                    <PagerStyle BackColor="#666666" ForeColor="White" HorizontalAlign="Center" />
                    <RowStyle BackColor="#E3EAEB" />
                    <SelectedRowStyle BackColor="#C5BBAF" Font-Bold="True" ForeColor="#333333" />
                    <SortedAscendingCellStyle BackColor="#F8FAFA" />
                    <SortedAscendingHeaderStyle BackColor="#246B61" />
                    <SortedDescendingCellStyle BackColor="#D4DFE1" />
                    <SortedDescendingHeaderStyle BackColor="#15524A" />
                </asp:GridView>
            </div>
        </div>
    </div>

</asp:Content>
```

简单运行后就是这个效果

![image-20240516215052307](/image-20240516215052307.png)

链接`bootstrap`的css样式

![image-20240516220125441](/image-20240516220125441.png)

在设计处可以简单看看效果，这里需要设计数据库的内容，之后设计几个数据库表。

![image-20240516230955045](/image-20240516230955045.png)

这里是用到工具箱中的控件来实现的：

![image-20240517092244792](/image-20240517092244792.png)

用于显示和操作数据库中的数据，算是数据表格吧。

![image-20240516231348604](/image-20240516231348604.png)

自动生成，确定格式自动配置代码。

![image-20240516231402151](/image-20240516231402151.png)

在母版页面配置`URL`然后配置`url`页面即可跳转，前端样式就如下

![image-20240519215241930](/image-20240519215241930.png)

母版页面代码大致是这样：

```asp
<%-- 这里是母版页面 --%>

<%@ Master Language="C#" AutoEventWireup="true" CodeBehind="Site1.master.cs" Inherits="Blide.View.Admin.Site1" %>

<!DOCTYPE html>

<html>
<head runat="server">
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <title></title>
    <asp:ContentPlaceHolder ID="head" runat="server">
    </asp:ContentPlaceHolder>
    <%-- 引入页面样式 --%>
    <link rel="stylesheet" href="../../Assets/Library/bootstrap-4.6.2-dist/css/bootstrap.min.css" />
</head>
<body>
    <%-- 4.修改颜色 --%>
    <nav class="navbar navbar-expand-lg navbar-success bg-success">
        <div class="container-fluid">
            <a class="navbar-brand text-light"  href="#">HMS</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarScroll" aria-controls="navbarScroll" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarScroll">
                <%-- 模板页面 母版带有url地址可以跳转到指定的页面--%>
                <ul class="navbar-nav me-auto my-2 my-lg-0 navbar-nav-scroll" style="--bs-scroll-height: 100px;">
                    <%-- 列表1  --%>
                    <li class="nav-item">
                        <a class="nav-link text-light" href="Rooms.aspx">客房</a>
                    </li>
                    <%-- 列表2 --%>
                    <li class="nav-item">
                        <a class="nav-link text-light" href="Categories.aspx">房型</a>
                    </li>
                    <%-- 列表3 --%>
                    <li class="nav-item">
                        <a class="nav-link text-light" href="Users.aspx">用户</a>
                    </li>
                    <%-- 列表4 --%>
                    <li class="nav-item">
                        <a class="nav-link text-light" href="../login.aspx">退出</a>
                    </li>
                </ul>

            </div>
        </div>
    </nav>


    <form id="form1" runat="server">
        <div>
            <asp:ContentPlaceHolder ID="MyBody" runat="server">
            </asp:ContentPlaceHolder>
        </div>
    </form>
</body>
</html>
```

## 后端数据库部分(大概)

创建数据库，这里需要建几个表

![image-20240517093235487](/image-20240517093235487.png)

![image-20240517152119079](/image-20240517152119079.png)

表的`sql`语句如下，这里需要注意一下Id需要标注自增。

- `BookingTb1`

```sql
CREATE TABLE [dbo].[BookingTb1] (
    [BId]     INT  NOT NULL,
    [BDate]   DATE NOT NULL,
    [BRoom]   INT  NOT NULL,
    [Agent]   INT  NOT NULL,
    [Datein]  DATE NOT NULL,
    [DateOut] DATE NOT NULL,
    [Amount]  INT  NOT NULL,
    PRIMARY KEY CLUSTERED ([BId] ASC),
    CONSTRAINT [FK3] FOREIGN KEY ([Agent]) REFERENCES [dbo].[UserTb1] ([UId]),
    CONSTRAINT [FK2] FOREIGN KEY ([BRoom]) REFERENCES [dbo].[RoomTb1] ([RId])
);
```

- `CategoryTb1`

```sql
CREATE TABLE [dbo].[CategoryTb1] (
    [CatId]      INT           IDENTITY (100, 1) NOT NULL,
    [CatName]    NVARCHAR (50) NOT NULL,
    [CatRemakes] NVARCHAR (50) NOT NULL,
    PRIMARY KEY CLUSTERED ([CatId] ASC)
);
```

- `RoomTb1`

```sql
CREATE TABLE [dbo].[RoomTb1] (
    [RId]       INT            NOT NULL,
    [RName]     NVARCHAR (50)  NOT NULL,
    [RCategory] INT            NOT NULL,
    [RLocation] NVARCHAR (150) NOT NULL,
    [RCost]     INT            NOT NULL,
    [RRemakes]  NVARCHAR (150) NOT NULL,
    [Status]    NVARCHAR (50)  NOT NULL,
    PRIMARY KEY CLUSTERED ([RId] ASC),
    CONSTRAINT [FK1] FOREIGN KEY ([RCategory]) REFERENCES [dbo].[CategoryTb1] ([CatId])
);
```

- `UserTb1`

```sql
CREATE TABLE [dbo].[UserTb1] (
    [UId]    INT            IDENTITY (500, 1) NOT NULL,
    [UName]  VARCHAR (50)   NOT NULL,
    [UPhone] VARCHAR (50)   NOT NULL,
    [UGen]   NVARCHAR (10)  NOT NULL,
    [RAdd]   NVARCHAR (150) NOT NULL,
    [UPass]  VARCHAR (50)   NOT NULL,
    PRIMARY KEY CLUSTERED ([UId] ASC)
);
```

输入完成记得更新一下

![image-20240517093641389](/image-20240517093641389.png)

出不来要按刷新

![image-20240517094453030](/image-20240517094453030.png)

## 管理面板

构建一下，数据库和前端交互的方法

![image-20240517110144868](/image-20240517110144868.png)

在Models文件夹创建一个`Functions类`

![image-20240517152522404](/image-20240517152522404.png)

这里的代码就是构造数据库，获取或者向数据库输入数据

```c#
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.SqlClient;
using System.Linq;
using System.Web;

namespace Blide.Models
{
    public class Functions
    {
        private SqlConnection Con;
        private SqlCommand Cmd;
        private DataTable dt;
        private string ConStr;
        private SqlDataAdapter sda;

        // 用于执行 SQL 命令，并返回受影响的行数，通常用于数据库的写操作（如插入、更新和删除）。
        public int setData(string Query)
        {
            int Cnt;
            if(Con.State == ConnectionState.Closed)
            {
                Con.Open();
            }
            Cmd.CommandText = Query;
            Cnt = Cmd.ExecuteNonQuery();
            Con.Close();

            return Cnt;
        }
        // GetData 方法用于执行 SQL 查询，并返回查询结果，以 DataTable 的形式存储，通常用于数据库的读取操作。
        public DataTable GetData(string Query)
        {
            // 创建一个新的 DataTable 对象
            dt = new DataTable();

            // 使用 SqlDataAdapter 执行 SQL 查询，并将结果填充到 DataTable 中
            sda = new SqlDataAdapter(Query, ConStr);
            sda.Fill(dt);

            // 返回填充后的 DataTable 对象
            return dt;
        }
		//初始化了数据库连接相关的属性
        public Functions()
        {
            // 链接字符串 - 这里的@的作用是什么 - 
            ConStr = @"Data Source=(LocalDB)\\MSSQLLocalDB;AttachDbFilename=C:\\Users\\27332\\Documents\\EHotelDb.mdf;Integrated Security=True;Connect Timeout=30";
            Con = new SqlConnection(ConStr);
            Cmd = new SqlCommand();
            Cmd.Connection = Con;
        }

    }
}
```

> @符号用于定义连接字符串 ConStr，这个连接字符串包含了连接数据库所需的信息，如数据源、数据库文件路径等。

对应的在aspx中。.aspx.cs 文件是后端代码文件，用于控制与其对应的 .aspx 页面的行为和逻辑。我们需要对于这个分类的页面就行编码。

![image-20240517153305039](/image-20240517153305039.png)

然后再设计页面中通过点击对应的按钮来绑定点击或者输入事件：

![image-20240519215906792](/image-20240519215906792.png)

再对应的前端代码处会出现`Onclick`

![image-20240519220011735](/image-20240519220011735.png)

之前创建函数的时候，其实是没有绑定这个点击事件。

![image-20240517165717183](/image-20240517165717183.png)

对应的数据库中，也要设计编码`Chinese_PRC_CI_AS`，要不然输出不出来中文

![image-20240517171010174](/image-20240517171010174.png)

然后就可以添加到数据库了。

![image-20240517171046477](/image-20240517171046477.png)

不支持中文的话，这里需要配置一下代码

```sql
alter database[C:\USERS\27332\DOCUMENTS\EHOTELDB.MDF] set single_user with rollback immediate;
alter database[C:\USERS\27332\DOCUMENTS\EHOTELDB.MDF] collate Chinese_PRC_CI_AS;
alter database[C:\USERS\27332\DOCUMENTS\EHOTELDB.MDF] set multi_user;
```

![image-20240517171019112](/image-20240517171019112.png)

![image-20240517172622832](/image-20240517172622832.png)

对数据库输入上面的`SQL`语句即可

![image-20240517172727863](/image-20240517172727863.png)

在数据表格中，右上角选择选择按钮，对这个进行添加选择。点击选择按钮可以讲选择的数据填入到前端的框中。

![image-20240517183217257](/image-20240517183217257.png)

![image-20240517223324138](/image-20240517223324138.png)

制作对应按钮的函数，让它可以访问数据库。

![image-20240517223342041](/image-20240517223342041.png)

![image-20240517223348715](/image-20240517223348715.png)

顺带修改了一下按钮的布局：（虽然说还是简陋）

![image-20240517231738520](/image-20240517231738520.png)

对应完成的前端代码：

```asp
<%@ Page Title="" Language="C#" MasterPageFile="~/View/Admin/AdminMaster.Master" AutoEventWireup="true" CodeBehind="Categories.aspx.cs" Inherits="Blide.View.Admin.Categories" EnableEventValidation="false"%>

<asp:Content ID="Content1" ContentPlaceHolderID="head" runat="server">
</asp:Content>
<asp:Content ID="Content2" ContentPlaceHolderID="MyBody" runat="server">
    <%-- 控制 --%>
    <%-- 第二层  --%>
    <div class="container-fluid">
        <div class="row">
            <div class="col-4"></div>
            <div class="col-4">
                <h1 class="text-success text-center">房型管理</h1>
            </div>
            <div class="col-4"></div>
        </div>

        <div class="row">
            <%-- 数据id - 给与间隔 在设计上可以用格式--%>
            <div class="col-md-3">
                <form>
                    <%-- 输入对话框1 --%>
                    <div class="mb-3">
                        <label for="CatNameTb" class="form-label">房型名称</label>
                        <%-- 这里的server可以被后端 --%>
                        <input type="text" class="form-control" id="CatNameTb" runat="server" />
                    </div>

                    <%-- 输入对话框3 --%>
                    <div class="mb-3">
                        <label for="RemarkTb" class="form-label">标签</label>
                        <input type="text" class="form-control" id="RemarkTb" runat="server" />
                    </div>

                    <%-- 最后的Onclick是绑定点击事件的 不能删除--%>
                    <%-- 按钮 - 编辑和删除  form-control表单格式一至--%>
                    <div class="row">
                        <div class ="col d-grid">
                            <asp:Button ID="EditBtn" runat="server" Text="编辑" class="btn btn-warning btn-block form-control" OnClick="EditBtn_Click"/>
                        </div>
                        <%-- 并排 --%>
                        <div class ="col d-grid">
                            <asp:Button ID="DeleteBtn" runat="server" Text="删除" class="btn btn-danger btn-block form-control" OnClick="DeleteBtn_Click" />
                        </div>         
                        <br />
                    </div>

                    <%-- 保存控制按钮 --%>
                    <div class ="d-grid mb-3">
                        <label id="ErrMsg" runat="server" class="text-danger"></label>
                        <%-- 控制 --%>
                        <asp:Button ID="SaveBtn" runat="server" Text="保存" class ="form-control btn btn-success btn-block" OnClick="SaveBtn_Click" />
                    </div>

                </form>
            </div>

            <%-- 自动生成  --%>
            <div class="col-md-9">
                <%-- 数据id - 给与间隔 在设计上可以用格式--%>
                <asp:GridView ID="CategoriesGV" runat="server" class="table" CellPadding="4" ForeColor="#333333" GridLines="None" OnSelectedIndexChanged="CategoriesGV_SelectedIndexChanged1">
                    <AlternatingRowStyle BackColor="White" />
                    <Columns>
                        <asp:TemplateField ShowHeader="False">
                            <ItemTemplate>
                                <asp:LinkButton ID="LinkButton1" runat="server" CausesValidation="False" CommandName="Select" Text="选择"></asp:LinkButton>
                            </ItemTemplate>
                        </asp:TemplateField>
                    </Columns>
                    <EditRowStyle BackColor="#7C6F57" />
                    <FooterStyle BackColor="#1C5E55" Font-Bold="True" ForeColor="White" />
                    <HeaderStyle BackColor="#1C5E55" Font-Bold="True" ForeColor="White" />
                    <PagerStyle BackColor="#666666" ForeColor="White" HorizontalAlign="Center" />
                    <RowStyle BackColor="#E3EAEB" />
                    <SelectedRowStyle BackColor="#C5BBAF" Font-Bold="True" ForeColor="#333333" />
                    <SortedAscendingCellStyle BackColor="#F8FAFA" />
                    <SortedAscendingHeaderStyle BackColor="#246B61" />
                    <SortedDescendingCellStyle BackColor="#D4DFE1" />
                    <SortedDescendingHeaderStyle BackColor="#15524A" />
                </asp:GridView>
            </div>
        </div>
    </div>
</asp:Content>
```

对应的asp.cs

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;

namespace Blide.View.Admin
{
    public partial class Categories : System.Web.UI.Page
    {
        Models.Functions Con;
        // 数据库链接代码功能实现页面
        protected void Page_Load(object sender, EventArgs e)
        {
            Con = new Models.Functions();
            ShowCategories();
        }

        // 把设计中的表格显示出来 - 没有数据会报错
        private void ShowCategories()
        {
            // 拿出数据库的值
            string Query = "select CatId as Id, CatName as Categories, CatRemarks from CategoryTb1";
            CategoriesGV.DataSource = Con.GetData(Query);
            CategoriesGV.DataBind();
            CategoriesGV.HeaderRow.Cells[1].Text = "序号";
            CategoriesGV.HeaderRow.Cells[2].Text = "房型";
            CategoriesGV.HeaderRow.Cells[3].Text = "标签";

        }


        // 双击button创建的函数 - 这里按钮会调用吧 - 添加
        protected void SaveBtn_Click(object sender, EventArgs e)
        {
            try
            {
                // 获取aspx中的数据
                string CatName = CatNameTb.Value;
                string Rem = RemarkTb.Value;
                string Query = "insert into CategoryTb1 values('{0}','{1}')";
                // 这里是什么
                Query = string.Format(Query, CatName, Rem);
                Con.setData(Query);
                ShowCategories();
                ErrMsg.InnerText = "房型已添加!!!";
            }
            catch (Exception Ex)
            {
                ErrMsg.InnerText = Ex.Message;
            }
        }

        //输入三个值 - 选择
        int Key = 0;
        protected void CategoriesGV_SelectedIndexChanged1(object sender, EventArgs e)
        {
            // 在对话框中输入我们选择的值
            Key = Convert.ToInt32(CategoriesGV.SelectedRow.Cells[1].Text);
            CatNameTb.Value = CategoriesGV.SelectedRow.Cells[2].Text;
            RemarkTb.Value = CategoriesGV.SelectedRow.Cells[3].Text;
        }
        // 保存
        protected void EditBtn_Click(object sender, EventArgs e)
        {
            // 从上面移动下来的
            try
            {
                // 获取aspx中的数据
                string CatName = CatNameTb.Value;
                string Rem = RemarkTb.Value;
                string Query = "update CategoryTb1 set CatName = '{0}',CatRemarks = '{1}' where CatId = {2}";
                // 这里是什么
                Query = string.Format(Query, CatName, Rem, CategoriesGV.SelectedRow.Cells[1].Text);
                Con.setData(Query);
                ShowCategories();
                ErrMsg.InnerText = "房型已更新!!!";
            }
            catch (Exception Ex)
            {
                ErrMsg.InnerText = Ex.Message;
            }
        }
        // 删除
        protected void DeleteBtn_Click(object sender, EventArgs e)
        {
            // 从上面移动下来的
            try
            {
                // 获取aspx中的数据
                string CatName = CatNameTb.Value;
                string Rem = RemarkTb.Value;
                string Query = "delete from CategoryTb1 where CatId = {0}";
                // 这里是什么 - 转化为字符串形式
                Query = string.Format(Query, CategoriesGV.SelectedRow.Cells[1].Text);
                Con.setData(Query);
                ShowCategories();
                ErrMsg.InnerText = "房型已删除!!!";
            }
            catch (Exception Ex)
            {
                ErrMsg.InnerText = Ex.Message;
            }
        }
    }

}
```

## 客房服务

因为结构上类似，代码都是以上面为基础而改的

![image-20240518172300215](/image-20240518172300215.png)

在前端中标签一个类，`required`表示必须填写

![image-20240518173603183](/image-20240518173603183.png)

对应设计的前端页面代码：

```asp
<%-- 包含母版页的页面 --%>

<%@ Page Title="" Language="C#" MasterPageFile="~/View/Admin/AdminMaster.Master" AutoEventWireup="true" CodeBehind="Rooms.aspx.cs" Inherits="Blide.View.Admin.WebForm1" EnableEventValidation="false" %>

<asp:Content ID="Content1" ContentPlaceHolderID="head" runat="server">
</asp:Content>

<%-- 需要引入样式 --%>
<asp:Content ID="Content2" ContentPlaceHolderID="MyBody" runat="server">
    <%----%>

    <%-- 写一些内容 --%>
    <div class="container-fluid">
        <div class="row">
            <div class="col-4"></div>
            <div class="col-4">
                <h1 class="text-success text-center">客房管理</h1>
            </div>
            <div class="col-4"></div>
        </div>

        <div class="row">
            <%--  --%>
            <div class="col-md-3">
                <form>
                    <%-- 客房名称1 --%>
                    <div class="mb-3">
                        <label for="RNameTb" class="form-label">客房名称</label>
                        <input type="text" class="form-control" id="RNameTb" runat="server" required="required" />
                    </div>
                    <%-- 房型2 --%>
                    <div class="mb-3">
                        <label for="CatCb" class="form-label">房型</label>
                        <%-- 这里是列表 - 下拉 --%>
                        <asp:DropDownList ID="CatCb" runat="server" required="required" class="form-control" ></asp:DropDownList>
                    </div>
                    <%-- 位置3 --%>
                    <div class="mb-3">
                        <label for="LocationTb" class="form-label">位置</label>
                        <input type="text" class="form-control" id="LocationTb" runat="server" required="required">
                    </div>

                    <%-- 模块4 --%>
                    <div class="mb-3">
                        <label for="CostTb" class="form-label">价格</label>
                        <input type="text" class="form-control" id="CostTb" runat="server" required="required">
                    </div>

                    <%-- 模块5 这里的标签一定要和数据库标签对应--%>
                    <div class="mb-3">
                        <label for="RemarksTb" class="form-label">标签</label>
                        <input type="text" class="form-control" id="RemarksTb" runat="server" required="required">
                    </div>


                    <%-- 模块5 --%>
                    <div class="mb-3">
                        <label for="CatCb" class="for-label">状态 </label>
                        <asp:DropDownList ID="StatusCb" runat="server" class="for-control">
                            <asp:ListItem>空闲</asp:ListItem>
                            <asp:ListItem>已预定</asp:ListItem>
                        </asp:DropDownList>
                    </div>



                    <%-- 按钮 --%>
                    <div class="d-grip">
                        <div class="row">
                            <div class="col d-grid">
                                <asp:Button ID="EditBtn" runat="server" Text="编辑" class="btn btn-warning btn-block form-control" OnClick="EditBtn_Click" />
                            </div>
                            <%-- 并排 --%>
                            <div class="col d-grid">
                                <asp:Button ID="DeleteBtn" runat="server" Text="删除" class="btn btn-danger btn-block form-control" OnClick="DeleteBtn_Click" />
                            </div>
                            <br />
                        </div>

                        <%-- 保存控制按钮 --%>
                        <div class="d-grid mb-3">
                            <label id="ErrMsg" runat="server" class="text-danger"></label>
                            <%-- 控制 --%>
                            <asp:Button ID="SaveBtn" runat="server" Text="保存" class="form-control btn btn-success btn-block" OnClick="SaveBtn_Click" />
                        </div>
                    </div>
                    <br />

                </form>
            </div>

            <%-- 第二层  --%>
            <div class="col-md-9">
                <%-- 数据id - 给与间隔 在设计上可以用格式--%>
                <asp:GridView ID="RoomsGV" runat="server" class="table" CellPadding="4" ForeColor="#333333" GridLines="None" OnSelectedIndexChanged="RoomsGV_SelectedIndexChanged">
                    <AlternatingRowStyle BackColor="White" />
                    <Columns>
                        <asp:CommandField ShowSelectButton="True" />
                    </Columns>
                    <EditRowStyle BackColor="#7C6F57" />
                    <FooterStyle BackColor="#1C5E55" Font-Bold="True" ForeColor="White" />
                    <HeaderStyle BackColor="#1C5E55" Font-Bold="True" ForeColor="White" />
                    <PagerStyle BackColor="#666666" ForeColor="White" HorizontalAlign="Center" />
                    <RowStyle BackColor="#E3EAEB" />
                    <SelectedRowStyle BackColor="#C5BBAF" Font-Bold="True" ForeColor="#333333" />
                    <SortedAscendingCellStyle BackColor="#F8FAFA" />
                    <SortedAscendingHeaderStyle BackColor="#246B61" />
                    <SortedDescendingCellStyle BackColor="#D4DFE1" />
                    <SortedDescendingHeaderStyle BackColor="#15524A" />
                </asp:GridView>
            </div>
        </div>
    </div>
</asp:Content>

```

后端代码：

```c#
using System;

namespace Blide.View.Admin
{


    public partial class WebForm1 : System.Web.UI.Page
    {

        Models.Functions Con;
        protected void Page_Load(object sender, EventArgs e)
        {
            Con = new Models.Functions();
            ShowRooms();
            GetCategories();
        }

        // 显示中文标签名称
        private void ShowRooms()
        {
            string Query = "select * from RoomTb1";
            RoomsGV.DataSource = Con.GetData(Query);
            RoomsGV.DataBind();
            RoomsGV.HeaderRow.Cells[0].Text = "房间号";
            RoomsGV.HeaderRow.Cells[1].Text = "房间类型";
            RoomsGV.HeaderRow.Cells[2].Text = "床位";
            RoomsGV.HeaderRow.Cells[3].Text = "价格";
            RoomsGV.HeaderRow.Cells[4].Text = "是/否";
            RoomsGV.HeaderRow.Cells[5].Text = "备注";
            RoomsGV.HeaderRow.Cells[6].Text = "状态";
        }

        private void GetCategories()
        {
            string Query = "Select * from CategoryTb1";
            // 控件传回端口 - 作用不晓得
            if(!Page.IsPostBack)
            {
                CatCb.DataTextField = Con.GetData(Query).Columns["CatName"].ToString();
                CatCb.DataValueField = Con.GetData(Query).Columns["CatId"].ToString();
                CatCb.DataSource = Con.GetData(Query);
                CatCb.DataBind();
            }

        }

        protected void EditBtn_Click(object sender, EventArgs e)
        {
            try
            {
                // 从文本框和下拉框中获取用户输入的值
                string RName = RNameTb.Value;
                string RCat = CatCb.SelectedValue.ToString();
                string Rloc = LocationTb.Value;
                string Cost = CostTb.Value;
                string Rem = RemarksTb.Value;
                string Status = StatusCb.SelectedValue.ToString();

                // 生成更新查询语句
                string Query = "update RoomTb1 set RName = '{0}', RCategory = '{1}', RLocation = '{2}', RCost = '{3}', RRemarks = '{4}', Status = '{5}' where Rid = {6}";
                Query = string.Format(Query, RName, RCat, Rloc, Cost, Rem, Status, RoomsGV.SelectedRow.Cells[1].Text);

                // 执行查询语句
                Con.setData(Query);

                // 更新显示
                ShowRooms();

                // 清空输入控件的值
                ErrMsg.InnerText = "客房已更新！！";
                reset();

            }
            catch (Exception Ex)
            {
                // 捕获异常并显示错误信息
                ErrMsg.InnerText = Ex.Message;
            }
        }

        protected void DeleteBtn_Click(object sender, EventArgs e)
        {
            try
            {
                string Query = "delete from RoomTb1 where RId = {0}";
                Query = string.Format(Query, RoomsGV.SelectedRow.Cells[1].Text);
                Con.setData(Query);
                ShowRooms();
                ErrMsg.InnerText = "客房信息已删除 !!!";
                reset();
            }
            catch(Exception Ex)
            {
                ErrMsg.InnerText = Ex.Message;
            }
        }

        protected void SaveBtn_Click(object sender, EventArgs e)
        {
            try
            {
                // 获取aspx中的数据 - 前端输入框输入数据导入数据库
                string RName = RNameTb.Value;
                string RCat = CatCb.SelectedValue.ToString();
                string Cost = CostTb.Value;
                string Rloc = LocationTb.Value;
                string Rem = RemarksTb.Value;
                string Status = "空闲";
                string Query = "insert into RoomTb1 values('{0}','{1}','{2}','{3}','{4}','{5}')";
                // 拼起来 - 字符串拼接
                Query = string.Format(Query, RName, RCat, Rloc, Cost, Rem, Status);
                Con.setData(Query);
                ShowRooms();
                ErrMsg.InnerText = "客房已添加!";
                // 重置一下变量
                reset();
            }
            catch (Exception Ex)
            {
                ErrMsg.InnerText = Ex.Message;
            }
        }
        // 清空前端输入框
        private void reset()
        {
            RNameTb.Value = "";
            CatCb.SelectedIndex = -1;
            LocationTb.Value = "";
            CostTb.Value = "";
            RemarksTb.Value = "";
        }
        // 这里是不是应该用私有变量
        int Key = 0;
        protected void RoomsGV_SelectedIndexChanged(object sender, EventArgs e)
        {
            // 在对话框中输入我们选择的值
            Key = Convert.ToInt32(RoomsGV.SelectedRow.Cells[1].Text);
            RNameTb.Value = RoomsGV.SelectedRow.Cells[2].Text;
            CatCb.SelectedValue = RoomsGV.SelectedRow.Cells[3].Text;
            LocationTb.Value = RoomsGV.SelectedRow.Cells[4].Text;
            CostTb.Value = RoomsGV.SelectedRow.Cells[5].Text;
            RemarksTb.Value = RoomsGV.SelectedRow.Cells[6].Text;
            StatusCb.SelectedValue = RoomsGV.SelectedRow.Cells[7].Text;
        }
    }
}
```

这里报错，主键之前没有输入过自增，数据库设计的问题。修改加上`RPIMARY KEY INENTITY`

![image-20240519095711946](/image-20240519095711946.png)

然后就可以自由交互了：

![image-20240519100827038](/image-20240519100827038.png)

![image-20240519102351991](/image-20240519102351991.png)

## 用户面板

![image-20240519155815821](/image-20240519155815821.png)

前端代码：

```asp
<%@ Page Title="" Language="C#" MasterPageFile="~/View/Admin/AdminMaster.Master" AutoEventWireup="true" CodeBehind="Users.aspx.cs" Inherits="Blide.View.Admin.Users" %>

<asp:Content ID="Content1" ContentPlaceHolderID="head" runat="server">
</asp:Content>
<asp:Content ID="Content2" ContentPlaceHolderID="MyBody" runat="server">
    <%-- 需要引入样式 --%>
    <%-- 写一些内容 --%>
    <div class="container-fluid">
        <div class="row">
            <div class="col-4"></div>
            <div class="col-4">
                <h1 class="text-success text-center">用户管理</h1>
            </div>
            <div class="col-4"></div>
        </div>

        <div class="row">
            <%--  --%>
            <div class="col-md-3">
                <form>
                    <%-- 模块1 --%>
                    <div class="mb-3">
                        <label for="UNameTb" class="form-label">用户名</label>
                        <input type="text" class="form-control" id="UNameTb" runat="server" />
                    </div>
                    <%-- 模块2 --%>
                    <div class="mb-3">
                        <label for="PhoneTb" class="form-label">电话号码</label>
                        <input type="text" class="form-control" id="PhoneTb" runat="server" />
                    </div>
                    <%-- 模块3 --%>
                    <div class="mb-3">
                        <label for="GenCb" class="form-label">性别</label>
                        <%-- 这里是列表 - 下拉 --%>
                        <asp:DropDownList ID="GenCb" runat="server" class="form-control">
                            <asp:ListItem>男</asp:ListItem>
                            <asp:ListItem>女</asp:ListItem>
                        </asp:DropDownList>
                    </div>

                    <%-- 模块4 --%>
                    <div class="mb-3">
                        <label for="AddressTb" class="form-label">地址</label>
                        <input type="text" class="form-control" id="AddressTb" runat="server">
                    </div>

                    <%-- 模块5 --%>
                    <div class="mb-3">
                        <label for="PasswordTb" class="form-label">密码</label>
                        <input type="text" class="form-control" id="PasswordTb" runat="server">
                    </div>

                    <%-- 按钮 --%>
                    <div class="d-grip">
                        <div class="row">
                            <div class="col d-grid">
                                <asp:Button ID="EditBtn" runat="server" Text="编辑" class="btn btn-warning btn-block form-control" OnClick="EditBtn_Click"  />
                            </div>
                            <%-- 并排 --%>
                            <div class="col d-grid">
                                <asp:Button ID="DeleteBtn" runat="server" Text="删除" class="btn btn-danger btn-block form-control" OnClick="DeleteBtn_Click" />
                            </div>
                            <br />
                        </div>

                        <%-- 保存控制按钮 --%>
                        <div class="d-grid mb-3">
                            <label id="ErrMsg" runat="server" class="text-danger"></label>
                            <%-- 控制 --%>
                            <asp:Button ID="SaveBtn" runat="server" Text="保存" class="form-control btn btn-success btn-block" OnClick="SaveBtn_Click" />
                        </div>
                    </div>
                    <br />
                </form>
            </div>

            <%-- 第二层  --%>
            <div class="col-md-9">
                <%-- 数据id - 给与间隔 在设计上可以用格式--%>
                <asp:GridView ID="UserGV" runat="server" class="table" CellPadding="4" ForeColor="#333333" GridLines="None" OnSelectedIndexChanged="UserGV_SelectedIndexChanged">
                    <AlternatingRowStyle BackColor="White" />
                    <Columns>
                        <asp:CommandField ShowSelectButton="True" />
                    </Columns>
                    <EditRowStyle BackColor="#7C6F57" />
                    <FooterStyle BackColor="#1C5E55" Font-Bold="True" ForeColor="White" />
                    <HeaderStyle BackColor="#1C5E55" Font-Bold="True" ForeColor="White" />
                    <PagerStyle BackColor="#666666" ForeColor="White" HorizontalAlign="Center" />
                    <RowStyle BackColor="#E3EAEB" />
                    <SelectedRowStyle BackColor="#C5BBAF" Font-Bold="True" ForeColor="#333333" />
                    <SortedAscendingCellStyle BackColor="#F8FAFA" />
                    <SortedAscendingHeaderStyle BackColor="#246B61" />
                    <SortedDescendingCellStyle BackColor="#D4DFE1" />
                    <SortedDescendingHeaderStyle BackColor="#15524A" />
                </asp:GridView>
            </div>
        </div>
    </div>

</asp:Content>

```

后端代码：

```c#
using System;
using System.Collections.Generic;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;

namespace Blide.View.Admin
{
    public partial class Users : System.Web.UI.Page
    {
        Models.Functions Con;
        protected void Page_Load(object sender, EventArgs e)
        {
            Con = new Models.Functions();
            ShowUsers();
        }

        private void ShowUsers()
        {
            string Query = "select * from UserTb1";
            DataTable DataSource = Con.GetData(Query);
            UserGV.DataSource = DataSource;
            UserGV.DataBind();

            // Set GridView Header Texts
            UserGV.HeaderRow.Cells[1].Text = "ID";
            UserGV.HeaderRow.Cells[2].Text = "用户名";
            UserGV.HeaderRow.Cells[3].Text = "邮箱";
            UserGV.HeaderRow.Cells[4].Text = "注册时间";
            UserGV.HeaderRow.Cells[5].Text = "地区";
            UserGV.HeaderRow.Cells[6].Text = "密码";

            // Set Save Button Text
            SaveBtn.Text = "保存更改";
        }

        // 点击 - 编辑
        protected void EditBtn_Click(object sender, EventArgs e)
        {
            string UName = UNameTb.Value;
            string UPhone = PhoneTb.Value;
            string UGen = GenCb.SelectedValue;
            string UAdd = AddressTb.Value;
            string UPass = PasswordTb.Value;

            string Query = "update UserTb1 set UName = '{0}', UPhone = '{1}', UGen = '{2}', UAdd = '{3}', UPass = '{4}' where UId = {5}";
            Query = string.Format(Query, UName, UPhone, UGen, UAdd, UPass, UserGV.SelectedRow.Cells[1].Text);
            Con.setData(Query);
            ErrMsg.InnerText = "用户信息更新";
            reset();
        }
        // 重置输入框
        private void reset()
        {
            UNameTb.Value = "";
            PhoneTb.Value = "";
            GenCb.SelectedValue = "";
            AddressTb.Value = "";
            PasswordTb.Value = "";
        }

        // 删除
        protected void DeleteBtn_Click(object sender, EventArgs e)
        {
            string Query = "delete from UserTb1 where UId = {0}";
            Query = string.Format(Query, UserGV.SelectedRow.Cells[1].Text);
            Con.setData(Query);
            ShowUsers();
            ErrMsg.InnerText = "用户信息已删除!!!!";
            reset();
        }
        // 保存
        protected void SaveBtn_Click(object sender, EventArgs e)
        {
            try
            {
                string UName = UNameTb.Value;
                string UPhone = PhoneTb.Value;
                string UGen = GenCb.SelectedValue;
                string UAdd = AddressTb.Value;
                string UPass = PasswordTb.Value;

                // 修正SQL查询语句的字符串拼接方式和占位符位置
                string Query = "insert into UserTb1 values('{0}','{1}','{2}','{3}','{4}')";
                Query = string.Format(Query, UName, UPhone, UGen, UAdd, UPass);

                Con.setData(Query);
                ShowUsers();
                ErrMsg.InnerText = "用户已添加!!!";
                reset();
            }
            catch (Exception Ex)
            {
                ErrMsg.InnerText = Ex.Message;
            }
        }
		// 转化输出
        int Key = 0;
        protected void UserGV_SelectedIndexChanged(object sender, EventArgs e)
        {
            Key = Convert.ToInt32(UserGV.SelectedRow.Cells[1].Text);
            UNameTb.Value = UserGV.SelectedRow.Cells[2].Text;
            PhoneTb.Value = UserGV.SelectedRow.Cells[3].Text;
            GenCb.SelectedValue = UserGV.SelectedRow.Cells[4].Text;
            AddressTb.Value = UserGV.SelectedRow.Cells[5].Text;
            PasswordTb.Value = UserGV.SelectedRow.Cells[6].Text;
        }

        // 选择

    }
}
```

## 登录页面

修改`id`方便后面关联，接下来要做两个页面，管理员登录和用户登录

![image-20240519111837973](/image-20240519111837973.png)

修改主页面样式

![image-20240519121802806](/image-20240519121802806.png)

![image-20240519121751654](/image-20240519121751654.png)

登录前端页面：

```asp
<%@ Page Language="C#" AutoEventWireup="true" CodeBehind="login.aspx.cs" Inherits="Blide.View._1" EnableEventValidation="false" %>

<!DOCTYPE html>

<html xmlns="http://www.w3.org/1999/xhtml">
<head runat="server">
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <title></title>
    <%-- 1.链接css - 开源的css --%>
    <%-- 内部的css有两种格式 - 一种是压缩的一种是原装 --%>
    <link rel="stylesheet" href="../Assets/Library/bootstrap-4.6.2-dist/css/bootstrap.min.css" />
    <style>
        body {
            /*这里图片有问题*/
            background-image: url(../Assets/Images/hotel3.jpg);
            background-size: cover;
            background-repeat: no-repeat;
        }
        /*这里链接不要用括号*/
        /*前面加.表示类选择器*/
        .container-fluid {
            /*透明度*/
            opacity: 0.9;
        }
    </style>
</head>
<body>
    <%-- 2.锁定网页 md表示是中等屏幕 这里构造了网格系统
         这里有规则--%>
    <form id="forml" runat="server">
        <div class="container-fluid">
            <div class="row" style="height: 200px"></div>
            <div class="row">
                <%-- 居中盒子 --%>
                <div class="col-md-4"></div>
                <%-- 特殊按钮 --%>
                <div class="col-md-4 bg-light rounded-3">
                    <%-- 标题 这里圆角失效了不知道为什么 还有居中--%>
                    <h1 class="text-success text-center">假日酒店</h1>
                    <%-- 粘贴框架html --%>

                    <%-- 模块1 --%>
                    <div class="mb-3">
                        <label for="UserTb" class="form-label">用户名</label>
                        <input type="text" class="form-control" id="UserTb" runat="server" required="required"/>
                    </div>
                    <%-- 模块2 --%>
                    <div class="mb-3">
                        <label for="PasswordTb" class="form-label">密码</label>
                        <input type="password" class="form-control" id="PasswordTb" runat="server" required="required"/>
                    </div>

                    <%-- 选定 --%>
                    <div class="mb-3 form-check">
                        <%-- 提示标签 --%>
                        <label id ="ErrMsg" class="text-danger" runat="server"></label>
                        <%-- 用户选项 --%>
                        <input type="radio" runat="server" id="AdminCb" name="Role" />
                        <label class="text-success">管理员 </label>
                        <%-- 加空格 --%>
                        &nbsp&nbsp
                        <input type="radio" runat="server" id="UserCb" name="Role" />
                        <label class="text-success">用户 </label>
                    </div>

                    <%-- 控制按钮 这里的样式不够长--%>
                    <div class="mb-3 form-check container">
                         <asp:Button ID="LoginBtn" class="btn btn-success btn-block form-check" runat="server" Text="登录" OnClick="LoginBtn_Click" style="width: 100%;"/>
                    </div>

                </div>
            </div>
        </div>
    </form>

</body>

</html>
```

登录后端页面：

```c#
using System;
using System.Collections.Generic;
using System.Data;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;

namespace Blide.View
{
    public partial class _1 : System.Web.UI.Page
    {
        Models.Functions Con;
        protected void Page_Load(object sender, EventArgs e)
        {
            Con = new Models.Functions();
            Session["UserName"] = "";
            Session["UId"] = "";
        }

        protected void LoginBtn_Click(object sender, EventArgs e)
        {
            
            if(AdminCb.Checked)
            {
                if (UserTb.Value == "Admin" && PasswordTb.Value == "password")
                {
                    Session["UserName"] = "Admin";
                    // 登录管理员
                    Response.Redirect("Admin/Rooms.aspx");
                }
                else
                {
                    ErrMsg.InnerText = "无效管理员";
                }
            }
            else
            {
                string Query = "select UId, UName, UPass from UserTb1 where UName = '{0}' and UPass = '{1}'";
                Query = string.Format(Query,UserTb.Value,PasswordTb.Value);
                DataTable dt = Con.GetData(Query);
                if(dt.Rows.Count == 0)
                {
                    ErrMsg.InnerText = "无效用户";
                }
                else
                {
                    Session["UserName"] = dt.Rows[0][0].ToString();
                    Session["UId"] = Convert.ToInt32(dt.Rows[0][0].ToString());
                    Response.Redirect("Admin/Categories.aspx");
                }
            }
        }
    }
}
```

就先这样吧，之后有空来完善，这只是一个粗略的项目，只完成了前端和后端的交互，而且ASP也没啥人用了(雾)。

---------
