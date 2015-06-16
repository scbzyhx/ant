# Ant - 拓展 Ant

Ant 带有一组预定义的任务，但是你可以创建自己的任务，如下面的例子所示。

定制 Ant 任务应拓展 org.apache.tools.ant.Task 类，同时也应该拓展 execute() 方法。下面是一个简单的例子：

``` java
package com.tutorialspoint.ant;

import org.apache.tools.ant.Task;
import org.apache.tools.ant.Project;
import org.apache.tools.ant.BuildException;

public class MyTask extends Task {
   String message;
   public void execute() throws BuildException {
      log("Message: " + message, Project.MSG_INFO);
   }
   
   public void setMessage(String message) {
      this.message= message;
   }
}
```

为了运行定制的任务，你需要添加下列内容到  Hello World 传真 web 应用程序中：
``` java
<target name="custom">
   <taskdef name="custom" classname="com.tutorialspoint.ant.MyTask" />
   <custom message="Hello World!"/>
</target>
```

执行上述的定制任务后将打印出一条信息为： 'Hello World!'

>c:\>ant custom
>test:
>[custom] Message : Hello World!
>elapsed: 0.2 sec
>BUILD PASSED

这只是一个简单的例子，你可以使用 Ant 的能力去做任何你想提高你的构建和部署过程的事情。
