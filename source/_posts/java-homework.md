---
title: Java作业
date: 2019-10-15 13:54:39
tags: java
categories: 编程
---
<center>BMI计算器</center>
<!--more-->

---

```java
import java.text.DecimalFormat;
import java.util.Scanner;

import org.omg.CORBA.PRIVATE_MEMBER;

class GetBMI{
	double weight,height,final_BMI;
	boolean right = true;
	
	//计算BMI的值	
	double GetBMI(){
		final_BMI = weight/(height*height);
		return final_BMI;
	}
	
	//获取身高体重数据，并对输入的数据进行可行性验证
	void getData() {
		boolean a = true;
		Scanner input = new Scanner(System.in);
		while(a) {
			System.out.println("请输入你的体重:");
			if(input.hasNextDouble()) {
				weight = input.nextDouble();
				System.out.println("请输入你的身高:");
				height = input.nextDouble();
				break;
			}else {
				System.out.println("数据输入错误，请重新来！");
				continue;
			}
		}
	}
	
	//获取BMI所在的范围，打印出提醒语句
	void compare() {
		GetBMI();
		if (final_BMI >= 40) {
			System.out.println("You can go die!!!!");
		}else  if(final_BMI >= 35 && final_BMI < 40) {
			System.out.println("你严重肥胖！");
		}else if (final_BMI >= 30 && final_BMI < 35) {
			System.out.println("你属于肥胖！");
		}else if (final_BMI >= 25 && final_BMI < 30) {
			System.out.println("你属于偏胖！");
		}else if (final_BMI >= 18 && final_BMI < 25 ) {
			System.out.println("你的BMI正常！");
		}else{
			System.out.println("你太瘦了！");
		}
	}
	
	//获得计算结果并输出
	void show() {
		DecimalFormat dFormat = new DecimalFormat("0.00");
		this.GetBMI();
		System.out.println("Your BMI is :"+dFormat.format(final_BMI));
		compare();
	}
	
}
public class BMICauculator {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
			GetBMI bmi = new GetBMI();
			bmi.getData();
			bmi.show();
	}

}

```
