---
title: JAVA-抽象
date: 2019-10-26 11:52:47
tags: java
categories: 编程
---

<center>JAVA中的抽象，作业实例</center>
<!--more-->

```java
//定义了一个抽象基类（父类）
public abstract class Shape {
	
	protected String shapeName;
	
	public abstract double getArea(); //抽象方法
	public abstract double getLength(); //抽象方法
	
	Shape(String shapeName) {
		this.shapeName = shapeName;
		System.out.println("形状是："+shapeName);
	}
	
	public static void main(String args[]) {
		Shape rectangle,circle;
		rectangle = new Rectangle("矩形", 3.0, 4.0);
		rectangle.getArea();
		rectangle.getLength();
		circle = new Circle("圆", 5.0);
		circle.getArea();
		circle.getLength();
	}
	
}

//继承自Shape类，必须实现父类的抽象方法
class Rectangle extends Shape{
	Double area,length,width,height;
	Rectangle(String shapeName,double width,double height) {
		super(shapeName); 
		this.width = width;
		this.height = height;
		
	}
	
	//实现父类中的getArea方法
	public double getArea() {
		area = width * height;
		System.out.println("面积是：" + area);
		return area;
	}
	
	//实现父类中的getLength方法
	public double getLength() {
		length = 2 * (width + height);
		System.out.println("周长是：" + length);
		return length;
	}
	
}

//继承自Shape类，必须实现父类的抽象方法
class Circle extends Shape{
	Double area,length,radius;
	final double PI = 3.14;
	Circle(String shapeName,double radius) {
		super(shapeName);
		this.radius = radius;
		
	}
	
	//实现父类中的getArea方法
	public double getArea() {
		area = PI * (radius * radius);
		System.out.println("面积是：" + area);
		return area;
	}
	
	//实现父类中的getLength方法
	public double getLength() {
		length = (2 * radius) * PI;
		System.out.println("周长是：" + length);
		return length;
	}
	
}
```