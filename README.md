## Thinkpad T460p opencore monterey + ventura + sonoma

| *项目*     | *工作与否* |                            *备注*                            |
| ---------- | :--------: | :----------------------------------------------------------: |
| CPU变频    |     √      |             系统默认变频， i5-6300HQ, 0x191b0000             |
| SMBios     |     √      |                           MBP13,3                            |
| 🔊声卡      |     √      |            ALC-293(无法使用时可以尝试reset nvram)            |
| 显卡       |     √      |                      Intel HD530 @1440p                      |
| HDMI       |     X      |                         内建屏幕黑屏                         |
| 有线网卡   |     √      |                        Intel I219LM2                         |
| WiFi       |     √      |                           AC 8260                            |
| 📹摄像头    |     √      |                           还没测试                           |
| USB-3.0    |     x      |            速度:最大 5 Gb/秒，右下USB只能使用2.0             |
| 蓝牙       |     √      |            蓝牙鼠标 微软3600 正常使用       |
| 🔋电池      |     √      |                                                              |
| 亮度快捷键 |     √      |                            F5,F6                             |
| 声音快捷键 |     √      |                            F2,F3                             |
| 触摸板     |     √      |                              OK                              |
| HIDPI      |     √      | [one-key-hidpi]([xzhih/one-key-hidpi: Enable macOS HiDPI and have a native setting. (github.com)](https://github.com/xzhih/one-key-hidpi)) |

###### 2023-05-31 新增支持ventura, 除了蓝牙不正常其他都正常

## **升级步骤**

### **将 Skylake GPU 模拟成 Kaby Lake GPU**

config.plist 中添加以下 DeviceProperties

| Devices                   | Key                   | Value                                 | 类型 |
| ------------------------- | --------------------- | ------------------------------------- | ---- |
| PciRoot(0x0)/Pci(0x2,0x0) | `AAPL,ig-platform-id` | 笔记本：`00001B59` 台式机：`00001259` | DATA |
|                           | `device-id`           | `16590000`                            | DATA |

### **将 Skylake CPU 模拟成 Kaby Lake CPU**

如果以上步骤后，电脑无法正常进入系统，说明还需要把 Skylake CPU 模拟成 Kaby Lake CPU。

在 NVRAM 中，UUID 为`7C436110-AB2A-4BBB-A880-FE41995C9F82`下添加启动命令

| 启动参数              | 说明                                         |
| --------------------- | -------------------------------------------- |
| `lilucpu=9`           | 将 Skylake CPU 模拟成 Kaby Lake CPU          |
| `igfxsklaskbl`        | 将 Skylake GPU 模拟成 Kaby Lake GPU          |
| `-disablegfxfirmware` | 防止 KBL iGPU 启动的时候无限循环重试         |
| `-wegnoegpu`          | 禁止一切除了iGPU的其他GPU，比如AMD或者NVIDIA |

### **修改 SMBIOS**

以下 SMBIOS 支持 Ventura

| 平台   | SMBIOS                | 说明     |
| ------ | --------------------- | -------- |
| 笔记本 | MacBookPro14,1 及以后 | 官方支持 |

2024-03 支持sonoma, Intel WIFI 蓝牙 显卡 声音正常
