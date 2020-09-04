# Si4735_Radio
Use Si4735/32 <br> 
## Description

1. At Silicon Labs AN332 Rev.1.2 (Si47XX PROGRAMMING GUIDE) Page 2, Note 5 :<br> 
***Si4732-A10 has the same firmware FMRX component and AM_SW_LW RX component as that of Si4735-D60, so
Si4732-A10 is considered as the most recent revision as D60, and the Si4735-D60 related descriptions in Appendix A
and Appendix B also apply to Si4732-A10 if not specified.***<br> 
So Si4735-D60 ≈ Si4732-A10, Maybe just silkmask is diferent.
2. We use Digital Audio output with Pcm5102(i2s), at this time the chip must be input **External reference clock**. There are two solutions. First use mcu to generate *32.768k clock*. Second *Use active crystal*. But they all have shortcomings. Some RF devices are sensitive to harmonics despite it's just 32kHz and mcu may also bring more jitter or Phase noise. At last we decided to use an active crystal and filter.<br> 
![A radio use Si4734 with Rclk input](https://raw.githubusercontent.com/huhu6608/Si4735_Radio/master/Photo/si4734.JPG)
*<p align="center">A radio use Si4734 with Rclk input</p>*
We can see that this waveform(No load,if add load it will have DC offset.) is a approximately 400mV Vpp Quasi-sine wave include 2nd,3rd,4th,5th haemonic.