# 部署应用程序

在之前的章节中，我们已经学会了如何去打包一个应用程序以及怎样将其部署到一个文件夹中。

在这个章节中，我们将要直接将 web 应用程序部署到一个应用服务器的部署文件夹中。随后，我们将添加一些 Ant 目标来启动和停止服务。让我们继续 Hello World 传真 web 应用程序。 这一章节是对之前的章节的一个延续，所有新的组件会用粗体突出显示。

##build.properties
```
# Ant properties for building the springapp

appserver.home=c:\\install\\apache-tomcat-7.0.19
# for Tomcat 5 use $appserver.home}/server/lib
# for Tomcat 6 use $appserver.home}/lib
appserver.lib=${appserver.home}/lib

deploy.path=${appserver.home}/webapps

tomcat.manager.url=http://www.tutorialspoint.com:8080/manager
tomcat.manager.username=tutorialspoint
tomcat.manager.password=secret
```
## build.xml

```
<?xml version="1.0"?>

<project name="fax" basedir="." default="usage">
   <property file="build.properties"/>
   <property name="src.dir" value="src"/>
   <property name="web.dir" value="war"/>
   <property name="javadoc.dir" value="doc"/>
   <property name="build.dir" value="${web.dir}/WEB-INF/classes"/>
   <property name="name" value="fax"/>

   <path id="master-classpath">
      <fileset dir="${web.dir}/WEB-INF/lib">
         <include name="*.jar"/>
      </fileset>
   <pathelement path="${build.dir}"/>
   </path>
    
   <target name="javadoc">
      <javadoc packagenames="faxapp.*" sourcepath="${src.dir}" 
         destdir="doc" version="true" windowtitle="Fax Application">

         <doctitle><![CDATA[<h1>= Fax Application = </h1>]]></doctitle>
         <bottom><![CDATA[Copyright © 2011. All Rights Reserved.]]></bottom>
         <group title="util packages" packages="faxapp.util.*"/>
         <group title="web packages" packages="faxapp.web.*"/>
         <group title="data packages" packages="faxapp.entity.*:faxapp.dao.*"/>

      </javadoc>
   </target>

   <target name="usage">
   <echo message=""/>
   <echo message="${name} build file"/>
   <echo message="-----------------------------------"/>
   <echo message=""/>
   <echo message="Available targets are:"/>
   <echo message=""/>
   <echo message="deploy    --> Deploy application as directory"/>
   <echo message="deploywar --> Deploy application as a WAR file"/>
   <echo message=""/>
   </target>


   <target name="build" description="Compile main source tree java files">
   <mkdir dir="${build.dir}"/>
      <javac destdir="${build.dir}" source="1.5" target="1.5" debug="true"
         deprecation="false" optimize="false" failonerror="true">
         <src path="${src.dir}"/>
         <classpath refid="master-classpath"/>
      </javac>
   </target>


   <target name="deploy" depends="build" description="Deploy application">
      <copy todir="${deploy.path}/${name}" 
         preservelastmodified="true">
         <fileset dir="${web.dir}">
            <include name="**/*.*"/>
         </fileset>
      </copy>
   </target>


   <target name="deploywar" depends="build" description="Deploy application as a WAR file">
      <war destfile="${name}.war" webxml="${web.dir}/WEB-INF/web.xml">
         <fileset dir="${web.dir}">
            <include name="**/*.*"/>
         </fileset>
      </war>
      
      <copy todir="${deploy.path}" preservelastmodified="true">
         <fileset dir=".">
            <include name="*.war"/>
         </fileset>
      </copy>
   </target>
    

   <target name="clean" description="Clean output directories">
      <delete>
         <fileset dir="${build.dir}">
            <include name="**/*.class"/>
         </fileset>
      </delete>
   </target>
```

```
<!-- ============================================================ -->
<!-- Tomcat tasks -->
<!-- ============================================================ -->

<path id="catalina-ant-classpath">
<!-- We need the Catalina jars for Tomcat -->
<!--  * for other app servers - check the docs -->
   <fileset dir="${appserver.lib}">
      <include name="catalina-ant.jar"/>
   </fileset>
</path>

<taskdef name="install" classname="org.apache.catalina.ant.InstallTask">
   <classpath refid="catalina-ant-classpath"/>
</taskdef>
<taskdef name="reload" classname="org.apache.catalina.ant.ReloadTask">
   <classpath refid="catalina-ant-classpath"/>
</taskdef>
<taskdef name="list" classname="org.apache.catalina.ant.ListTask">
   <classpath refid="catalina-ant-classpath"/>
</taskdef>
<taskdef name="start" classname="org.apache.catalina.ant.StartTask">
   <classpath refid="catalina-ant-classpath"/>
</taskdef>
<taskdef name="stop" classname="org.apache.catalina.ant.StopTask">
   <classpath refid="catalina-ant-classpath"/>
</taskdef>

<target name="reload" description="Reload application in Tomcat">
   <reload url="${tomcat.manager.url}"username="${tomcat.manager.username}"
      password="${tomcat.manager.password}" path="/${name}"/>
</target>
</project>
```

在这个例子中，我们已经使用 Tomcat 作为我们应用的服务器。 首先，在构建属性文件中，我们已经定义了一些附加属性。
<li>**appserver.home** 指向 Tomcat 服务器的安装路径</li>
<li>**appserver.lib** 指向 Tomcat 服务器的安装文件下的库文件。</li>
<li>**deploy.path** 变量当前指向 Tomcat 的 web 应用程序文件夹。</li>

在 Tomcat 中的应用程序能通过使用 Tomcat 管理应用程序进行启动和停止。管理应用程序的统一资源定位器（URL），用户名和密码也在 build.properties 文件夹中进行指定。 接下来，我们声明一个新的 CLASSPATH 来包含 **catalina-ant.jar**。 若要通过 Apache Ant 来运行 Tomcat， 这个 jar 文件是必须的。

catalina-ant.jar 提供了下述的任务：
<table>
  <tr>
    <th>属性</th>
    <th>描述</th>
  </tr>
  <tr>
    <td>InstallTask</td>
    <td>安装一个 web 应用程序。 类名字为：  org.apache.catalina.ant.InstallTask </td>
  </tr>
  <tr>
    <td>ReloadTask</td>
    <td>重新安装一个 web 应用程序。类名字为： org.apache.catalina.ant.ReloadTask</td>
  </tr>
    <tr>
    <td>ListTask</td>
    <td>列出所有的 web 应用程序。类名字为： Class Name: org.apache.catalina.ant.ListTask</td>
  </tr>
    <tr>
    <td>StartTask</td>
    <td>启动一个 web 应用程序。类名字为：  org.apache.catalina.ant.StartTask</td>
  </tr>
     <tr>
    <td>StopTask</td>
    <td>停止一个 web 应用程序。类名字为：  org.apache.catalina.ant.StopTask</td>
  </tr>
     <tr>
    <td>ReloadTask</td>
    <td> 重新加载一个无需停止的 web 应用程序。类名字为：org.apache.catalina.ant.ReloadTask</td>
  </tr>
</table>

重载任务需要下列附加参数：
<li>管理应用程序的 URL</li>
<li>重启 web 应用程序的用户名</li>
<li>重启 web 应用程序的密码</li>
<li>重启的 web 应用程序的名字</li>

让我们发出部署 war (**deploy-war**)的命令来复制 web 应用程序到 Tomcat 的 webapps 文件夹中。同时，我们重新加载传真 web 应用程序。下述的输出是运行 Ant 文件的结果。

```
>C:\>ant deploy-war
>Buildfile: C:\build.xml

>BUILD SUCCESSFUL
>Total time: 6.3 seconds

>C:\>ant reload
>Buildfile: C:\build.xml

>BUILD SUCCESSFUL
>Total time: 3.1 seconds
```  

一旦上述的任务被运行，web 应用程序就是已经被部署好且重新加载了的。
