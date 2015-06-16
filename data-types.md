# Ant - 数据类型


Ant 提供一些预定义的数据类型。不要将术语“数据类型”和那些在编程语言中可用的数据类型相混淆，而是将他们视作一组已经在产品中配置好的服务。

下述的数据类型是由 Apache Ant 提供的。

##文件集

文件集的数据类型代表了一个文件集合。它被当作一个过滤器，用来包括或移除匹配某种模式的文件。

例如，参考下面的代码。这里，src 属性指向项目的源文件夹。

文件集选择源文件夹中所有的 .java 文件，除了那些包含有 'Stub' 单词的文件。能区分大小写的过滤器被应用到文件集上，这意味着名为 Samplestub.java 的文件将不会被排除在文件集之外。

``` java
<fileset dir="${src}" casesensitive="yes">
   <include name="**/*.java"/>
   <exclude name="**/*Stub*"/>
</fileset>
```

##模式集合
一个模式集合指的是一种模式，基于这种模式，能够很容易地过滤文件或者文件夹。模式可以使用下述的元字符进行创建。

<li>**?** －仅匹配一个字符</li>
<li>**\*** －匹配零个或者多个字符</li>
<li>**\*\*** －递归地匹配零个或者多个目录</li>

下面的例子演示了模式集合的使用。

``` java
<patternset id="java.files.without.stubs">
   <include name="src/**/*.java"/>
   <exclude name="src/**/*Stub*"/>
</patternset>
```

该模式集合能够通过一个类似于下述的文件集进行重用：

``` java
<filelist id="config.files" dir="${webapp.src.folder}">
   <file name="applicationConfig.xml"/>
   <file name="faces-config.xml"/>
   <file name="web.xml"/>
   <file name="portlet.xml"/>
</filelist>
```

##文件列表
文件列表数据类型与文件集相类似，除了以下几处不同：
<li>文件列表包含明确命名的文件的列表，同时其不支持通配符。</li>
<li>文件列表数据类型能够被应用于现有的或者还不存在的文件中。</li>

让我们来看一个下述的关于文件列表数据类型的例子。在这个例子中，属性 **webapp.src.folder** 指向该项目中的 web 应用的源文件夹。

``` java
<fileset dir="${src}" casesensitive="yes">
   <patternset refid="java.files.without.stubs"/>
</fileset>
```

##过滤器集合
使用一个过滤器集合数据类型与拷贝任务，你可以在所有文件中使用一个替换值来替换掉一些与模式相匹配的文本。

一个常见的例子就是对一个已经发行的说明文件追加版本号，代码如下：

``` java
<copy todir="${output.dir}">
   <fileset dir="${releasenotes.dir}" includes="**/*.txt"/>
   <filterset>
      <filter token="VERSION" value="${current.version}"/>
   </filterset>
</copy>
```

在这段代码中：
<li>属性 **output.dir** 指向项目的输出文件夹。</li>
<li>属性 **releasenotes.dir** 指向项目的发行说明文件夹。</li>
<li>属性 **current.version** 指向项目的当前版本文件夹。</li>
<li>拷贝任务，顾名思义，是用来将文件从一个地址拷贝到另一个地址。</li>

##路径
**path** 数据类型通常被用来表示一个类路径。各个路径之间用分号或者冒号隔开。然而，这些字符在运行时被替代为执行系统的路径分隔符。

类路径被设置为项目中 jar 文件和类文件的列表，如下面例子所示：

``` java
<path id="build.classpath.jar">
   <pathelement path="${env.J2EE_HOME}/${j2ee.jar}"/>
   <fileset dir="lib">
      <include name="**/*.jar"/>
   </fileset>
</path>
```

在这段代码中：
<li> 属性 **env.J2EE_HOME** 指向环境变量 J2EE_HOME 。</li>
<li> 属性 **j2ee.jar ** 指向在 J2EE 基础文件夹下面的名为 J2EE jar 的文件。</li>