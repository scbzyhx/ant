# Ant - 创建 WAR 文件

使用 Ant 创建 WAR 文件是极其简单的。这与创建  JAR 文件任务非常类似。 毕竟，WAR 文件与 JAR 文件只是两种不同的 ZIP 文件。

WAR 任务是 JAR 任务的一个扩展，但是其对控制哪些文件进入 WEB-INF/classes 文件夹和生成 web.xml 文件进行了一些很好的补充。WAR 任务对指定 WAR 文件布局是非常有用的。

既然 WAR 任务是 JAR 任务的一个扩展，JAR 任务的所有的属性都适用于 WAR 任务。

<table border="1">
  <tr>
    <th>属性</th>
    <th>描述</th>
  </tr>
  <tr>
    <td>webxml</td>
    <td>web.xml 文件的路径</td>
  </tr>
  <tr>
    <td>lib</td>
    <td>指定什么文件可以进入 WEB-INF\lib 文件夹的一个组</td>
  </tr>
    <tr>
    <td>classes</td>
    <td>指定什么文件可以进入 WEB-INF\classes 文件夹的一个组</td>
  </tr>
    <tr>
    <td>metainf</td>
    <td> 指定生成 MANIFEST.MF 文件的指令</td>
  </tr>
</table>

继续我们的 **Hello World** 传真应用项目，让我们添加一个新的目标来生成 jar 文件。 但是在此之前，我们需要考虑一下 war 任务。 请看下面的例子：

``` java
<war destfile = "fax.war" webxml = "${web.dir}/web.xml">

   <fileset dir = "${web.dir}/WebContent">
      <include name = "**/*.*"/>
   </fileset>
   
   <lib dir = "thirdpartyjars">
      <exclude name = "portlet.jar"/>
   </lib>
   
   <classes dir = "${build.dir}/web"/>
   
</war>
```

按照前面的例子中，web.dir 变量指向源 web 文件夹， 即该文件包含 JSP， css 和 javascript 文件等等。

该 build.dir 变量指向输出文件夹，WAR 的包能在该文件夹下找到。 通常情况下， 类将被绑定到 WAR 文件下的 WEB-INF/classes 文件夹下。

在这个例子中，我们创建了一个名为 fax.war 的 war 文件。WEB.XML 文件可以从 web 源文件中获取。 所有 web 下来自 “WebContent” 的文件都被复制到 WAR 文件中。

WEB-INF/lib 文件夹中填充了来自于第三方 jar 文件夹中的 jar 文件。但是，我们排除了 portlet.jar，因为该 jar 文件已经存在于应用服务器的 lib 文件夹中了。 最后，我们从一个构建目录下的 web 文件夹中复制所有的类，并将复制的类全部放入 WEB-INF/classes 文件夹下。

将一个 war 任务封装到一个 Ant 任务中并运行它。 这将在指定位置创建一个 WAR 文件。

类，库，metainf 和 webinf 目录完全可以进行嵌套以使得他们都能存在于项目结构下分散的文件夹中。 但是最佳的实践建议是，你的 web 项目的 web 内容架构应该与 WAR 文件类似。 传真应用项目的架构就是使用了这个基本原理。

要执行 war 任务，将其封装在一个目标里面，最常见的是，构建目标或者是包目标，然后运行它们。


``` java
<target name="build-war">

   <war destfile="fax.war" webxml="${web.dir}/web.xml">
      <fileset dir="${web.dir}/WebContent">
         <include name="**/*.*"/>
      </fileset>
      
      <lib dir="thirdpartyjars">
         <exclude name="portlet.jar"/>
      </lib>
      
      <classes dir="${build.dir}/web"/>
   </war>
   
</target>
```

在这个文件上运行 Ant 会替我们创建 fax.war 文件。

下述的输出就是运行 Ant 文件的结果：

>C:\>ant build-war
>Buildfile: C:\build.xml

>BUILD SUCCESSFUL
>Total time: 12.3 seconds

该 fax.war 文件当前被放置在输出文件夹中。war 文件的内容如下所示：

>fax.war:
   >+---jsp	：这个文件夹包含了 jsp 文件
   >+---css ：这个文件夹包含了 stylesheet 文件
   >+---js ：这个文件夹包含了  javascript 文件
   >+---images：这个文件夹包含了  image 文件
   >+---META-INF：这个文件夹包含了  Manifest.Mf
   >+---WEB-INF
		   >+---classes ：这个文件夹包含了编译好的类
	      >+---lib ：第三方库和使用程序 jar 文件
	      >WEB.xml ：定义 WAR 包的配置文件 