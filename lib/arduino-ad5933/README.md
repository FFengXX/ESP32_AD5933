# AD5933 Arduino Library

This is a simple library for using the AD5933 impedance convert system with an Arduino compatible device.

## AD5933
The AD5933 is developed by Analog Devices. [From the AD5933 page](http://www.analog.com/en/products/rf-microwave/direct-digital-synthesis-modulators/ad5933.html#product-overview):

>The AD5933 is a high precision impedance converter system solution that combines an on-board frequency generator with a 12-bit, 1 MSPS, analog-to-digital converter (ADC). The frequency generator allows an external complex impedance to be excited with a known frequency. The response signal from the impedance is sampled by the on-board ADC and a discrete Fourier transform (DFT) is processed by an on-board DSP engine. The DFT algorithm returns a real (R) and imaginary (I) data-word at each output frequency.

>Once calibrated, the magnitude of the impedance and relative phase of the impedance at each frequency point along the sweep is easily calculated. This is done off chip using the real and imaginary register contents, which can be read from the serial I2C interface.

In other words, the AD5933 lets you measure the complex impedance of something.

## Library

### Compatibility
This library *should* be compatible with any Arduino compatible device, albeit perhaps with some changes. It was developed and tested with an RFduino, but I do not see a reason why it would not work with a regular Arduino.

### Missing Features
While the library is enough to get impedance readings, I must admit that it is somewhat incomplete. The following features are yet to be implemented (and likely will not be implemented by me):

* Configure the AD5933 excitation range
* When performing calibration, calibrate the phase such that the phase of impedance readings can be analyzed

### Installation
Simply move the entire folder `arduino-ad5933` to your `Arduino/libraries` folder, usually in your home directory or documents folder.

### Usage

#### Example
Perhaps the easiest way to see how to use the library is to look at the example code in the `examples` directory. Once you install the library, you can easily open this code in the Arduino editor by going to `File > Examples > arduino-ad5933 > ad5933-test`.

This example will show you how to initially setup the AD5933 and run frequency sweeps. There are two methods for doing a frequency sweep. The first is by using the `frequencySweep()` function. This function is very easy to use, but you have to wait until the entire sweep is complete before handling data in bulk and this requires more memory, especially for large sweeps. The other method is by manipulating the AD5933 at the I2C level directly, which is slightly more complex, but allows you to handle data immediately after a reading and has significantly lower memory overhead.

#### Brief Overview
There are an assortment of functions in `AD5933.h` that can be used with numerous constants. Each one of the functions are static, so be sure to include `AD5933::` in front. Here I cover a few of the main ones.

NOTE: Many of these functions return booleans indicating their success. This may be useful for debugging.

To reset the board: `AD5933::reset();`

To enable on-board temperature measurement: `AD5933::enableTemperature()`

To get a temperature reading: `double temp = AD5933::getTemperature()`

To set the clock source to internal: `AD5933::setInternalClock(true)` OR `AD5933::setClockSource(CTRL_CLOCK_INTERNAL)`

To set the clock source to external: `AD5933::setClockSource(CTRL_CLOCK_EXTERNAL)`

To set the frequency sweep start frequency: `AD5933::setStartFrequency(#)`

To set the frequency sweep increment frequency: `AD5933::setIncrementFrequency(#)`

To set the frequency sweep number of increments: `AD5933::setNumberIncrements(#)`

To set the PGA gain: `AD5933::setPGAGain(PGA_GAIN_X1/PGA_GAIN_X5)`

To set the power mode: `AD5933:setPowerMode(POWER_ON/POWER_DOWN/POWER_STANDBY)`

To perform a calibration sweep, which computes gain factor for each frequency step based on a known reference resistance (the results are stored in the gainFactor array, so make sure this is large enough): `AD5933::calibration(double[] gainFactor, double[] phase, int referenceResistor, int numIncrements)`

To perform an entire frequency sweep (the results are stored in the real and imaginary arrays): `AD5933::frequencySweep(int[] real, int[] imag, int numIncrements)`

To read a register: `byte res = AD5933::readRegister(/* register address */)`

That is sufficient for most things. Check out the header file for more functions and constants. Also check out the example code for better illustrations of usage.

## Reference
A lot of code was reference from [this GitHub repo](https://github.com/WuMRC/drive). I made a fair amount of modifications, but the repo can also probably be used for additional sample code if needed.





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