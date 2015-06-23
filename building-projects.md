# 构建项目

现在我们已经学习了 Ant 的数据类型，是时候在实际过程中运用所学知识了。在这一章节中，我们将会构建一个项目。这一章节的目的是创建一个 Ant build 文件，该文件能够编译 Java 源文件和将这些类文件存储在 WEB-INF\classes 文件夹下。

考虑接下来构建项目的结构：

- 数据脚本存储在 **db** 文件夹中。
- java 源文件存储在 **src** 文件夹中。
- images (图像)，js (JavaScript 脚本)，style (css 层叠样式表)存储在 **war** 文件夹中。
- JSPs 文件存储在 **jsp** 文件夹中。
- 第三方的 jar 文件存储在 **lib** 文件夹中。
- java 类文件存储在 **WEB-INF\classes** 文件夹中。
 
学习完本教程的剩余部分后，就能知道这个项目是一个 **Hello World** 传真应用。

```
C:\work\FaxWebApplication>tree
Folder PATH listing
Volume serial number is 00740061 EC1C:ADB1
C:.
+---db
+---src
.  +---faxapp
.  +---dao
.  +---entity
.  +---util
.  +---web
+---war
   +---images
   +---js
   +---META-INF
   +---styles
   +---WEB-INF
      +---classes
      +---jsp
      +---lib
```

下面给出上述项目的 build.xml 文件的内容。让我们来一条语句接一条语句地来分析它。

```
<?xml version="1.0"?>
<project name="fax" basedir="." default="build">
   <property name="src.dir" value="src"/>
   <property name="web.dir" value="war"/>
   <property name="build.dir" value="${web.dir}/WEB-INF/classes"/>
   <property name="name" value="fax"/>

   <path id="master-classpath">
      <fileset dir="${web.dir}/WEB-INF/lib">
         <include name="*.jar"/>
      </fileset>
      <pathelement path="${build.dir}"/>
   </path>

   <target name="build" description="Compile source tree java files">
      <mkdir dir="${build.dir}"/>
      <javac destdir="${build.dir}" source="1.5" target="1.5">
         <src path="${src.dir}"/>
         <classpath refid="master-classpath"/>
      </javac>
   </target>
 
   <target name="clean" description="Clean output directories">
      <delete>
         <fileset dir="${build.dir}">
            <include name="**/*.class"/>
         </fileset>
      </delete>
   </target>
</project>
```

首先，让我们来声明一些源文件，web 文件和构建文件的一些属性信息。

```
<property name="src.dir" value="src"/>
<property name="web.dir" value="war"/>
<property name="build.dir" value="${web.dir}/WEB-INF/classes"/>
```

在上面的例子中：

- **src.dir** 表示这个项目的源文件目录，也就是存储 java 文件的地方。
- **web.dir** 表示这个项目的 web 文件目录，也就是存储 JSPs 文件，web.xml，css，javascript 以及其它与 web 相关的文件的地方。
- **build.dir** 表示该项目的输出文件。

属性也可以引用其它属性。在上面的例子中，**build.dir** 属性引用了 **web.dir** 属性。

在上面的例子中，**src.dir** 就是项目源文件存放的地方。

我们项目的默认目标是编译目标。但是首先让我们来看一下 **clean** 目标。

**clean** 目标，就像它的名字所表明的意思一样，删除构建文件夹中的所有文件。

```
<target name="clean" description="Clean output directories">
   <delete>
      <fileset dir="${build.dir}">
         <include name="**/*.class"/>
      </fileset>
   </delete>
</target>
```  

控制类路径 (master-classpath) 保存类路径的相关信息。在这种情况下，它包含了构建文件夹和 jar 文件夹中的所有的类文件。

```
<path id="master-classpath">
   <fileset dir="${web.dir}/WEB-INF/lib">
      <include name="*.jar"/>
   </fileset>
   <pathelement path="${build.dir}"/>
</path>
```

最后，构建目标构建这些文件。首先，我们创建一个构建目录，如果该目录不存在，我们就执行 javac 命令（具体以 jdk 1.5 作为我们目标的编译环境）。 我们对 javac 任务提供源文件夹和类路径，并且通过执行 javac 任务将类文件存放在构建文件夹中。

```
<target name="build" description="Compile main source tree java files">
   <mkdir dir="${build.dir}"/>
   <javac destdir="${build.dir}" source="1.5" target="1.5" debug="true"
      deprecation="false" optimize="false" failonerror="true">
      <src path="${src.dir}"/>
      <classpath refid="master-classpath"/>
   </javac>
</target>
```

在这个文件上执行 Ant，编译 java 源文件，并将编译后的类文件存放在构建文件夹的地方。

运行 Ant 文件后，能看到以下输出：

```
C:\>ant
Buildfile: C:\build.xml

BUILD SUCCESSFUL
Total time: 6.3 seconds
```

文件被编译后，将存储在 **build.dir** 文件夹中。
