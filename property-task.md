# 属性任务

Ant 构建文件是用 XML 编写的，它不能像你喜欢的编程语言那样去声明变量。然而，正如你可能已经想到的，如果允许 Ant 声明变量，如项目名称，项目源目录等，这将是非常有用的。
 
Ant 使用**属性 (property)** 元素来让你能够具体说明属性。这就允许这些属性能够在不同的构建和不同的环境下发生改变。

默认情况下，Ant 提供以下预定义的属性，这些属性都是可以在构建文件中使用的：  

<table>
  <tr>
    <th>属性</th>
    <th>解释</th>
  </tr>
  <tr>
    <td>ant.file</td>
    <td>该构建文件的完整地址</td>
  </tr>
  <tr>
    <td>ant.version</td>
    <td>安装的 Apache Ant 的版本</td>
  </tr>
    <tr>
    <td>basedir</td>
    <td>构建文件的基目录的绝对路径，作为 **project** 元素的 **basedir** 属性</td>
  </tr>
    <tr>
    <td>ant.java.version</td>
    <td> Ant 使用的 JAVA 语言的软件开发工具包的版本</td>
  </tr>
    <tr>
    <td>ant.project.name</td>
    <td>项目的名字，具体声明为 **project** 元素的 **name** 属性</td>
  </tr>
    <tr>
    <td>ant.project.default-target</td>
    <td>当前项目的默认目标</td>
  </tr>
    <tr>
    <td>ant.project.invoked-targets</td>
    <td>在当前项目中被调用的目标的逗号分隔列表</td>
  </tr> <tr>
    <td>ant.core.lib</td>
    <td>Ant 的 jar 文件的完整的地址</td>
  </tr> <tr>
    <td>ant.home</td>
    <td>Ant 安装的主目录</td>
  </tr>
  <tr>
    <td>ant.library.dir</td>
    <td>Ant 库文件的主目录，特别是 ANT_HOME/lib 文件夹</td>
  </tr>
</table>

Ant 也确保系统属性在构建文件中可用，如 file.separator。

除了上述内容以外，用户也可以使用 property 元素定义一些额外的属性。下面的例子就演示了怎样去定义一个叫做 sitename 的属性：

```
<?xml version="1.0"?>
<project name="Hello World Project" default="info">

   <property name="sitename" value="www.tutorialspoint.com"/>
   <target name="info">
      <echo>Apache Ant version is ${ant.version} - You are at ${sitename} </echo>
   </target>
   
</project>
```

在上述的构建文件下运行 Ant 可以产生以下输出：

```
C:\>ant
Buildfile: C:\build.xml

info: [echo] Apache Ant version is Apache Ant(TM) version 1.8.2  
      compiled on December 20 2010 - You are at www.tutorialspoint.com

BUILD SUCCESSFUL
Total time: 0 seconds

C:\>
```
