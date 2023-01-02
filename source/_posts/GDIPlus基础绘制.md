---
title: GDIPlus基础绘制
date: 2023-01-02 21:27:26
tags: GDI+
---

## 类型准备

​	绘制时最核心的几个类型:

​	1.Gdiplus::Graphics  集成绘制功能的核心类

​	2.Gdiplus::PointF ,Gdiplus::Point   在绘制平面上定义坐标点的核心类

​    3.Gdiplus::Pen 绘制时使用的画笔

​    4.Gdiplus::SolidBrush 绘制时使用的画刷

​	5.Gdiplus::FontFamily 绘制字符串时系统字体对象

​	6.Gdiplus::Font 绘制字符串时的字体 由FontFamily 提供字



## 绘制示例

		//以窗口作为 绘图平面
	Gdiplus::Graphics gObj(this->m_hWnd);
	
	switch (DrawType)
	{
	case DrawType::Line:/// 线
	{
		Gdiplus::Pen blackPen(Color(255, 0, 0, 0), 5);//一只线宽为3的 黑色画笔
		//定义点的位置
		Gdiplus::PointF point1(10.0f, 10.0f);
	
		Gdiplus::PointF point2(10.0f, 100.0f);
		Gdiplus::PointF point3(100.0f, 100.0f);
		Gdiplus::PointF point4(100.0f, 10.0f);
		Gdiplus::PointF point5(10.0f, 10.0f);
	
		Gdiplus::PointF points[5] = { point1,point2 ,point3,point4,point5 };
		Gdiplus::PointF* pPoint = points;
	
		gObj.DrawLines(&blackPen, pPoint, 5);
	}
		break;
	case DrawType::Rect:/// 矩形
	{
		Gdiplus::Pen bluePen(Color(255, 0, 0, 255), 3);
		Gdiplus::RectF rectf(200,200,200,200);
		gObj.DrawRectangle(&bluePen, rectf);
	}
		break;
	case DrawType::Curve:/// 曲线
	{
		Gdiplus::Pen greenPen(Color::Green, 3);
		Gdiplus::Pen redPen(Color::Red, 3);
	
		//定义曲线经过的点
		Point p1(100,100);
		Point p2(200, 50);
		Point p3(700, 10);
		Point p4(500, 100);
	
		Point curvePoints[7] = { p1,p2,p3,p4 };
	
		Point* pCurvePoints = curvePoints;
	
		//绘制曲线
		gObj.DrawCurve(&greenPen, pCurvePoints, 7);
	
		//绘制弯曲强度的曲线
		//gObj.DrawCurve(&redPen, pCurvePoints, 7, 1.5f);
	
		//绘制曲线的定义红点
		Gdiplus::SolidBrush sb(Color::Red);
		gObj.FillEllipse(&sb, Rect(95, 95, 10, 10));
		gObj.FillEllipse(&sb, Rect(200, 50, 10, 10));
		gObj.FillEllipse(&sb, Rect(395, 5, 10, 10));
		gObj.FillEllipse(&sb, Rect(795, 95, 10, 10));
	
		//绘制坐标
		Gdiplus::SolidBrush coordSb(Color::Black);
		Gdiplus::FontFamily ff(L"微软雅黑");
		Gdiplus::Font font(&ff,12, Gdiplus::FontStyleRegular, Gdiplus::UnitPixel);
		PointF coordP1(100, 100);
		gObj.DrawString(L"坐标:100,100", -1, &font, coordP1, &coordSb);
		coordP1.X = 200;
		coordP1.Y = 50;
		gObj.DrawString(L"坐标:200,50", -1, &font, coordP1, &coordSb);
	}
	break;
	case DrawType::CloseCurve:/// 闭合曲线
	{
		Gdiplus::Pen pen(Color::LightGreen, 3);
		
		PointF p1(100, 100);
		PointF p2(200, 50);
		PointF p3(400, 10);
		PointF p4(500, 100);
		PointF p5(600, 200);
		PointF p6(700, 400);
		PointF p7(500, 500);
	
		PointF curvePoints[7] = { p1,p2,p3,p4,p5,p6,p7 };
	
		//绘制闭合曲线
		gObj.DrawClosedCurve(&pen, curvePoints, 7);
	
		//曲线的定义点
		Gdiplus::SolidBrush sb(Color::Red);
		gObj.FillEllipse(&sb,Rect(95,95,10,10));
		gObj.FillEllipse(&sb, Rect(795, 95, 10, 10));
		gObj.FillEllipse(&sb, Rect(795, 495, 10, 10));
		gObj.FillEllipse(&sb, Rect(195, 45, 10, 10));
		gObj.FillEllipse(&sb, Rect(395, 5, 10, 10));
		gObj.FillEllipse(&sb, Rect(595, 195, 10, 10));
		gObj.FillEllipse(&sb, Rect(695, 395, 10, 10));
	
	}
	break;
	case DrawType::Bezier:/// 贝塞尔
	{
		Pen pen(Color::Black, 3);
	
		Point startPoint(100, 100);
		Point contrlPoint1(200, 10);
		Point contrlPoint2(350, 50);
		Point endPoint(500, 150);
	
		gObj.DrawBezier(&pen, startPoint, contrlPoint1, contrlPoint2, endPoint);
	
		SolidBrush sb(Color::Green);
		SolidBrush sb1(Color::Red);
		gObj.FillEllipse(&sb, Rect(100, 100, 10, 10));
		gObj.FillEllipse(&sb1, Rect(200, 10, 10, 10));
		gObj.FillEllipse(&sb1, Rect(350, 50, 10, 10));
		gObj.FillEllipse(&sb, Rect(500, 150, 10, 10));
	
	}
	break;
	case DrawType::Polygon:/// 多边形
	{
		Pen pen(Color::Blue, 5);
	
		Point p1(100, 100);
		Point p2(200, 130);
		Point p3(150, 200);
		Point p4(50, 200);
		Point p5(0, 130);
	
		Point points[5] = { p1,p2,p3,p4,p5 };
	
		gObj.DrawPolygon(&pen, points, 5);
	}
	break;
	case DrawType::Arc:/// 弧度
	{
		Pen pen(Color::Brown, 3);
		Pen rectPen(Color::Black, 1);
	
		Rect rectngle(100, 100, 200, 100);
	
		gObj.DrawArc(&pen, rectngle, 0, 90);
		gObj.DrawRectangle(&rectPen, rectngle);
	}
	break;
	case DrawType::Pie:/// 扇形
	{
		Pen pen(Color::CadetBlue, 3);
		Pen pen1(Color::Black, 1);
		Rect rectangle(100, 100, 150, 150);
		gObj.DrawPie(&pen, rectangle, 0, 90);
		gObj.DrawRectangle(&pen1, rectangle);
	}
	break;
	case DrawType::Text:/// 字符串
	{
		TCHAR str[] = L"这是一次测试!";
	
		Gdiplus::FontFamily ff(L"黑体");
		Gdiplus::Font f(&ff, 20, Gdiplus::FontStyle::FontStyleBold, Gdiplus::Unit::UnitPixel);
			
		RectF layoutRect(10, 10, 200, 100);
		Gdiplus::StringFormat sf;
		sf.SetAlignment(StringAlignment::StringAlignmentCenter);
		sf.SetLineAlignment(StringAlignment::StringAlignmentNear);
	
		SolidBrush sb(Color::Black);
		size_t len = wcslen(str);
		gObj.DrawString(str, len, &f, layoutRect, &sf, &sb);
		gObj.DrawRectangle(&Pen(Color::Red,3), layoutRect);
	}
	break;
	}

​	关于更多的绘制API 可以查看  GdiplusGraphics.h 头文件。。
