# Basys2-Verilog-E-Piano
# 1.概述
本项目设计一个可以产生21种音阶的电子琴，由PS2键盘完成输入，在Basys2板识别处理后，产生特定频率声音，最后通过Pmod_AMP模块发出。
本项目演示视频已在b站上传https://www.bilibili.com/video/av22146292/
# 2.设计目标
1.确定键盘按键并发出对应频率的音频信号。
2.能将按下按键对应的音频通过七段码的高两位显示。
3.设计跑马灯，在按键改变时，同步变化跑马灯的移动方向。
4.有曲谱的功能，通过开关可以选择曲目，并将该曲目的音阶根据曲目依次显示在七段码的低两位。
# 3.硬件器材
Basys2板，USB键盘，USB转PS2转接头，Pmod_Amp模块，3.5mm音频接口的音箱
# 4.软件实现
## 4.1概述
采用层次化自顶向下以及模块化的设计方法，顶层模块为E_Piano，用于调用子模块
子模块1:：Basys2_Keyboard用于完成键盘与Basys2板子的数据传输
子模块2：make_melody用于根据已经识别的按键产生特定的音频信号
子模块3：Run_Horse用于增加跑马灯的效果，并实现方向随按键改变
子模块4：Music_Score增加曲谱的功能，并能根据开关选择不同的曲目

## 4.2各模块的具体实现：
   参见code文件夹
# 5.	实验中遇到的问题与难点
1.在Basys2与键盘的数据传输的模块，之前总是会发生数据读取错误的情况，读到一些奇怪的按键码，后来发现是控制数据传输的时钟频率过低，将其提高一倍后数据读取的准确度就非常高了。
2.在产生音阶的模块，由于是根据放在寄存器里的读到的按键通码来发声，寄存器里数据不改变，便会持续发出声音，后来引入了中间变量控制发声，计数0.2s后便改变中间变量，使amp模块不发声才解决。
3.实验中发现21个音阶中有的声音纯净，有的却会伴随着很大的噪声，并且当按键的发声0.2s过后，噪声仍会持续，当时百思不得其解，改程序改了很多次，后来怀疑是AMP模块出问题，将耳机接口接在另外一个接口中，噪音便消失。原来是amp模块的一个耳机接口出问题了，而另外一个是好的。
4.在最开始的时候将键盘Basys2通信与Basys2板发生分开来做，两个部分效果都不错，但一旦合并后便无法发出声音。后来才发现是对Verilog中数的表示形式理解出错了，例如两位十六进制数1C，应该表示为8’h1c而不是2’h1c,最前面的数表示的是二进制数的个数。
# 6.参考资料
宁改娣，金印彬，刘涛_字电子技术与接口技术实验教程_西安电子科技大学出版社_2013年_P201-212
