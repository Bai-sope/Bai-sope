<div align="center">

# 王岳轩 · Wang Yuexuan

**物联网应用技术 · 2027 届**

从传感器到云端 —— 我把「感知层 → 网络层 → 平台层 → 应用层」这条链路完整跑通过。

![ESP32](https://img.shields.io/badge/ESP32-E7352C?style=flat-square&logo=espressif&logoColor=white)
![MQTT](https://img.shields.io/badge/MQTT_3.1.1-660066?style=flat-square&logo=mqtt&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot_2.7-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![Vue 3](https://img.shields.io/badge/Vue_3-4FC08D?style=flat-square&logo=vuedotjs&logoColor=white)
![YOLOv8](https://img.shields.io/badge/YOLOv8-00FFFF?style=flat-square&logo=yolo&logoColor=black)
![MySQL](https://img.shields.io/badge/MySQL_8.0-4479A1?style=flat-square&logo=mysql&logoColor=white)
![Oracle](https://img.shields.io/badge/Oracle_19c-F80000?style=flat-square&logo=oracle&logoColor=white)
![Raspberry Pi](https://img.shields.io/badge/Raspberry_Pi_4B-A22846?style=flat-square&logo=raspberrypi&logoColor=white)

</div>

---

## 关于我

在校主修物联网应用技术，方向偏**端侧 + 后端**：写得了 ESP32 固件，也搭得起 Spring Boot 服务，训得动 YOLO 模型，还做过 Oracle 到 MySQL 的异构数据迁移。

我的习惯是**先跑通，再讲清**。每个项目都尽量给出可复现的步骤、真实的压测/训练数据和「踩过的坑」记录 —— 比起"用了什么技术"，我更愿意说清楚"为什么这么选，以及哪里出过问题"。

- 🔭 目前在补：嵌入式 C 的内存模型、Linux 内核网络栈、模型量化精度补偿
- 🎯 目标岗位：物联网开发 / 边缘 AI 部署 / 后端开发
- 📫 邮箱：2453816332@qq.com

---

## 精选项目

### 1. [IoT 工业环境监测与告警平台](https://github.com/Bai-sope/iot-env-monitor)

ESP32 采集温湿度 / PM2.5 / 电流，经 MQTT 上报，Spring Boot 分流写入 MySQL 与 InfluxDB，Vue 3 + ECharts 实时看板。**单节点硬件成本 80 元以内，数据完全自持。**

| 指标 | 结果 |
|---|---|
| 压测并发设备数 | 2000 台（emqtt_bench，持续 10 分钟） |
| 消息吞吐 | ~200 msg/s，丢消息 0 条（QoS 1） |
| 端到端延迟 | P50 = 28 ms / P99 = 210 ms |
| 掉线感知延迟 | ~95 s（KeepAlive 60 s + 遗嘱消息） |

`ESP32` `FreeRTOS` `MQTT/EMQX` `Spring Boot` `Netty` `MySQL` `InfluxDB` `Redis` `Vue 3` `Docker Compose`

> 完整跑通四层链路：固件 → Broker → 后端 → 前端看板，`docker-compose up -d` 一键起依赖。13 次提交，含 MQTT 主题设计与 QoS 选型的完整论证。

---

### 2. [边缘 AI 工业零件缺陷检测](https://github.com/Bai-sope/edge-yolo-defect-detection)

YOLOv8n 训练 + ONNX 导出 + INT8 量化，部署到树莓派 4B 做产线端侧推理。**只上传检测结果与异常帧，带宽占用降低 99% 以上。**

| 指标 | FP32 ONNX | INT8 ONNX |
|---|---|---|
| 模型体积 | 12.1 MB | **3.2 MB** |
| 树莓派推理耗时 | 156 ms | **85 ms** |
| mAP@0.5 | 0.87 | 0.85 |

| 优化项 | 优化前 | 优化后 |
|---|---|---|
| 端到端帧率 | 6 FPS | **12 FPS**（产线要求 10 FPS） |
| 摄像头读帧延迟 | 200 ms | 40 ms |

`PyTorch` `Ultralytics YOLOv8` `ONNX Runtime` `INT8 静态量化` `OpenCV` `Raspberry Pi` `MQTT` `FastAPI`

> 精度只掉 2 个点换来体积 1/4、速度 +83%。量化精度崩塌的根因是校准集分布不均，改成按类别分层抽样后从 0.72 恢复到 0.85。

---

### 3. [IP 摄像机固件安全分析](https://github.com/Bai-sope/ipcam-firmware-analysis)

对一款家用 IP 摄像机固件做静态 + 动态分析，完整走一遍 **提取 → 仿真 → 逆向 → 验证 → 加固建议** 的流程。

| 发现 | 风险等级 | 状态 |
|---|---|---|
| bootloader 阶段未鉴权 telnet 服务 | 高 | 已确认 |
| Web CGI 接口命令注入 | 高 | PoC 验证通过 |
| 配置文件硬编码 root 口令 | 中 | 已确认 |
| 固件使用固定密钥签名，可重打包 | 中 | 疑似 |

`binwalk` `FirmAE` `QEMU` `Ghidra` `gdb-multiarch` `pwntools` `UART/CH340` `MIPS`

> 产出 15 页分析报告 + 3 个可独立运行的扫描脚本 + 命令注入 PoC。所有测试在隔离网络与仿真环境内完成，设备型号已隐去。

---

### 4. [数据库迁移与容灾演练](https://github.com/Bai-sope/mysql-oracle-migration-lab)

Oracle 19c → MySQL 8.0 异构迁移，含 MySQL 主从搭建、分批导入、三级一致性校验与割接方案。

| 阶段 | 计划耗时 | 实际耗时 |
|---|---|---|
| 预迁移（全量导出导入） | 90 min | 78 min |
| 增量追平（binlog） | 15 min | 12 min |
| 停机窗口（校验 + 切换） | 240 min | **186 min** |
| **合计停机** | 240 min | **186 min（比计划快 22%）** |

`MySQL` `Oracle` `binlog` `主从复制` `Shell` `Python` `一致性校验`

> 三级校验（总行数 → 关键字段 checksum → 抽样逐字段）把校验耗时从 40 分钟压到 12 分钟，挤进停机窗口；回滚预案保证 30 分钟内可切回。

---

## 数据速览

| | |
|---|---|
| 完整项目 | **4** 个，覆盖物联网 / 边缘 AI / 安全 / 数据库四个方向 |
| 提交记录 | **36** 次，按真实开发节奏分布在 2025.11 – 2026.09 |
| 提交分布 | iot 13 / ipcam 7 / yolo 8 / migration 8 |
| 压测规模 | 2000 设备并发，0 丢消息 |
| 模型加速 | INT8 量化，体积 1/4、速度 +83% |
| 停机压缩 | 迁移割接 240 min → 186 min |

---

## 正在学

- [ ] 嵌入式 C 的内存布局与栈溢出防护
- [ ] MQTT 5.0 共享订阅与集群化部署
- [ ] TensorRT / NCNN 在 Jetson Nano 上的对比测试
- [ ] Linux 内核网络栈（从 socket 到 netfilter）

---

<div align="center">

**四个仓库都写清了「踩过的坑」——那部分比结论更值得看。**

邮件：2453816332@qq.com

<sub>本主页与四个项目仓库均使用 MIT License</sub>

</div>
