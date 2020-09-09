# Si4735_Radio
Use Si4735/32 <br> 
## Description

1. At Silicon Labs AN332 Rev.1.2 (Si47XX PROGRAMMING GUIDE) Page 2, Note 5 :<br> 
***Si4732-A10 has the same firmware FMRX component and AM_SW_LW RX component as that of Si4735-D60, so
Si4732-A10 is considered as the most recent revision as D60, and the Si4735-D60 related descriptions in Appendix A
and Appendix B also apply to Si4732-A10 if not specified.***<br> 
So Si4735-D60 *"16pin version"* = Si4732-A10.
2. We use Digital Audio output with Pcm5102(i2s), at this time the chip must be input **External reference clock**. There are two solutions. First use mcu to generate *32.768k clock*. Second *Use active crystal*. But they all have shortcomings. Some RF devices are sensitive to harmonics despite it's just 32kHz and mcu may also bring more jitter or Phase noise. At last we decided to use an active crystal and filter.<br> <br> 
![A radio use Si4734 with Rclk input](https://raw.githubusercontent.com/huhu6608/Si4735_Radio/develop/asserts/si4734.JPG)
*<p align="center">A radio use Si4734 with Rclk input</p>*
We can see that this waveform(No load,if add load it will have DC offset.) is a approximately 400mV Vpp Quasi-sine wave include 2nd,3rd,4th,5th haemonic.<br> <br> 
3.耳放芯片使用TPA6111A2，而功放芯片采取NS8002（class AB，BTL）。由于采取外置dac，那么最后输出级一定要足够清晰，例如在很多收音机中使用的cxa1622虽然成本很低而且实现起来相当容易可是牺牲了耳机的底噪。dac最大输出在2Vpp，tpa6111采取一倍放大对于普通16/32Ω耳机来说就有很大音量，而且它的THD随着输出阻抗的上升而减小对于送入后级功放信号也有帮助。一些NS8002有D类功放的功能，这对于收音机的AM模式是否有影响是个未知数，所以还是采取AB类，另外8002一些优势在于它便宜（包括外部BOM），BTL输出代表了他在低压单电源下比单端输出高4倍的功率。<br> 
4.想要实现良好的AM效果，我想应该分两部分来看。首先，中波波段永远是磁棒越长效果越好，而我们想要达到小的体积比然要牺牲中波灵敏度，需要在其中找到平衡，尽可能放下更大的磁棒。其次对于短波来说，不同于普通的pll收音机，似乎这个dsp芯片抗过载能力很低，外接长天线会适得其反带来更多噪声压低广播音频。很多采用这个芯片的收音机加入了前级放大，这里就见仁见智了，毕竟内部是有LNA的许多diyer并没有加前级放大效果也很好。其次就是如刚才所说内置LNA属于高阻输入（大约几kΩ），datasheet中提到中波铁氧体磁棒要采取高阻，那么想要获得最大的效果就要使内部外部阻抗匹配，应当尝试加巴伦（悬空RFGND，实现伪差分）把外部阻抗变换成高阻抗。<br> 
5.电池充电为了实现边冲边放，就要使用带电源路经管理的芯片。对于是否使用线性充电，做了很多考虑，关键在于线性充电且带电源路径管理的芯片少之又少。这与芯片的场景有关系，因为本身线性充电损耗大无法进行大电流操作，通常用在系统耗电少且要求不高的场景。最后选用ETA6002是因为外围比较简单，他的开关频率在3MHz是否会对射频部分产生干扰有待后续测试。<br> <br> 
![A radio use Si4734 with Rclk input](https://raw.githubusercontent.com/huhu6608/Si4735_Radio/develop/asserts/eta6002_shit.jpg)
*<p align="center">ETA6002 datasheet</p>*
![A radio use Si4734 with Rclk input](https://raw.githubusercontent.com/huhu6608/Si4735_Radio/develop/asserts/eta6002_sch.png)
*<p align="center">ETA6002 application sch</p>*
这个的电源路径管理以及ssop8封装是他独特的优势。
