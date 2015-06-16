#Ant - 属性文件

当你需要对小部分属性进行设置时，可以选择直接在构建文件中设置。然而，对于个大项目，最好将设置属性的信息存储在一个独立的文件中。
 
存储属性信息在一个独立的文件中将会提供以下好处：
 
<li>它可以让您重复使用相同的构建文件，该文件在不同的执行环境中使用不同的属性设置。例如，构建属性文件在 DEV , TEST , 和 PROD 环境中可以独立地被维护。</li>

<li>当你事先不知道属性的值时（例如，在一个实际的环境中），这样处理是有益的。这样允许您在知道属性值后，在其他环境中执行构建操作。</li>

这里没有硬性规定，但是一般情况下，属性文件都被命名为 **build.properties** ， 并且与 **build.xml** 存放在同一目录层。 你可以基于部署环境 ——比如： **build.properties.dev** 和  **build.properties.test** 创建多个 **build.properties** 文件。

在下面的例子中展示了 **build.xml** 文件和与之相联系的 **build.properties**文件：

## build.xml

```
<?xml version="1.0"?>
<project name="Hello World Project" default="info">

   <property file="build.properties"/>
   
   <target name="info">
      <echo>Apache Ant version is ${ant.version} - You are at ${sitename} </echo>
   </target>
   
</project>
```

## build.properties

```
# The Site Name
sitename=www.tutorialspoint.com
buildversion=3.3.2
```

注意到上面的练习中，**sitename**是一个自定义属性，执行后映射到一个地址为“www.tutorialspoint.com”的网站上。你可以用这种方式声明任意数量的属性。
在上面的例子中，还有一个自定义属性**buildversioin** ，它表明了当前构建的版本号。

除了以上提到的两个属性， Ant 还提供了其他内置属性，在前一章节中已经提到，但是下面我们再一次给出相关属性。

<table>
<tr><td>属性</td><td>描述</td></tr>
<tr><td>ant.file</td><td>表示buildfile的绝对路径。</td></tr>
<tr><td>ant.version </td><td>表示Ant的版本。</td></tr>
<tr><td>basedir   </td><td>表示project基目录的绝对路径。</td></tr>
<tr><td>ant.jave.version </td><td>表示Ant检测到的JDK的版本。</td></tr>
<tr><td>ant.project.name</td><td>表示当前指定的project的名字。</td></tr>
<tr><td>ant.project.default-target</td><td>表示当前项目的默认目标。</td></tr>
<tr><td>ant.project.invoked-targets</td><td>表示被当前项目调用的一系列用逗号分隔开的目标。</td></tr>
<tr><td>ant.core.lib</td><td>表示Ant jar文件的绝对路径。</td></tr>
<tr><td>ant.home</td><td>表示Ant安装的根目录。</td></tr>
<tr><td>ant.library.dir</td><td>表示Ant 函数库，一般情况下 ANT_HOME/lib文件的根目录。</td></tr>
</table>

在这一章节的例子中，用到的Ant 内置属性是**ant.version**属性。