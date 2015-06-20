﻿# 构建文件

一般来说，Ant 的构建文件默认为 build.xml，放在项目顶层目录中。然而，并没有限制构建文件必须命名为 bulid.xml，也并不限制放在项目顶层目录中。你可以将构建文件命名为其他名字，也可以将它放在项目的其他地方。
 
这个教程将以简单的方式向你展示如何利用 Apache Ant 来自动地构建和部署项目的过程。在完成本教程的学习以后，你将会发现你已经具备下一阶段学习 Apache Ant 中等水平的专业知识 。
 
对于下面的练习，创建一个文件命名为 build.xml 的文件，存储在你电脑的任意地方 ，并包含一下的内容：

```
<?xml version="1.0"?>
   <project name="Hello World Project" default="info">
   
   <target name="info">
      <echo>Hello World - Welcome to Apache Ant!</echo>
   </target>
   
</project>
```

注意到上面的练习中，在 xml 文件的声明前面没有任何空行或者空格。如果你在写 xml 文件的声明时加入了空行或者空格，执行 ant -build 操作时，将会出现下面的错误信息：  

```
The processing instruction target matching "[xX][mM][lL]" is not allowed.
```  

错误信息的意思是：处理指令目标匹配 "[xX][mM][lL]" 不被允许。所有的构建文件需要包含项目元素（project 标签）和至少一个目标元素（target 标签）。

构建文件的项目元素 <project> 有3个属性：

<table>
<tr><td>属性</td><td>描述</td></tr>
<tr><td>项目名 (name)</td><td>表示项目的名称。（可选）</td></tr>
<tr><td>默认 (default)</td><td>表示构建脚本默认运行的目标，即制定默认的 target。一个项目（project）可以包含多个目标（target）。（必须）</td></tr>
<tr><td>基准目录 (basedir)</td><td>表示当该属性没有指定时，使用 Ant 的构件文件的附目录作为基准目录。（可选）</td></tr>
</table>


一个目标(target)是一系列你想运行的任务(tasks)，运行时看成一个单元。在我们的例子中，我们用一个简单的目标来为用户提供一个有信息的消息。

目标和目标之间可以有依赖关系。举个例子，一个部署（deploy）目标可能依赖于封装（package）目标，而这个封装目标可能又依赖于编译（compile）目标等。依赖关系被表示成依赖属性（depends）。例如：

```
<target name="deploy" depends="package">
  ....
</target>

<target name="package" depends="clean,compile">
  ....
</target>

<target name="clean" >
  ....
</target>

<target name="compile" >
  ....
</target>
```

构建文件的目标元素有以下属性：

| 属性    |    描述 | 
| -------- | --------|
| name目标名  | 表示目标的名称。（必须） |
| depends依赖     |   用于描述target之间的依赖关系，若与多个target存在依赖关系时，需要以“,”间隔。Ant会依照depends属性中target出现的顺序依次执行每个target。被依赖的target会先执行。（可选） |
| description描述| 关于target功能的简单描述。（可选） |
| if 如果    |    用于验证指定的属性是否存在，若不存在，所在target将不会被执行。（可选） |
| unless 除非     | 该属性的功能与if属性的功能正好相反，它也用于验证指定的属性是否存在，若不存在，所在target将会被执行。（可选） |

在上面的例子中 echo 任务主要负责打印消息。在我们的例子中，执行 echo 任务后，打印出“hello world”消息。

为了运行 ant 的构建文件，打开命令提示符并导航到 build.xml建立的文件夹。输入ant info 命令或者 ant 命令。这两种命令都可以运行，因为 info是构建文件的默认目标。你讲会看到下面的输出信息：

```
C:\>ant
Buildfile: C:\build.xml

info: [echo] Hello World - Welcome to Apache Ant!

BUILD SUCCESSFUL
Total time: 0 seconds

C:\>
```
