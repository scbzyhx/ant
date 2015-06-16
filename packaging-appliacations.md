﻿#Ant - 封装应用

我们通过 **Hello World** 传真应用，已经琐碎地学习了 Ant 的不同方面的知识了。

现在是时候把我们所学的知识都运用起来创建一个全面和完整的 build.xml 文件了。考虑下面给出的 **build.properties** 和 **build.xml** 文件：

##build.properties

```
eploy.path = c:\tomcat6\webapps
```

##build.xml

```
<?xml version = "1.0"?>

<project name = "fax" basedir = "." default = "usage">

   <property file = "build.properties"/>
   <property name = "src.dir" value = "src"/>
   <property name = "web.dir" value = "war"/>
   <property name = "javadoc.dir" value = "doc"/>
   <property name = "build.dir" value = "${web.dir}/WEB-INF/classes"/>
   <property name = "name" value = "fax"/>

   <path id = "master-classpath">
      <fileset dir = "${web.dir}/WEB-INF/lib">
         <include name = "*.jar"/>
      </fileset>
      <pathelement path = "${build.dir}"/>
   </path>
    
   <target name = "javadoc">
      <javadoc packagenames = "faxapp.*" sourcepath = "${src.dir}" 
         destdir = "doc" version = "true" windowtitle = "Fax Application">
         
         <doctitle><![CDATA[<h1> =  Fax Application  = </h1>]]>
         </doctitle>

         <bottom><![CDATA[Copyright ? 2011. All Rights Reserved.]]>
         </bottom>

         <group title = "util packages" packages = "faxapp.util.*"/>
         <group title = "web packages" packages = "faxapp.web.*"/> 
         <group title = "data packages" packages = "faxapp.entity.*:faxapp.dao.*"/>
      </javadoc>
   </target>

   <target name = "usage">
      <echo message = ""/>
      <echo message = "${name} build file"/>
      <echo message = "-----------------------------------"/>
      <echo message = ""/>
      <echo message = "Available targets are:"/>
      <echo message = ""/>
      <echo message = "deploy    --> Deploy application as directory"/>
      <echo message = "deploywar --> Deploy application as a WAR file"/>
      <echo message = ""/>
   </target>

   <target name = "build" description = "Compile main source tree java files">
      <mkdir dir = "${build.dir}"/>
      
      <javac destdir = "${build.dir}" source = "1.5" target = "1.5" debug = "true"
         deprecation = "false" optimize = "false" failonerror = "true">
         
         <src path = "${src.dir}"/>
         <classpath refid = "master-classpath"/>
         
      </javac>
   </target>


   <target name = "deploy" depends = "build" description = "Deploy application">
      <copy todir = "${deploy.path}/${name}" preservelastmodified = "true">

         <fileset dir = "${web.dir}">
            <include name = "**/*.*"/>
         </fileset>

      </copy>
   </target>


   <target name = "deploywar" depends = "build" description = "Deploy application as a WAR file">
   
      <war destfile = "${name}.war" webxml = "${web.dir}/WEB-INF/web.xml">
         <fileset dir = "${web.dir}">
            <include name = "**/*.*"/>
         </fileset>
      </war>
      
      <copy todir = "${deploy.path}" preservelastmodified = "true">
         <fileset dir = ".">
            <include name = "*.war"/>
         </fileset>
      </copy>
      
   </target>
    

   <target name = "clean" description = "Clean output directories">
      <delete>
         <fileset dir = "${build.dir}">
            <include name = "**/*.class"/>
         </fileset>
      </delete>
   </target>
   
</project>
```

在上面给出的例子中：

<li>我们首先在 build.properties 文件中声明了存放 Tomcat 的 webapp 文件夹的路径，并用变量 **deploy.path** 来保存。</li>
<li> 我们声明一个源文件夹来存放 java 文件，并用变量 **src.dir** 来保存。</li>
<li> 接下来，我们声明另一个源文夹来存放 web 文件，并用变量 **web.dir** 来保存。变量 **javadoc.dir** 用来存储 java 文档，变量 **build.dir** 是用来存储配置输出文件的路径。</li>
<li>然后，我们给这个 web 应用命名，也就是 **fax** 传真。</li>
<li>我们还定义了包含 JAR 文件的基本类路径，在上面给出的项目中也就是： WEB-INF/lib 文件夹。</li>
<li> 我们还将 **build.dir**  中的类文件存放在基本类路径下。</li>
<li> 这个 Javadoc 目标产生项目所需的文档，以及说明目标使用的 javadoc 文档。</li>

上述的例子向我们展示了两个部署目标： **deploy**  和 **deploywar** 。

这个 deploy 目标将文件从 web 目录复制到部署目录，并保存最后修改日期时间戳。这样很有用，特别是当我们将项目部署到服务器上,并且该服务器支持热部署。（释义：所谓热部署，就是在应用正在运行的时候升级软件，却不需要重新启动应用。）

这个 clean 目标清楚所有之前的构建文件。

这个 deploywar 目标构建 war 文件,然后将 war 文件复制到应用程序服务器的部署目录。