---
title: (Java)猜一个随机数字
date: 2020-01-11
tags: java
categories: 编程
---

<center></center>
<!--more-->

<center>GuessRandomNuber.java</center>	

```java

public class GuessRandomNumber extends JFrame {
	private ButtonPanel panel;
	private JTextField input;
	private JLabel output;
	private Random random;
	private int randomInt;
	//设置一个随机数
	public void createRandomInt() {
		random = new Random();
		randomInt = random.nextInt(1000) + 1;
		System.out.println("随机数是：" + randomInt);
	}
	
	/*初始化方法*/
	public void init() {
		//设置窗体属性
		setSize(600,337);
		setTitle("窗体");
		//初始化组件
		input = new JTextField();
		output = new JLabel("请猜个数字"); 
		panel = new ButtonPanel();
		//获取面板
		Container con = this.getContentPane();
		//面板设置布局
		con.setLayout(new BorderLayout());
		//panel.setLayout(new GridLayout(1,3));
		
		//将组件添加到面板
		con.add(input,BorderLayout.NORTH);
		con.add(output,BorderLayout.CENTER);
		con.add(panel,BorderLayout.SOUTH);
		//设置组件属性
		output.setFont(new Font("XHei",Font.BOLD,20));
		output.setForeground(Color.RED);
		
		setVisible(true);
		//设置默认关闭动作
		setDefaultCloseOperation(EXIT_ON_CLOSE);
		
		//生成一个随机数
		createRandomInt();
		
		panel.getGuess().addActionListener(new ActionListener() {
			
			@Override
			public void actionPerformed(ActionEvent e) {
				// TODO Auto-generated method stub
				output.setText("Guess...");
				String value = input.getText();
				int guessNumer = -1;
				try {
					guessNumer = Integer.parseInt(value);
				} catch (NumberFormatException e1) {
					// TODO Auto-generated catch block
					e1.printStackTrace();
					output.setText("您输入的不是数字，请重新输入");
					return;
				}
				
				if(guessNumer < 0 || guessNumer > 1000) {
					output.setText("猜的数字要在1~1000，请重新输入");
					return;
				}
				if(guessNumer > randomInt) {
					output.setText("猜大了，往小了猜");
					input.setText("");
				}
				else if( guessNumer < randomInt) {
					output.setText("猜小了，往大了猜");
					input.setText("");
				}
				else {
					output.setText("恭喜你，猜对了");
					input.setText("");
				}
				
			}
		});
		panel.getReset().addActionListener(new ActionListener() {
			
			@Override
			public void actionPerformed(ActionEvent e) {
				// TODO Auto-generated method stub
				createRandomInt();
				System.out.println("随机数是：" + randomInt);
				output.setText("请猜一个数字");
			}
		});
		panel.getExit().addActionListener(new ActionListener() {
			
			@Override
			public void actionPerformed(ActionEvent e) {
				// TODO Auto-generated method stub
				System.exit(0);
			}
		});
	}
	/*构造方法*/
	public GuessRandomNumber() {
		init();
	}
	public static void main(String[] args) {
		new GuessRandomNumber();
	}
}
```
<center>ButtonPanel.java</center>

```java

public class ButtonPanel extends JPanel{
	private JButton guess,reset,exit;
	public JButton getGuess() {
		return guess;
	}
	public void setGuess(JButton guess) {
		this.guess = guess;
	}
	public JButton getReset() {
		return reset;
	}
	public void setReset(JButton reset) {
		this.reset = reset;
	}
	public JButton getExit() {
		return exit;
	}
	public void setExit(JButton exit) {
		this.exit = exit;
	}
	public void init() {
		guess = new JButton("猜");
		reset = new JButton("重置");
		exit = new JButton("退出");
		setLayout(new GridLayout(1,3));
		add(guess);
		add(reset);
		add(exit);
		
		
	}
	public ButtonPanel() {
		init();
	}
}

```