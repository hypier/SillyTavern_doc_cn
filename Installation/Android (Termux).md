---
label: Android (Termux)
---

# Android(Termux) 安装

SillyTavern 可以通过 Termux 在 Android 手机上原生运行。

请参考 ArroganceComplex#2659 的指南：

<https://rentry.org/STAI-Termux>

## 常见错误

### 不支持的平台：android arm LEtime-web
32位 Android 需要一个无法通过 npm 安装的外部依赖。

使用以下命令安装：

`pkg install esbuild`

然后按照指南中的步骤运行。