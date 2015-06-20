# 创建 JAR 文件

编译完你的 java 源文件后，接下来就构建 java 存档，例如： JAR 文件。创建 Ant 中的 JAR 文件十分简单，运用 **jar** 任务来生成 **jar** 包。在  **jar** 任务中常用的属性如下所示：

<table>
<tr><td>属性 </td><td> 描述 </td></tr>
<tr><td> basedir</td><td>表示输出 JAR 文件的基目录。默认情况下，为项目的基目录。 </td></tr>
<tr><td>compress</td><td>表示告知 Ant 对于创建的 JAR 文件进行压缩。  </td></tr>
<tr><td>keepcompression</td><td>表示 project 基目录的绝对路径。 </td></tr>
<tr><td>destfile</td><td>表示输出 JAR 文件的名字。 </td></tr>
<tr><td>duplicate </td><td>表示发现重复文件时 Ant 执行的操作。可以是添加、保存、或者是使该重复文件失效。 </td></tr>
<tr><td> excludes </td><td>表示移除的文件列表，列表中使用逗号分隔多个文件。</td></tr>
<tr><td> excludesfile </td><td>与上同，但是使用模式匹配的方式排除文件。 </td></tr>
<tr><td>inlcudes</td><td>与 excludes 正好相反。 </td></tr>
<tr><td>includesfile </td><td> 表示在被归档的文件模式下，打包文件中已有的文件。与 excludesfile 相反。 </td></tr>
<tr><td> update </td><td>表示告知 Ant 重写已经建立的 JAR 文件。 </td></tr>
</table>

继续我们的 **Hello World** 传真应用项目，通过添加一个新的目标 target 来产生 jar 文件。 但是在此之前，让我们先来考虑下面给出的 jar 任务。

```
<jar destfile = "${web.dir}/lib/util.jar"
   basedir = "${build.dir}/classes"
   includes = "faxapp/util/**"
   excludes = "**/Test.class" />
```

这里，**web.dir** 属性指出了 web 源文件的路径。在我们的案例中， web 源文件路径也就是存放 util.jar 的地方。

在我们的案例中，**build.dir** 属性指出了配置文件夹的存储路径，也就是存放 util.jar 类文件的地方。

在上面的代码中，我们利用来自 **faxapp.util** 包中的类文件创建了一个名为 util.jar 的 jar 包。然而，我们排除名字为 Test 的类文件。输出的 jar 文件将会存放在 web 应用的配置文件 lib 中。

如果我们想 util.jar 成为可执行文件，只需在 **Main-Class** 元属性中加入**manifest**.

这样，上面给出的代码，在加入 **Main-Class** 元属性后，可以更新为如下形式：

```
<jar destfile = "${web.dir}/lib/util.jar"
   basedir = "${build.dir}/classes"
   includes = "faxapp/util/**"
   excludes = "**/Test.class">
   
   <manifest>
      <attribute name = "Main-Class" value = "com.tutorialspoint.util.FaxUtil"/>
   </manifest>
   
</jar>
```

为了执行 jar 任务，将它包装在目标 target 中，最常见的情况是，将 jar 任务包装在配置目标或者打包目标中（build 目标或 package 目标），并执行包装后的目标。

```
<target name="build-jar">
   <jar destfile="${web.dir}/lib/util.jar"
      basedir="${build.dir}/classes"
      includes="faxapp/util/**"
      excludes="**/Test.class">

      <manifest>
         <attribute name="Main-Class" value="com.tutorialspoint.util.FaxUtil"/>
      </manifest>

   </jar>
</target>
```

在上述文件上运行 Ant ，就能创建出 util.jar。

上述文件运行 Ant 后，得到以下的输出：

```
C:\>ant build-jar
Buildfile: C:\build.xml

BUILD SUCCESSFUL
Total time: 1.3 seconds
```

最后得到的输出 util.jar 将被存储在输出文件夹中。
