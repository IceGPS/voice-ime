# voice-ime

基于本地模型的离线语音输入法。按住说话、松开上屏；说过的每一句话，都会在每天结束时整理成一篇 Markdown 文档。

**核心原则**：离线可用、数据主权——不依赖任何云服务，不上传任何数据。

> 每一句话、每一张照片，都是你的数字资产。数据主权天生属于你——你不自己保管，就只能看着别人拿去变现：不打招呼，还用你的隐私给你喂广告。

## 一、两个目标

1. **本地语音识别，不依赖网络**：ASR 模型在设备端运行，断网可用，录音不出设备。
2. **每日成文**：每天的输入自动整理成一篇文档（`docs/daily/YYYY-MM-DD.md`），可回看、可同步。

## 二、两层结构

```
第一部分 · 日志层（Log）        录音 + 原始转写文字（含语气词、带时间戳），忠实记录，可回溯
第二部分 · 文档层（Document）    把当天转写整理成一篇可读文档（去语气词、加标点、去口语化、按主题分节）
```

**文档层是本项目的重点开发**，日志层复用现有方案。

## 三、各平台仓库

| 平台 | 仓库 | 状态 |
|---|---|---|
| Windows | [IceGPS/CapsWriter-Offline](https://github.com/IceGPS/CapsWriter-Offline) | v5.4 已验收：Qwen3-ASR-1.7B、MP3 录音、JSONL、每日文档、本地大模型总结 |
| Android | [IceGPS/voice-ime-android](https://github.com/IceGPS/voice-ime-android) | V41 已验收：sherpa-onnx + SenseVoice-Small、拼音键盘（全拼/小鹤双拼） |
| Linux | 待定 | 用户决定平台顺序后启动 |
| macOS | 待定 | 用户决定平台顺序后启动 |
| iOS | 待定 | 等 Android 端侧跑稳后排期 |

四平台要求界面和功能保持一致。

## 四、Android 版（V41）功能概览

详见 [Android 仓库 README](https://github.com/IceGPS/voice-ime-android)：

- **极简键盘**（默认）：一行按键 `⇧ ⚙️ 🌐 ⌨️ 🎤 ⌫ ⏎`，把屏幕留给编辑区
  - 🎤 按住中央说话（红底反馈），点其他位置输入空格
  - ⇧ 展开符号区/数字区（可左右滑动，17 个符号 + 22 个数字数学符号）
  - ⌨️ 一键切换拼音全键盘
- **拼音键盘**：QWERTY 手绘键盘，全拼/小鹤双拼（设置页切换），88 万词条离线词库
  - 字母键上滑输数字/符号，候选栏横滑 + `>` 浮层展开
  - 空格键中央 🎤 按住语音输入
- **转写后端**：端侧 sherpa-onnx + SenseVoice-Small INT8，备用局域网 WebSocket（mDNS 自动发现）

## 五、Windows 版（v5.4）功能概览

详见 [CapsWriter-Offline 仓库](https://github.com/IceGPS/CapsWriter-Offline)：

- 基于 CapsWriter-Offline fork，接 Qwen3-ASR-1.7B
- 录音存 MP3（年/月分片），JSONL 转写日志
- 每日文档编译器：去语气词、加标点、按主题分节 → `docs/daily/YYYY/MM/YYYY-MM-DD.md`
- 本地大模型（Qwen3.8-27B）按需启动做内容总结
- 数据经 Syncthing 在多设备间同步

## 六、版本演进思路

- **V8→V23**：从"猜参数修闪退"到"看 logcat 找 FATAL"，确立真机调试纪律；端侧 ASR 路线跑通
- **V24→V26**：数据链路（录音+JSONL+Syncthing）和连接（VAD、mDNS），从"能用"到"好用"
- **V27→V31**：极简键盘输入能力补完（数字符号区、深浅色、AAC 录音、文件名带识别文本）
- **V32→V34**：拼音键盘——用户说"再改进一下就可以替代默认输入法"，补上键盘输入短板；放弃 librime JNI，用纯 Kotlin + RIME 数据
- **V35→V41**：对标搜狗打磨候选词体验，统一双键盘语音输入行为，听用户意见做减法

完整演进见 [Android 仓库 README](https://github.com/IceGPS/voice-ime-android#四版本演进v8--v41)。

## 七、致谢

### 中州韵（RIME）

Android 拼音键盘的词库和双拼方案来自 RIME 社区：rime-ice（雾凇拼音）88 万词条、
小鹤双拼标准映射。只用了 RIME 的**数据**，引擎为独立实现的纯 Kotlin 代码。

### CapsWriter（HaujetZhao）

项目的起点。Windows 版直接基于 CapsWriter-Offline 改造；Android 局域网协议、
录音命名规范、JSONL 日志思路均受其启发。

## 八、目录说明

```
voice-ime/              # 本仓库：项目总览 + 规划文档
├── README.md           # 本文件
├── docs/
│   ├── plan.md         # 总体计划
│   └── shared/
│       └── doc-compiler.md  # 每日文档编译器设计
├── windows/            # Windows 版说明（代码在 CapsWriter-Offline 仓库）
├── android/            # Android 版说明（代码在 voice-ime-android 仓库）
└── shared/             # 跨平台共享设计
```

各平台代码在各自仓库维护，本仓库只保留项目级文档。
