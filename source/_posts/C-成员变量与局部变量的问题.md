---
layout: drafts
title: C#成员变量与局部变量的问题
tags: C#
mathjax: true
categories: 编码
date: 2022-05-08 10:19:45
---


今天遇到个之前觉得很常见的代码写法, 在成员方法中直接调用成员变量, `代码A` 如下:
```C#
	public class Test
	{
		public static Random Random { get; set; }

		public void TestRandom()
		{
			this.Random = new Random();

			if(this.Random == null)
				return;

			var num = this.Random.Next();

			var num1 = this.Random.Next();
		}
    }
```



## 存在的问题
### 问题一, 在多线程下可能存在问题
在多线程下可能存在执行`this.Random == null`之后, 被其他线程将成员变量Random将其改为`null`, 这样将会导致空指针

因此使用局部变量接收成员变量的引用, `代码B` 如下:

```C#
	public class Test
	{
		public Random Random { get; set; }

		public void TestRandom()
		{
			this.Random = new Random();

			var random = this.Random;

			if(random == null)
				return;

			var num = random.Next();

            var num1 = random.Next();
		}
    }
```

### 问题二, 性能上访问成员变量不如访问局部变量快
直接调用成员属性生成的汇编

`var num = this.Random.Next();`

```Assembly
00007FF7E7A85D6D  mov         rcx,qword ptr [rbp+80h]  
00007FF7E7A85D74  call        方法存根对象: TestVariable.Test.get_Random() (07FF7E7A859B0h)  
00007FF7E7A85D79  mov         qword ptr [rbp+38h],rax  
00007FF7E7A85D7D  mov         rcx,qword ptr [rbp+38h]  
00007FF7E7A85D81  mov         rax,qword ptr [rbp+38h]  
00007FF7E7A85D85  mov         rax,qword ptr [rax]  
00007FF7E7A85D88  mov         rax,qword ptr [rax+40h]  
00007FF7E7A85D8C  call        qword ptr [rax+28h]  
00007FF7E7A85D8F  mov         dword ptr [rbp+34h],eax  
00007FF7E7A85D92  mov         ecx,dword ptr [rbp+34h]  
00007FF7E7A85D95  mov         dword ptr [rbp+5Ch],ecx  
```

调用局部变量生成的汇编

`var num = random.Next();`

```Assembly
00007FF7E7A88D27  mov         rcx,qword ptr [rbp+48h]  
00007FF7E7A88D2B  mov         rax,qword ptr [rbp+48h]  
00007FF7E7A88D2F  mov         rax,qword ptr [rax]  
00007FF7E7A88D32  mov         rax,qword ptr [rax+40h]  
00007FF7E7A88D36  call        qword ptr [rax+28h]  
00007FF7E7A88D39  mov         dword ptr [rbp+24h],eax  
00007FF7E7A88D3C  mov         ecx,dword ptr [rbp+24h]  
00007FF7E7A88D3F  mov         dword ptr [rbp+44h],ecx  
```

直接调用成员属性每次都需要去找到对应的成员的地址再调用, 而局部变量则不需要

## 结论
如果在一个方法内需要多次调用一个成员变量/成员属性/静态成员变量, 建议使用一个局部变量存储引用, 通过局部变量去调用。