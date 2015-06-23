# JUnit 集成

JUnit 是一个 Java 语言的单元测试框架。它易于使用且易于扩展。有许多可用的 JUnit 扩展。如果你对 JUnit 不是很熟悉，你可以在 <a href = "www.junit.org">www.junit.org</a> 网址上下载相关手册。

这一章节将向你展示如何用 Ant 来扩展 JUnit 。Ant 直接使用 JUnit 任务。

下面给出 JUnit 任务的相关属性：

<table>
<tr><td>属性</td><td> 描述 </td></tr>
<tr><td>dir</td><td>表示从哪里调用 VM。当 **fork** 被禁用的时候，这个属性将会被忽略。 </td></tr> 
<tr><td>jvm</td><td>表示调用 JVM 的命令。当 **fork** 被禁用的时候，这个属性将会被忽略。 </td></tr>
<tr><td>fork</td><td>表示在独立的 java 虚拟机中运行测试文件。  </td></tr>
<tr><td>errorproperty</td><td>表示当有一个 JUnit 失效的时候，设置属性的名字。 </td></tr>
<tr><td>failureproperty</td><td>表示当有一个 JUnit 失效的时候，设置属性的名字。 </td></tr>
<tr><td>haltonerror</td><td>表示当一个测试有错误的时候，停止执行过程。 </td></tr>
<tr><td>haltonfailure</td><td>表示当有故障发生的时候，停止执行过程。 </td></tr>
<tr><td>printsummary</td><td>表示告知 Ant 展示每个测试例子的简单统计。 </td></tr>
<tr><td>showoutput</td><td>表示告知 Ant 展示输出结果到 log 日志文件或者格式器上。 </td></tr>
<tr><td>tempdir</td><td>表示存放 Ant 将会运用到的临时文件。 </td></tr>
<tr><td>timeout</td><td>表示测试过程耗时太长，超过了设置时间（毫秒级）。 </td></tr>
</table>

让我们继续 **Hello World** 传真应用这个主题，并加入 JUnit 任务。

下面给出的例子展示了一个简单的 JUnit 测试例子的执行过程：

```
<target name="unittest">
   <junit haltonfailure="true" printsummary="true">
      <test name="com.tutorialspoint.UtilsTest"/>
   </junit>
</target>
```

上面给出的例子展示了 com.tutorialspoint.UtilsTest junit 类的执行过程。运行上面的代码，将会看到以下输出：

```
test:
[echo] Testing the application
[junit] Running com.tutorialspoint.UtilsTest
[junit] Tests run: 12, Failures: 0, Errors: 0, Time elapsed: 16.2 sec
BUILD PASSED
```
