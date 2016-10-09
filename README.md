## Description

  The distributed operation layer (DOL) is a software development framework to program parallel applications. The DOL allows to specify applications based on the Kahn process network model of computation and features a simulation engine based on SystemC. Moreover, the DOL provides an XML-based specification format to describe the implementation of a parallel application on a multi-processor systems, including binding and mapping.

  ![structure](https://raw.githubusercontent.com/Scintillium/Res/master/DOL-RES/dol-form.png)

## How to install

* 在ubuntu下安装一些必要环境

  `$ sudo apt-get update`

  `$ sudo apt-get install ant`

  `$ sudo apt-get install openjdk-7-jdk`

  `$ sudo apt-get install unzip`

* 在64位系统安装 openjdk 时可能发生错误，可以从[oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html)官网下载`jdk-8u40-linux-x64`

  1. 首先创建目录 ’/usr/lib/java‘

    `$ cd /usr/lib`

    `$ sudo mkdir java`

  1. 然后进入`jdk-8u40-linux-x64`下载文件所在文件夹打开terminal，将文件解压至目标文件夹

    `$ sudo tar zxvf ./jdk-8u40-linux-x64.gz -C /usr/lib/java`

  1. 将jdk重命名位jdk8  

   `$ cd /usr/lib/java`

   `$ sudo mv jdk1.8.0_40/ jdk8`

  1. 配置环境变量

   `gedit ~/.bashrc `

   在文件末尾添加下列语句

   `# enable jdk environment`

     `export JAVA_HOME=/usr/lib/java/jdk8`

     `export JRE_HOME=${JAVA_HOME}/jre`

     `export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib`

     `export PATH=${JAVA_HOME}/bin:$PATH`

    保存并且退出，运行下列语句使得更改生效

    `source ~/.bashrc`

 1. 配置默认JDK

   `$ sudo update-alternatives --install /usr/bin/java java /usr/lib/java/jdk8/bin/java 300`

   `$ sudo update-alternatives --install /usr/bin/javac javac /usr/lib/java/jdk8/bin/javac 300`

 1. 查看当前各种JDK版本和配置

   `$ sudo update-alternatives --config java`

   ![JDK版本](https://raw.githubusercontent.com/Scintillium/Res/master/DOL-RES/jdk.png)

* 下载相关文件

 `$ sudo wget http://www.accellera.org/images/downloads/standards/systemc/systemc-2.3.1.tgz`

 `$ sudo wget http://www.tik.ee.ethz.ch/~shapes/downloads/dol_ethz.zip`

* 解压文件

  新建dol文件夹

  `$ mkdir dol`

  将dolethz.zip解压到 dol文件夹中

  `$ unzip dol_ethz.zip -d dol`

   解压systemc

   `tar -zxvf systemc-2.3.1.tgz`

* 编译systemc

  解压后进入systemc-2.3.1的目录下

  `$	cd systemc-2.3.1`

  新建一个临时文件夹objdir

  `$	mkdir objdir`

  进入该文件夹objdir

  `$	cd objdir`

   运行configure(能根据系统的环境设置一下参数，用于编译)

   `$	../configure CXX=g++ --disable-async-updates`

    ![conffigure](https://raw.githubusercontent.com/Scintillium/Res/master/DOL-RES/systemc.png)

    编译

    `sudo make install`

    然后查看文件目录如下

    `$ cd ..`

    `$ ls`

    ![list](https://raw.githubusercontent.com/Scintillium/Res/master/DOL-RES/systemc-list.png)

    记录当前路径

    `$ pwd`

    例如我得到的路径为`/home/ewan/systemc-2.3.1
`

* 编译DOL

  打开DOL文件夹

  `$ cd ../dol`

  修改build_zip.xml文件

  `gedit build_zip.xml`

  找到下面这段话，就是说上面编译的systemc位置在

   > < property name="systemc.inc" value="YYY/include"/  

  > < property name="systemc.lib" value="YYY/lib-linux/libsystemc.a"/>

  将其中的YYY改为pwd所得结果，注意，64位系统将lib-linux改为lib-linux64

  编译dol

  `ant -f build_zip.xml all`

  如果编译成功的话会显示build successful

* 测试样例

 在上一步build成功情况下

 `cd build/bin/main`

 运行第一个例子

 `ant -f runexample.xml -Dnumber=1`

 成功结果截图如下

 ![success](https://raw.githubusercontent.com/Scintillium/Res/master/DOL-RES/dol-build-success.png)




## Experimental experience
