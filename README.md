

## 项目概述 / Project Overview

本项目在 **Yahboom K230** 上实现了一个基于摄像头的目标检测、位置追踪及示波器可视化的系统。
通过 **YOLO 模型** 进行目标检测，结合 **指数平滑滤波**（Exponential Smoothing）实现检测框平滑跟踪，并实时将目标中心与摄像头中心的偏移显示在屏幕上，同时通过 UART 将偏移数据发送到外部设备。

项目特点：

* 支持灰度图像和二值化图像处理
* 基于 YOLO 模型的实时目标检测
* 目标位置平滑滤波（指数平滑）
* 示波器波形可视化，显示目标偏移随时间变化
* 支持 UART 通信发送偏移数据
* 支持按键操作切换模式
* 文件1传统视觉，文件2yolo

---

## 功能 / Features

1. **摄像头初始化**：自动设置分辨率 480×640，帧率 30fps，灰度图像处理
2. **显示屏显示**：在 LCD 上显示检测结果、帧率以及示波器波形
3. **目标检测**：

   * YOLO 模型推理
   * 边界框绘制与中心点计算
4. **滤波与平滑**：

   * 指数平滑滤波（Exponential Smoothing）平滑检测框
   * 可替换为卡尔曼滤波
5. **示波器显示**：

   * 绘制 dx、dy 偏移随时间的波形
   * 支持历史数据保存，最大长度 300
6. **UART 通信**：

   * 发送偏移信息到外部设备，格式：`@dx dy#%`
   * 可接收外部指令

---

## 硬件需求 / Hardware Requirements

* Yahboom K230 CanMV
* ST7701 显示屏
* 外部 MCU

---

## 软件依赖 / Software Requirements

* Python (MicroPython)
* cv_lite, media, ybUtils 库
* YOLO 模型文件（kmodel）
* PipeLine 与 DetectionApp 模块

---

## 关键技术点 / Key Features

1. **YOLO 目标检测**
   使用 kmodel 文件进行推理，可处理单目标或多目标，返回边界框、类别和置信度。
2. **指数平滑滤波**
   对检测框进行平滑，提高中心点稳定性：

   ```python
   filtered_box = exp_filter.update(current_box)
   ```
3. **示波器波形显示**
   将 dx、dy 随时间绘制波形：

   ```python
   draw_oscilloscope(pl.osd_img, cx, cy)
   ```
4. **UART 数据通信**
   偏移量发送格式：

   ```
   @dx dy#%
   ```

---

## 示例效果 / Demo

* 检测框实时跟踪目标
* 中心点显示十字标记
* dx、dy 偏移波形随时间更新
* UART 输出偏移数据

---
![e2c659ab875adde0d3b4f2a2adcb1c13](https://github.com/user-attachments/assets/fff2af88-5bb2-484a-86cd-b962314c2431)

<img width="1947" height="1277" alt="屏幕截图 2025-12-07 185041" src="https://github.com/user-attachments/assets/a987fc38-0969-4d64-8346-f4153f02407d" />

