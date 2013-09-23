---
layout: post
title: "自增长运算符"
categories: [skill, c, java]
---

	//源码，C或者Java
	int a=5;
	a = 3 + (a++);
	int b=6;
	b = 4 + (++b);
	//注意，如果没有下面的输出，编译器优化掉上面的代码==！
	//printf("a:%d\tb:%d", a, b);
	//System.out.println("a:"+a+"\tb:"+b);

##Java	结果a:8 b:11
由于Jvm中操作数栈和函数变量表是分开的，所以后自增长操作(a++)都会被‘覆盖’掉，也就是说`a=3+a++`会先将3和a加载到操作数栈，之后直接在局部变量表中实现a的自增运算，之后再将`3+a`的结果写回局部变量表中的a，这样就被覆盖了  

详见源码，字节码，汇编码如下:
<pre>
<code>
plex@plex-R458:~/wksp/coding$ cat Test.java
public class Test {

	public static void main(String[] args) throws Exception {
		int a=5;
		a = 3 + (a++);
		int b=6;
		b = 4 + (++b);
		System.out.println("a:"+a);
		System.out.println("b:"+b);
	}

}
plex@plex-R458:~/wksp/coding$ javap -c Test
Compiled from "Test.java"
public class Test extends java.lang.Object{
public Test();
  Code:
   0:	aload_0
   1:	invokespecial	#1; //Method java/lang/Object."<init>":()V
   4:	return

public static void main(java.lang.String[])   throws java.lang.Exception;
  Code:
   0:	iconst_5 //常量5入操作数栈
   1:	istore_1 //5出栈，放到第一个变量(a)上
   2:	iconst_3 //常量3入操作数栈
   3:	iload_1 //变量a入操作数栈，注意，a=5
   4:	iinc	1, 1 //a自增，修改变量表的值?
   7:	iadd //3,5出栈，和8如栈
   8:	istore_1 //8出栈，放到第一个变量a上
   9:	bipush	6 //常数6入操作数栈
   11:	istore_2 //6出栈，放到第二个变量上
   12:	iconst_4 //常数4入操作数栈
   13:	iinc	2, 1 //第二个变量b自加运算
   16:	iload_2 //加载第二个变量b到操作数栈，注意，b=7!!!
   17:	iadd //7,4出栈，11入栈
   18:	istore_2 //11出栈，放到第二个变量b上
   19:	getstatic	#2; //Field java/lang/System.out:Ljava/io/PrintStream;
   22:	new	#3; //class java/lang/StringBuilder
   25:	dup
   26:	invokespecial	#4; //Method java/lang/StringBuilder."<init>":()V
   29:	ldc	#5; //String a:
   31:	invokevirtual	#6; //Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
   34:	iload_1
   35:	invokevirtual	#7; //Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
   38:	invokevirtual	#8; //Method java/lang/StringBuilder.toString:()Ljava/lang/String;
   41:	invokevirtual	#9; //Method java/io/PrintStream.println:(Ljava/lang/String;)V
   44:	getstatic	#2; //Field java/lang/System.out:Ljava/io/PrintStream; 47:	new	#3; //class java/lang/StringBuilder 50:	dup
   51:	invokespecial	#4; //Method java/lang/StringBuilder."<init>":()V
   54:	ldc	#10; //String b:
   56:	invokevirtual	#6; //Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
   59:	iload_2
   60:	invokevirtual	#7; //Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
   63:	invokevirtual	#8; //Method java/lang/StringBuilder.toString:()Ljava/lang/String;
   66:	invokevirtual	#9; //Method java/io/PrintStream.println:(Ljava/lang/String;)V
   69:	return

}

//下面的汇编码木有看懂的说
plex@plex-R458:~/wksp/coding$/usr/lib/jvm/java-6-openjdk/bin/java  -XX:+UnlockDiagnosticVMOptions -XX:+PrintAssembly -Xcomp -XX:CompileCommand=dontinline,*Test.main -XX:CompileCommand=compileonly,*Test.main Test
OpenJDK Server VM warning: PrintAssembly is enabled; turning on DebugNonSafepoints to gain additional output
CompilerOracle: dontinline *Test.main
CompilerOracle: compileonly *Test.main
Loaded disassembler from hsdis-i386.so
Decoding compiled method 0xb464afc8:
Code:
[Disassembling for mach='i386']
[Entry Point]
[Verified Entry Point]
[Constants]
  # {method} 'main' '([Ljava/lang/String;)V' in 'Test'
  # parm0:	ecx	   = '[Ljava/lang/String;'
  #		   [sp+0x10]  (sp of caller)
  0xb464b0c0: mov	%eax,-0x3000(%esp)
  0xb464b0c7: push   %ebp
  0xb464b0c8: sub	$0x8,%esp		  ;*synchronization entry
										; - Test::main@-1 (line 4)
  0xb464b0ce: mov	$0x18,%ecx
  0xb464b0d3: call   0xb462c720		 ; OopMap{off=24}
										;*getstatic out
										; - Test::main@19 (line 8)
										;   {runtime_call}
  0xb464b0d8: call   0x014807e0		 ;*getstatic out
										; - Test::main@19 (line 8)
										;   {runtime_call}
  0xb464b0dd: hlt	
  0xb464b0de: hlt	
  0xb464b0df: hlt	
[Exception Handler]
[Stub Code]
  0xb464b0e0: jmp	0xb46479e0		 ;   {no_reloc}
[Deopt Handler Code]
  0xb464b0e5: push   $0xb464b0e5		;   {section_word}
  0xb464b0ea: jmp	0xb462dbe0		 ;   {runtime_call}
  0xb464b0ef: .byte 0x0
a:8
b:11
</code>
</pre>

##C(++)	结果a:9 b:11 
C中没有局部变量表，所有的操作都在操作数栈上直接操作，没有Java中的覆盖问题，具体执行流程还不确定


