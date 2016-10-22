## Lab2: DOL实例分析&编程

### 任務一 修改dol中example2，讓三個模塊變爲两个

想要改变程序的结果，则需要改变xml文件，以下是修改过后的代码


```xml
 <?xml version="1.0" encoding="UTF-8"?>
 < processnetwork xmlns="http://www.tik.ee.ethz.ch/~shapes/schema/PROCESSNETWORK" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.tik.ee.ethz.ch/~shapes/schema/PROCESSNETWORK
http://www.tik.ee.ethz.ch/~shapes/schema/processnetwork.xsd" name="example2">

  <variable value="2" name="N"/>

  <!-- instantiate resources -->
  <process name="generator">
    <port type="output" name="10"/>
    <source type="c" location="generator.c"/>
  </process>

  <iterator variable="i" range="N">
    <process name="square">            <!-- square -->
      <append function="i"/>
      <port type="input" name="0"/>
      <port type="output" name="1"/>
      <source type="c" location="square.c"/>
    </process>
  </iterator>

  <process name="consumer">
    <port type="input" name="100"/>
    <source type="c" location="consumer.c"/>
  </process>

  <iterator variable="i" range="N + 1">
    <sw_channel type="fifo" size="10" name="C2">
      <append function="i"/>
      <port type="input" name="0"/>
      <port type="output" name="1"/>
    </sw_channel>
  </iterator>

  <!-- instantiate connection -->
  <iterator variable="i" range="N">
    <connection name="to_square">
      <append function="i"/>
      <origin name="C2">
        <append function="i"/>
        <port name="1"/>
      </origin>
      <target name="square">
        <append function="i"/>
        <port name="0"/>
      </target>
    </connection>

    <connection name="from_square">
        <append function="i"/>
        <origin name="square">
          <append function="i"/>
          <port name="1"/>
        </origin>
        <target name="C2">
          <append function="i + 1"/>
          <port name="0"/>
        </target>
    </connection>
  </iterator>

  <connection name="g_">
    <origin name="generator">
     <port name="10"/>
    </origin>
    <target name="C2">
      <append function="0"/>
      <port name="0"/>
    </target>
  </connection>

  <connection name="_c">
    <origin name="C2">
      <append function="N"/>
      <port name="1"/>
    </origin>
    <target name="consumer">
      <port name="100"/>
    </target>
  </connection>

</processnetwork> ```

其中使用了三个iterator，依次为square，各个组件之间的channel，以及从一个square到另一个square的connection，都是会重复使用的部分。而其中迭代器根据N的大小使用N次，所以只要将N的值从3改为2即可。

dot图如下

![example2](https://raw.githubusercontent.com/Scintillium/Res/master/DOL-RES/example2-dot.png)

### 任务二 修改example1的代码使得其输出三次方数

example的运算在square中执行

代码如下



```c
int square_fire(DOLProcess *p) {
    float i;

    if (p->local->index < p->local->len) {
        DOL_read((void*)PORT_IN, &i, sizeof(float), p);
        i = i*i*i;
        DOL_write((void*)PORT_OUT, &i, sizeof(float), p);
        p->local->index++;
    }

    if (p->local->index >= p->local->len) {
        DOL_detach(p);
        return -1;
    }

    return 0;
}

```
很容易发现在DOL_read和DOL_write中有i的累乘操作，这里我已经将i = i * i；改爲i = i * i * i ;從而實現輸出三次方數。

DOT圖並未發生改變。

![example1](https://raw.githubusercontent.com/Scintillium/Res/master/DOL-RES/example1-dot.png)
