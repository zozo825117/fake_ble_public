RGB demo 使用说明
=======================


## 概述

* 硬件
XCR3203_module2 + RGB demo主板

[炫彩RGB](http://www.zozo825117.cn:28186/index.php/s/eTMJxEMtBaXZyCq)

使用32BL003智能模块实现红外，蓝牙，2.4G三合一控制。

![](http://www.zozo825117.cn:10080/light_ble_public/pic_bed/light_blue_ir_rgb_demo.png)

演示demo：
1. 主机部分+RGB+IR+XCR3203_module2智能模块
2. 红外遥控器
3. 2.4G遥控demo

## 模块资料
[原理图&BOM](http://www.zozo825117.cn:28186/index.php/s/4gGFWLpkyGygrYH)

## 红外
主机上电，即可实现红外功能，此时，不依赖于32BL003智能模块

## 蓝牙功能
* app下载地址：  
  [light_blue_ir](http://www.zozo825117.cn:10080/light_ble_public/app_release/light_blue_ir.apk)
  > 建议使用PC或手机浏览器打开教程下载。

* 使用：
  1. 打开app
  2. 选择添加遥控器，选择遥控器RGB_00EF
  3. 选择右上角按钮进行扫描(确保主机上电)。
  4. 选择对应模块的mac，就可以连接成功，之后的功能既和红外遥控器功能一致。
  5. 扫描时需要注意打开手机定位  
  `如果问题先参考详细` [蓝牙红外遥控器 使用说明V02](http://www.zozo825117.cn:10080/light_ble_public/light_blue_ir_rgb_demo/%e8%93%9d%e7%89%99%e7%ba%a2%e5%a4%96%e9%81%a5%e6%8e%a7%e5%99%a8%20%e4%bd%bf%e7%94%a8%e8%af%b4%e6%98%8eV02.html)

## 2.4G遥控器
待更新！

## 接线说明

![](http://www.zozo825117.cn:10080/light_ble_public/pic_bed/light_blue_ir_rgb_demo_sch.png)
1. 下载口反了  
2. PA3接红外，D11  
3. arduino D6 RGB  
4. PB4 debug tx  PB5 debug rx  
5. 晶振是12Mhz  

## 其他
- [ ] TODO
