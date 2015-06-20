# 生成文档

文档在任何项目中都是必须的。文档对一个项目的维护起了至关重要的作用。 通过使用内置的 Javadoc 工具，使用 Java 生成文档变得更加容易。Ant 通过按需生成文档使得这个步骤甚至变得更简单。

如你所知，javadoc 工具具有高度的灵活性，而且其还允许进行一些配置。Ant 通过使用 javadoc 任务的方式来公开这些配置选项。如果你对 javadoc 不熟悉的话，我们建议你先看一下[ Java 文档教程](http://www.tutorialspoint.com/java/java_documentation.htm)。

下述的章节列出了在 Ant 中最常使用的 javadoc 的选项。

## 属性  

源包括源路径，源路径引用或者源文件三个属性。
<li>源路径 (sourcepath) 指向源文件所在的文件夹，例如： src 文件夹。</li>
<li>源路径引用 (sourcepathref) 指向由该路径属性引用的路径，例如：delegates.src.dir 。</li>
<li>源文件 (sourcefiles) 在你想指定单独的文件时使用，比如指定一个逗号分隔列表。</li>

目标路径是通过使用 destdir 文件夹来指定的，例如 build.dir 。

你能够通过指定应被包括的包的名字来过滤 javadoc 任务。 这可以通过使用 packagenames 属性实现， 即一个以逗号分隔的包文件列表。

你可以过滤 javadoc 过程以只显示公有的，私有的，包或者被保护的类和成员。 这些可以通过使用 private，public，package 和 protected 属性实现。

你也可以通过使用相应的属性来告诉 javadoc 任务去包含作者和版本信息。

你也可以使用 group 属性将所有的包组织在一起，以使得他们更易被操作。


## 将上述内容集中到一起 

让我们继续我们的主题，Hello world 传真应用程序。让我们给我们的传真应用项目添加一个文档目标。

下面给出的例子是我们在项目中使用的 javadoc 任务。在这个例子中，我们指定 javadoc 去使用 src.dir 作为源目录，doc 作为目标。

我们还定制窗口标题，标题，以及显示在 java 文档页上的页脚信息。

此外，我们还创建了三个组：
<li>为源文件夹中的实用工具类创建了一个组，</li>
<li>为用户接口的类创建了一个组，并且</li>
<li>为数据库相关的类创建了一个组。</li>

您可能会注意到，数据包组含有两个包 -- faxapp.entity 和 faxapp.dao 。

```
<target name = "generate-javadoc">
   <javadoc packagenames="faxapp.*" sourcepath="${src.dir}" 
      destdir = "doc" version = "true" windowtitle = "Fax Application">
      
      <doctitle><![CDATA[= Fax Application =]]></doctitle>
      
      <bottom>
         <![CDATA[Copyright © 2011. All Rights Reserved.]]>
      </bottom>
      
      <group title = "util packages" packages = "faxapp.util.*"/>
      <group title = "web packages" packages = "faxapp.web.*"/>
      <group title = "data packages" packages = "faxapp.entity.*:faxapp.dao.*"/>
   </javadoc>
   
   <echo message = "java doc has been generated!" />
</target>
```

让我们运行 javadoc Ant 任务。它将生成 java 文档文件，并将这些文件放置于 doc 文件夹中。

当执行 javadoc 目标时，其产生以下的输出：

```
>C:\>ant generate-javadoc
>Buildfile: C:\build.xml

>java doc has been generated!

>BUILD SUCCESSFUL
>Total time: 10.63 second
```

java 文档文件现在出现在 doc 文件夹中。

通常情况下，javadoc 文件作为发行版或者包目标的一部分。
