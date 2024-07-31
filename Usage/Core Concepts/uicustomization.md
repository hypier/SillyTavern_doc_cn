# 用户界面自定义

### UI 自定义

#### 消息声音

要在收到来自机器人的新消息时播放您自己的自定义声音，请替换 SillyTavern 文件夹中的以下 MP3 文件：

`public/sounds/message.mp3`

音量为 80%。

如果启用了“仅背景声音”选项，则声音仅在 SillyTavern 窗口**未聚焦**时播放。

### UI 颜色

您可以使用颜色选择器和滑块以多种方式自定义 UI。

您还可以保存预设并与其他用户共享（保存在 `/data/<user-handle>/themes/` 中）。

### 高级用户选项

#### 公式渲染

启用数学公式渲染，使用 [showdown-katex](https://obedm503.github.io/showdown-katex/) 包。

支持以下格式规则：

##### LaTeX 语法

```
$$ formula goes here $$
```

##### Asciimath 语法

```
formula goes here $
```

更多信息： [KaTeX](https://katex.org/)