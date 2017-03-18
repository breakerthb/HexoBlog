---
title: CSharpGDI+绘图
tags:
  - CSharp
  - GDI
categories: CSharp
date: 2017-03-18 05:58:48
---

GDI+：Graphics Device Interface Plus也就是图形设备接口,提供了各种丰富的图形图像处理功能;在C#.NET中，使用GDI+处理二维（2D）的图形和图像，使用DirectX处理三维（3D）的图形图像,图形图像处理用到的主要命名空间是System . Drawing：提供了对GDI+基本图形功能的访问，主要有Graphics类、Bitmap类、从Brush类继承的类、Font类、Icon类、Image类、Pen类、Color类等.

画板可以通过Graphics这个类来创建

笔又可以分好多种类,比如铅笔（用来画线条）,画刷（用来画区域）等。在c#中我们可以用Pen,Brush类来实现类似功能

颜色自然是用Color类

所需命名空间：using System.Drawing;

# Demo 1 : 在空白窗体中画基本图形

准备一个画板:
创建一个画板主要有3种方式:

> A: 在窗体或控件的Paint事件中直接引用Graphics对象  
> B: 利用窗体或某个控件的CreateGraphics方法  
> C: 从继承自图像的任何对象创建Graphics对象

这次我们就先以A为例:

    private void Form1_Paint(object sender, PaintEventArgs e)
    {
    	Graphics g = e.Graphics; //创建画板,这里的画板是由Form提供的.
    }

然后,我们要只笔:

    private void Form1_Paint(object sender, PaintEventArgs e)
    {
		Graphics g = e.Graphics; //创建画板,这里的画板是由Form提供的.
		Pen p = new Pen(Color.Blue, 2);//定义了一个蓝色,宽度为的画笔
    }

接下来我们就可以来画画了.

	private void Form1_Paint(object sender, PaintEventArgs e)
	{
		Graphics g = e.Graphics; //创建画板,这里的画板是由Form提供的.
		Pen p = new Pen(Color.Blue, 2);//定义了一个蓝色,宽度为的画笔
		g.DrawLine(p, 10, 10, 100, 100);//在画板上画直线,起始坐标为(10,10),终点坐标为(100,100)
		g.DrawRectangle(p, 10, 10, 100, 100);//在画板上画矩形,起始坐标为(10,10),宽为,高为
		g.DrawEllipse(p, 10, 10, 100, 100);//在画板上画椭圆,起始坐标为(10,10),外接矩形的宽为,高为
	}

效果图如下:

![执行效果](http://upload-images.jianshu.io/upload_images/145902-73dd0d82c9c17074.JPG)


# Demo 2 : Pen的使用

Pen的属性主要有: Color(颜色),DashCap(短划线终点形状),DashStyle(虚线样式),EndCap(线尾形状),StartCap(线头形状),Width(粗细)等.我们可以用Pen 来画虚线,带箭头的直线等

    Pen　p = new　Pen(Color.Blue, 5);//设置笔的粗细为,颜色为蓝色
    
    Graphics　g = this.CreateGraphics();
    
    //画虚线
    
    p.DashStyle = DashStyle.Dot;//定义虚线的样式为点
    
    g.DrawLine(p, 10, 10, 200, 10);
    
    //自定义虚线
    p.DashPattern = new　float[] { 2, 1 };//设置短划线和空白部分的数组
    g.DrawLine(p, 10, 20, 200, 20);
    //画箭头,只对不封闭曲线有用
    p.DashStyle = DashStyle.Solid;//实线
    p.EndCap = LineCap.ArrowAnchor;//定义线尾的样式为箭头
    g.DrawLine(p, 10, 30, 200, 30);
    g.Dispose();
    p.Dispose();

以上代码运行结果:

![http://images.cnblogs.com/cnblogs\_com/stg609/a3.JPG](http://upload-images.jianshu.io/upload_images/145902-16f9e2f1f8bb35f0.jpg)

# Demo 3 : Brush的使用

作用:我们可以用画刷填充各种图形形状，如矩形、椭圆、扇形、多边形和封闭路径等,主要有几种不同类型的画刷:

SolidBrush：画刷最简单的形式，用纯色进行绘制

HatchBrush：类似于 SolidBrush，但是可以利用该类从大量预设的图案中选择绘制时要使用的图案，而不是纯色

TextureBrush：使用纹理（如图像）进行绘制

LinearGradientBrush：使用沿渐变混合的两种颜色进行绘制

PathGradientBrush ：基于编程者定义的唯一路径，使用复杂的混合色渐变进行绘制

我们这里只是简单介绍使用其中的几种:


    Graphics g = this.CreateGraphics();
    
    Rectangle rect = new Rectangle(10, 10, 50, 50);//定义矩形,参数为起点横纵坐标以及其长和宽
    
    //单色填充
    
    SolidBrush b1 = new SolidBrush(Color.Blue);//定义单色画刷　　　　　
    
    g.FillRectangle(b1, rect);//填充这个矩形
    
    //字符串
    
    g.DrawString("字符串", new Font("宋体", 10), b1, new PointF(90, 10));
    
    //用图片填充
    
    TextureBrush b2 = new TextureBrush(Image.FromFile(@"e:picture1.jpg"));
    
    rect.Location = new Point(10, 70);//更改这个矩形的起点坐标
    
    rect.Width = 200;//更改这个矩形的宽来
    
    rect.Height = 200;//更改这个矩形的高
    
    g.FillRectangle(b2, rect);
    
    //用渐变色填充
    
    rect.Location = new Point(10, 290);
    
    LinearGradientBrush b3 = new　LinearGradientBrush(rect, Color.Yellow , Color.Black , LinearGradientMode.Horizontal);
    
    g.FillRectangle(b3, rect);

　　运行效果图:

![http://images.cnblogs.com/cnblogs\_com/stg609/a1.jpg](http://upload-images.jianshu.io/upload_images/145902-f0d75c4d89030d54.jpg)


# Demo 4 : 颜色的使用

在GDI+中，颜色封装在Color结构中。把红、绿、蓝色值传送给Color结构的一个函数，就可以创建一中颜色，几乎从来不需要创建颜色。Color结构包含大约150个属性，提供了大量的预置颜色。如果需要以LightGoldenrodYellow 或 LavenderBlush颜色绘制图形，都有预定义的颜色我们使用。声明一个Color类型的变量，用Color结构中的一种颜色初始化它，如下：

Color redColor = Color.Red;

Color anotherColor = Color.LightGoldenrodYellow;

下面几乎已经准备好绘图了，但绘图前有两点需要注意。

表示颜色的另一种方法是把颜色分解为3种组件：色调、饱和度、亮度。Color结构包含完成分解颜色的实用方法：GetBrightness()、GetHue()、GetSaturation()。

下面的示例将创建一个颜色选择对话框，使用它查看以RGB定义的颜色和以色调、饱和度、亮度定义的颜色之间的关系。

    public Form1()
    {
    
    InitializeComponent();
    
    this.colorDialog1.ShowDialog();
    
    }

生成这个窗体时，就生成了颜色选择对话框。

运行应用程序，单击Define Custom Colors按钮，显示一个对话框，在其中可以使用鼠标选择一种颜色，查看该颜色的RGB值。还可以获得该颜色的色调、饱和度、亮度值。也可以直接输入RGB值，查看得到的颜色。


# Demo 5 : 坐标轴变换

在winform中的坐标轴和我们平时接触的平面直角坐标轴不同,winform中的坐标轴方向完全相反:窗体的左上角为原点(0,0),水平向左则X增大,垂直下向则Y增大

![效果](http://upload-images.jianshu.io/upload_images/145902-b7532e0e0f58cc6e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们来实际操作下,通过旋转坐标轴的方向来画出不同角度的图案,或通过更改坐标原点的位置来平衡坐标轴的位置.

    Graphics g = this.CreateGraphics();
    //单色填充
    //SolidBrush b1 = new SolidBrush(Color.Blue);//定义单色画刷　　　　　
    Pen p = new Pen(Color.Blue,1);
    //转变坐标轴角度
    for (int i = 0; i < 90; i++)
    {
    　　 g.RotateTransform(i);//每旋转一度就画一条线
    　　 g.DrawLine(p, 0, 0, 100, 0);
    　　 g.ResetTransform();//恢复坐标轴坐标
    }
    //平移坐标轴
    g.TranslateTransform(100, 100);
    g.DrawLine(p, 0, 0, 100, 0);
    g.ResetTransform();
    //先平移到指定坐标,然后进行度旋转
    g.TranslateTransform(100,200);
    for (int i = 0; i < 8; i++)
    {
        g.RotateTransform(45);
        g.DrawLine(p, 0, 0, 100, 0);
    }
    g.Dispose();

运行效果图:

![执行效果](http://upload-images.jianshu.io/upload_images/145902-7dd399b659ac9708.jpg)


# Demo 6 : Point

GDI+使用Point表示一个点。这是一个二维平面上的点--一个像素的表示方式。许多GDI+函数例如DrawLine()。以Point作为参数。声明和构造Point的代码如下所示：

	Point p = new Point()1,1;

有一些公共属性可以获得和设置Point的X和Y的坐标。


# Demo 7 : Size

GDI+使用Size表示一个尺寸(像素)。Size结构包含宽度和高度。声明和构造Size的代码如下所示：
    
	Size s = new Size(5,5);

有一些公共属性可以获得和设置Size的宽度和高度。


# Demo 8 : Rectangle

GDI+在许多不同的地方使用这个结构，以指定矩形的坐标。Point结构定义矩形的左上角，Size定义其大小。Rectangle有两个构造函数。一个构造函数的参数是X坐标，Y坐标，宽度和高度，另一个构造函数的参数是Point和Size结构，声明和构建Rectangle的两个范例如下：

	Rectangle r1 = new Rectangle(1,2,5,6);
	Point p = new Point(1,2);
	Size s = new Size(5,6);
	Rectangle = new Rectangle(p,s);
       
有一些公共属性可以获得和设置Rectangle的4个点和大小。另外，还有其他属性和方法可以完成诸如测试矩形是否维空，确定矩形是否与另一个矩形相交，提取两个矩形的相交部分，合并两个矩形等工作。

下面两个更重要的数据类型可以用作GDI+中许多绘图函数的参数。


# Demo 9 : GraphicsPaths

GraphicsPath类表示一系列连续的线条和曲线。在构造一条路径时，可以添加线条、Bezier曲线、圆弧、饼形图、多边形和矩形等。在构造一条复杂的路径后，可以用一个操作绘制路径：调用DrawPath()。可以调用FillPath()填充路径。
使用一个点数组和PathTypes构造GraphicsPath，PathTypes是一个byte数组，其中的每个元素对应于点数组中的每一个元素，并给出了路径如何通过这些点来构造的其他信息。例如：如果点是路径的起始点，那么这个点的路径类型就是PathPointType.Start。如果点是两个线条的连接点，那么这个点的路径类型就是PathPointType.Line。如果点用于构造一条从前一点到后一点之间的Bezier曲线，路径类型就是PathPointType.Bezier。
下面是用4条线段创建一个图形路径：

	using System.Drawing.Drawing2D; //增加此句
	
	protected override void OnPaint(PaintEventArgs e)
	{
	        GraphicsPath path;
	        path = new GraphicsPath(new Point[]{
	                   new Point(10,10),
	                   new Point(150,10),
	                   new Point(200,150),
	                   new Point(10,150),
	                   new Point(200,160),
	                   },new byte[]{
	                   (byte)PathPointType.Start,
	                   (byte)PathPointType.Line,
	                   (byte)PathPointType.Line,
	                   (byte)PathPointType.Line,
	                   (byte)PathPointType.Line});
	        e.Graphics.DrawPath(Pens.Black,path);
	}

# Demo 10 : Regions

Region类是一个复杂的图形，由矩形和路径组成。在构造了一个Regions后，就可以使用FillRegion()方法绘制该区域。下面的示例创建一个Region对象，并绘制到窗口中。
下面的代码创建一个区域，给它添加一个Rectangle和一个GraphicsPath，再用蓝色填充该区域：

	using System.Drawing,Drawing2D;
	protected override void OnPaint(PaintEventArgs e)
	{
	    Rectangle r1 = new Rectangle(10,10,50,50);
	    Rectangle r2 = new Rectangle(40,40,50,50);
	    Region r = new Region(r1);
	    r.Union(r2);
	
	    GraphicsPath path = new GraphicsPath(new Point[]{
	        new Point(45,45),
	        new Point(145,55),
	        new Point(200,150),
	        new Point(75,150),
	        new Point(45,45)}, 
	        new byte[]{
	        (byte)PathPointType.Start,
	        (byte)PathPointType.Bezier,
	        (byte)PathPointType.Bezier,
	        (byte)PathPointType.Bezier,
	        (byte)PathPointType.Line});
	
	    r.Union(path);
	    e.Graphics.FillRegion(Brushes.Blue, r);
	}

  
构造区域代码有些复杂，最复杂的是如何构造组成区域的路径。构造区域包括构造矩形和路径，之后调用Union()方法。如果决定使矩形和路径相交，就可以用Intersection()方法替代Union()。


PS:最后我们来看下Graphics这个画板上我们还可以画什么

其实我们上面用到的都是在画一些简单的图形,直线,矩形,扇形,圆孤等,我们还可以用它来绘制图片,这可以用它的DrawI
mage方法.