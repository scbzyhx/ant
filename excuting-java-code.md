﻿#Ant - 执行 Java 代码

你可以用 Ant 来执行 Java 代码。在下面的例子中，给出的 java 类文件需要一个参数（管理员的邮箱地址），执行后将发送一封邮件。

```
public class NotifyAdministrator
{
   public static void main(String[] args)
   {
      String email = args[0];
      notifyAdministratorviaEmail(email);
      System.out.println("Administrator "+email+" has been notified");
   }
   public static void notifyAdministratorviaEmail(String email
   { 
	   //......
   }
}
```

这里给出上面 java 类文件需要的 build.xml 构建文件。

```
<?xml version="1.0"?>
<project name="sample" basedir="." default="notify">
   <target name="notify">
      <java fork="true" failonerror="yes" classname="NotifyAdministrator">
         <arg line="admin@test.com"/>
      </java>
   </target>
</project>
```

当 build.xml 被执行后，将会产生下面的输出：

```
C:\>ant
Buildfile: C:\build.xml

notify: [java] Administrator admin@test.com has been notified

BUILD SUCCESSFUL
Total time: 1 second
```

在这里例子中， java 代码完成了一件简单的事情 —— 发送一封邮件。我们也可以运用 Ant 任务来完成这项操作。然而，既然你已经有了这个想法，你可以扩展你的 build.xml 文件来调用 java 代码，完成同样的事情，比如：你还可以加密你的代码。