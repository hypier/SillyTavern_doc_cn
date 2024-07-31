# 稳定扩散

## 图像生成扩展

使用本地或基于云的 Stable Diffusion API 生成图像。
通过聊天输入框中的 `/sd (anything_here)` 命令也支持免费模式。
最常见的 Stable Diffusion 生成设置可以在 SillyTavern UI 中进行自定义。

## 生成模式

| 魔杖菜单项          | 斜杠命令参数          | 描述                                           | 备注                                   |
| :----------------- | :--------------------- | :--------------------------------------------- | :------------------------------------ |
| "你自己"           | `you`                  | 当前角色的全身肖像。                          | -                                     | 
| "你的脸"           | `face`                 | 当前角色的特写肖像。                          | 强制肖像纵横比。                       |
| "我"               | `me`                   | 用户角色的肖像。                              | -                                     |
| "整个故事"         | `scene`                | 聊天事件的视觉回顾。                          | -                                     |
| "最后一条消息"     | `last`                 | 最后一条聊天消息的视觉回顾。                  | -                                     |
| "原始最后一条消息" | `raw_last`             | 最后一条消息按原样用作提示。                  | -                                     |
| "背景"             | `background`           | 基于故事背景的聊天背景。                      | 强制宽幅风景纵横比。                   |

## 如何生成图像

1. 在扩展上下文菜单（魔杖）中使用“图像生成”项。
2. 输入一个带有生成模式表中参数的 `/sd (argument)` 斜杠命令。其他任何内容都会触发“自由模式”，使 SD 生成您提示的任何内容。例如：`/sd apple tree` 将生成一幅苹果树的图片。
3. 在聊天消息的上下文操作中寻找画笔图标。这将强制所选消息进入“原始消息”模式。

除了原始消息和自由模式之外的每种生成模式都将触发提示生成，使用您当前选择的主要生成 API 将聊天上下文转换为 SD 提示。您可以使用扩展面板中的“SD 提示模板”设置抽屉配置每种生成模式的提示生成指令模板。

## 支持的来源

* [Stable Horde](https://stablehorde.net/)
* [NovelAI Diffusion](https://novelai.net/)。需要有效的订阅。
* [Stable Diffusion WebUI / AUTOMATIC1111](https://github.com/AUTOMATIC1111/stable-diffusion-webui)
* [SD.Next / vladmandic](https://github.com/vladmandic/automatic)
* [OpenAI DALL-E 2/3](https://platform.openai.com/)
* [TogetherAI](https://api.together.xyz/models)
* [Draw Things](https://drawthings.ai/)
* [Pollinations](https://pollinations.ai/)
* [ComfyUI](https://github.com/comfyanonymous/ComfyUI)。需要一个工作流 JSON 文件。
* [Stability AI](https://platform.stability.ai/)
* [SillyTavern Extras](https://github.com/SillyTavern/SillyTavern-Extras)（已弃用，不推荐）

## 选项

### 在生成之前编辑提示

允许在将自动生成的提示发送到 Stable Diffusion API 之前手动编辑它们。

### 交互模式

允许在用户消息后触发图像生成，而不是文本回复，该消息遵循特定模式：

1. 包含以下动词之一：send, mail, imagine, generate, make, create, draw, paint, render
2. 紧接着包含以下名词之一（不超过10个字符）：pic, picture, image, drawing, painting, photo, photograph
3. 随后是图像生成的目标主题，可以选择性地以“of a”或“of this”等短语开头。

有效请求及捕获主题的示例：

* `Can you please send me a picture of a cat` => `cat`
* `Generate a picture of the Eiffel tower` => `Eiffel tower`
* `Let's draw a painting of Mona Lisa` => `Mona Lisa`

某些特殊主题会触发预定义的生成模式：

* 'you', 'yourself' => "Yourself"
* 'your face', 'your portrait', 'your selfie' => "Your Face"
* 'me', 'myself' => "Me"
* 'story', 'scenario', 'whole story' => "The Whole Story"
* 'last message' => "The Last Message"
* 'background', 'scene background', 'scene', 'scenery', 'surroundings', 'environment' => "Background"

### 自动增强提示

此选项使用额外的 GPT-2 文本生成模型为主 API 生成的提示添加更多细节。 
对于 SDXL 图像模型效果最佳。 对于其他模型可能效果不佳，建议在这种情况下手动编辑提示。

默认 GPT-2 模型: [Cohee/fooocus_expansion-onnx](https://huggingface.co/Cohee/fooocus_expansion-onnx)

### Snap 自动调整分辨率

Snap 图像生成请求将强制纵横比（肖像、背景）调整到最接近的已知分辨率，同时尽量保持绝对像素数量。有关可能选项的列表，请参阅“分辨率”下拉菜单。

**推荐用于 SDXL 模型**。

## 常见提示前缀

添加在每个生成的或自由模式的提示之前。通常用于设置图像的整体风格。

示例：`best quality, anime lineart`。

**专业提示：** 使用 `{prompt}` 宏来指定生成的提示将插入的位置。

## 负面提示

您希望输出中不出现的图像特征。

示例：`bad quality, watermark`.

## 特定角色提示前缀

描述当前选定角色的任何特征。将添加在通用前缀之后。

示例：`女性，绿色眼睛，棕色头发，粉色衬衫`。

限制：
1. 仅在一对一聊天中有效。将不用于群组。
2. 不会用于背景和自由模式生成。

**专业提示：** 如果生成源支持，您还可以在这里使用 LoRAs/嵌入，例如：`<lora:DonaldDuck:1>`。

## 风格

使用此功能快速保存和恢复您喜欢的风格/质量预设，以便稍后使用或在切换模型时使用。风格预设包括以下内容：

1. 常用提示前缀
2. 负面提示