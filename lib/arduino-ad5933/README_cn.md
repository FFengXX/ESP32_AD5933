#AD5933 Arduino库

这是一个简单的库，用于在Arduino兼容设备上使用AD5933阻抗转换系统。

##公元5933年

AD5933由ADI公司开发。[摘自AD5933页](http://www.analog.com/en/products/rf-microwave/direct-digital-synthesis-modulators/ad5933.html#product-概述）：

>AD5933是一种高精度阻抗转换器系统解决方案，它将板载频率发生器与12位1 MSPS模数转换器（ADC）相结合。频率发生器允许以已知频率激励外部复阻抗。来自阻抗的响应信号由板载ADC采样，离散傅里叶变换（DFT）由板载DSP引擎处理。DFT算法在每个输出频率返回实数（R）和虚数（I）数据字。

>一旦校准，就可以很容易地计算出沿扫描的每个频率点处的阻抗大小和阻抗的相对相位。这是使用可以从串行I2C接口读取的实数和虚数寄存器内容在片外完成的。

换句话说，AD5933可以让你测量某物的复阻抗。

##图书馆

###兼容性

这个库*应该*与任何Arduino兼容设备兼容，尽管可能会有一些变化。它是用RFduino开发和测试的，但我看不出它不能与常规Arduino一起工作的原因。

###缺少功能

虽然这个库足以获得阻抗读数，但我必须承认它有点不完整。以下功能尚未实现（我可能不会实现）：

*配置AD5933励磁范围
*执行校准时，校准相位，以便分析阻抗读数的相位

###安装

只需将整个文件夹“arduino-ad5933”移动到您的“arduino/libraries”文件夹中，通常在您的主目录或文档文件夹中。

###使用方法

####示例

也许了解如何使用该库的最简单方法是查看“examples”目录中的示例代码。安装库后，您可以通过转到“文件”>“示例”>“Arduino-ad5933>ad5933-test”在Arduino编辑器中轻松打开此代码。

本示例将向您展示如何初始设置AD5933并运行频率扫描。有两种方法可以进行频率扫描。第一种方法是使用`frequencySweep（）`函数。此函数非常易于使用，但在批量处理数据之前，您必须等到整个扫描完成，这需要更多的内存，特别是对于大型扫描。另一种方法是直接在I2C级别操纵AD5933，这稍微复杂一些，但允许您在读取后立即处理数据，并且内存开销显著降低。

####简要概述

AD5933.h中有各种各样的函数，可以与许多常数一起使用。每个函数都是静态的，所以一定要在前面包含“AD5933:：”。在这里，我将介绍几个主要问题。

注意：这些函数中的许多都返回布尔值，表示它们的成功。这可能对调试有用。

要重置电路板：`AD5933:：reset（）`

要启用板载温度测量：`AD5933:：enableTemperature（）`

获取温度读数：`double temp=AD5933:：getTemperature（）`

要将时钟源设置为内部：`AD5933:：setInternalClock（true）`或`AD5933:：setClockSource（CTRL_clock_internal）`

要将时钟源设置为外部：`AD5933:：setClockSource（CTRL_clock _external）`

设置频率扫描开始频率：`AD5933:：setStartFrequency（#）`

设置频率扫描增量频率：`AD5933:：setIncrementFrequency（#）`

要设置频率扫描的增量数量：`AD5933:：setNumberIncrements（#）`

要设置PGA增益：`AD5933:：setPGAGain（PGA_gain_X1/PGA_gain_X5）`

要设置电源模式：`AD5933:设置电源模式（power_ON/power_DOWN/POWE_STANDBY）`

要执行校准扫描，即根据已知的参考电阻计算每个频率步长的增益因子（结果存储在gainFactor阵列中，因此请确保其足够大）：`AD5933:：校准（双[]gainFactor，双[]相位，int referenceResistor，int num增量）`

要执行整个频率扫描（结果存储在实数和虚数数组中）：`AD5933:：frequencySweep（int[]real，int[]imag，int-numIncrements）`

读取寄存器：`byte res=AD5933:：readRegister（/*寄存器地址*/）`

这对大多数事情来说已经足够了。查看头文件以了解更多函数和常量。还要查看示例代码，以更好地说明用法。

##参考资料

很多代码都参考了[这个GitHub仓库](https://github.com/WuMRC/drive). 我做了相当多的修改，但如果需要，回购也可能用于其他示例代码。

