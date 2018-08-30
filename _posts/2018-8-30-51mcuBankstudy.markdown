---
layout:     post
title:      "51内核多bank区域学习"
subtitle:   "core8051-more than 64kB 4BANKs study"
date:       2018-08-30 13:39:00
author:     "song"
header-mask: 0.3
catalog:    true
tags:
    - 51
    - c
    - asm
    - keil
---

# 51内核学习
## 数据类型
   cpu寄存器
   * B寄存器 0xF0H   
   * ACC
   * PSW
   * IP
   * SP-程序指针
   * R0~R7
   * DPL
   * DPH
   * 
   ram内存
   * DATA
   * XDATA
   * IDATA
   * --
   * 外设寄存器XDATA
   flash内存
   * CODE
   * 
## 关于BANK切换
因为单片机地址总线为16位，寻址空间为0xFFFF = 64kB大小，所以单片机程序空间最大为64kB，想写更多的程序就要用到扩展地址线的方式。有3种方法扩展地址总线，
* 使用单片机IO口 两根线，控制外围片选。相当于增加2Bits地址，有4*64kB ROM
* 使用P1口8bits 增加地址总线，作为高8位，可循之24bits地址。
* 使用自定义模式，ROM虽然是片外但是，从外观上来说是封装在芯片上。不用考虑外围电路。使用一个寄存器当作高位地址，实现BANK切换。（TW8836就是这种模式。稍有区别的是芯片没有flash，使用外部spiROM实现）。
### BANK切换方式