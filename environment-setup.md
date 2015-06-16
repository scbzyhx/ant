# Ant - 环境配置

Apache Ant Ant 是 Apache 软件许可证下发布的。Apache 是一个由一个开源机构认证的完全成熟的开源许可证。
Apache Ant 最新的版本包括了以下的部分：完整的源代码， 类文件和文档，这些内容都可以在[ http://ant.apache.org](https://zh.wikipedia.org/wiki/Markdown)上面找到。

## 安装 Apache Ant
安装 Apache Ant 的前提是你的电脑上已经下载并且安装了 Java 开发工具包（JDK）。如果电脑上没有安装的话，请按照下列的[提示](http://www.tutorialspoint.com/java/java_environment_setup.htm)进行下载和安装。 
<li> 确保设置 JAVA\_HOME 环境变量为你安装 Java 开发工具包的文件夹。</li>
<li>从 [http://ant.apache.org](http://ant.apache.org) 下载库。</li>
<li> 将文件解压到一个方便的地址，如 c:\folder 。可以使用 Winzip,  winRAR, 7-zip 或者其他类似的工具进行解压缩操作。</li>
<li>创建一个新的环境变量，命名为 ANT_HOME ， 该环境变量指向 Ant 的安装文件夹，在这个例子中，该文件夹为 c:\apache-ant-1.8.2-bin。</li>
<li>将 Apache Ant 的批处理文件的路径添加到 PATH 环境变量里。在这个例子中，该路径应为 c:\apache-ant-1.8.2-bin\bin 文件夹。</li>

## 验证 Apache Ant 安装
为了验证你的电脑上是否已经成功安装了 Apache Ant ，你可以在命令提示符中输入 ant。你应该会看到一个与下列相似的输出：
> C:\>ant -version
Apache Ant(TM) version 1.8.2 compiled on December 20 2010

如果你没有看到与上述相类似的输出，请重新检查一下你是否已经正确按照之前的步骤进行了安装。

## 安装 Eclipse
这个教程也包括了 Ant 和 Eclipse 继承开发环境的整合。 因此，如果你还没有安装好 Eclipse， 请下载并安装 Eclipse。

安装 Eclipse 请按照下述步骤：
<li> 从 [www.eclipse.org](www.eclipse.org) 上面下载最新版的 Eclipse 二进制文件。</li>
 <li>解压 Eclipse 二进制文件到一个方便的位置，比如 c:\folder 。</li>
 <li> 从 c:\eclipse\eclipse.exe 处运行 Eclipse 。</li>