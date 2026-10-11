# voice-ime

基于 Qwen3-ASR 的本地离线语音输入法。

按住说话、松开上屏；说过的每一句话，都会在每天结束时整理成一篇 Markdown 文档。

## 两个目标

1. **本地语音识别，不依赖网络**：Qwen3-ASR 模型在设备端运行，断网可用，录音不出设备。
2. **每日成文**：每天的输入自动整理成一篇文档（`docs/daily/YYYY-MM-DD.md`），可回看、可同步。

## 两层结构

```
第一部分 · 日志层（Log）        录音 + 原始转写文字（含语气词、带时间戳），忠实记录，可回溯
第二部分 · 文档层（Document）    把当天转写整理成一篇可读文档（去语气词、加标点、去口语化、按主题分节）
```

日志层 CapsWriter 已有，本项目不重做；**文档层是本项目重点开发**。

## 路线（严格分阶段）

- **第一阶段 · Windows**：fork [CapsWriter-Offline](https://github.com/HaujetZhao/CapsWriter-Offline)，
  接 Qwen3-ASR-1.7B，验证全部功能（断网全链路、转写质量、文档编译器、编辑区自适应高度、稳定性），
  验收通过后才进入第二阶段。
- **第二阶段 · Android**：自研输入法（`InputMethodService` + `RECORD_AUDIO`，键盘内直接录音），
  先用 Qwen3-ASR-0.6B 打通链路，复用同一套文档编译器。

详细计划见 [`docs/plan.md`](docs/plan.md)。

## 仓库结构

```
windows/          第一阶段：CapsWriter fork + Windows 验证
android/          第二阶段：Android 输入法（待第一阶段验收后启动）
shared/           两端共用：文档编译器契约与规则
docs/             项目文档
```

## 状态

- [x] 方案设计
- [ ] 第一阶段：Windows 验证
- [ ] 第二阶段：Android 输入法
