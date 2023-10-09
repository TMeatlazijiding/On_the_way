![image](https://github.com/TMeatlazijiding/On_the_way/blob/main/ic%E5%B0%8F%E7%AB%99/image/7jhkow1md1.jpeg)
# cell,pin,net,port之间的关系
~~~
cell就是基本的模块，可以是Verilog中的module或VHDL中的entity，或者综合后的更细粒度的逻辑单元，比如触发器（Flip Flop）、查找表（LUT）、进位链（Carry chain）。
每个cell都有自己的pin，pin是有方向的。cell之间通过net相连。顶层设计中，需要给输入/输出端口（port）分配管脚（package pin），这里就体现了pin与port的区别。
package pin必然位于IO bank之中。

pad是Passivation opening，pad在芯片内部。
ic内部的net要引到ic的外部做封装，但因为线的宽度太细，不能承受焊接的压力，就需要先连接到一块大的金属块，以大金属块作为支撑，这个承受压力的大金属块就是pad。
~~~
