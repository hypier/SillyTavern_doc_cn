# VRM

本指南将引导您完成为您的SillyTavern体验设置和自定义VRM扩展的过程。此扩展允许您为角色使用VRM动画模型，为您的虚拟角色提供动态和互动的元素。

## 前提条件

在开始之前，请确保您已满足以下前提条件：

1. **分支选择**：确保您使用的是 SillyTavern 的最新 `staging` 分支，以访问最新的功能和更新。

2. **扩展安装**：从扩展面板中的“下载扩展和资产”菜单（由堆叠块图标表示）安装“VRM”扩展。

3. **模型文件夹放置**：将您的 VRM 模型文件 (.vrm) 放入 `/data/<user-handle>/assets/vrm/model` 目录，将您的动画文件放入 `/data/<user-handle>/assets/vrm/animation` 目录。目前支持的动画文件格式为与 VRM 模型兼容的 .fbx 和 .bvh。这包括您可以从 Mixamo (https://www.mixamo.com/) 获取的任何动画，以及您可以从 XR Animator (https://github.com/ButzYung/SystemAnimatorOnline) 等工具导出的任何动画。

## 扩展设置

VRM 扩展提供了多种设置，以自定义您动画模型的行为。以下是关键设置：

![UI global settings](/static/extensions/vrm-global.png)

### 全局设置

1. **启用**:
   - 勾选此复选框以激活扩展，使您的 VRM 模型能够在 SillyTavern 中进行交互。
   - 如果您只想使用普通精灵，可以禁用该扩展。

2. **注视摄像头**:
   - 勾选此复选框以使 VRM 模型的眼睛注视摄像头。

3. **眨眼**:
   - 勾选此复选框以使 VRM 模型的眼睛在随机间隔内眨眼。模型表情应正确定义眨眼权重属性，否则模型可能会闭眼眨眼，例如，如果发生这种情况：
    - 如果您拥有 .vroid 文件，请修正模型
    - 不要使用该不正确的面部表情
    - 使用此复选框完全禁用眨眼

4. **TTS 同步**:
    - 勾选此复选框以使 VRM 的嘴部动作跟随播放的 TTS 声音。仅适用于由 Sillytavern 本身播放的 TTS 声音，如 XTTS（不在流媒体模式下）。如果禁用，嘴部将根据接收到的新角色消息的文本长度进行动画。

5. **自动发送交互**:
   - 勾选此复选框以在您点击带有映射消息的区域时自动触发角色交互（有关详细信息，请参见命中区域部分）。

### 性能设置

1. **身体碰撞盒**
    - 启用此复选框以激活对VRM模型多个部分的点击检测，具体可检测区域包括：头部/胸部/手部/腹股沟/臀部/腿部/脚部。碰撞盒的位置在每一帧中计算，并随身体动画而变化，禁用此选项可以提高性能。

2. **使用模型缓存**
    - 启用此复选框以在切换模型时将VRM模型保留在内存中，可以更快地切换回上一个模型。如果您为同一角色使用不同模型以更换服装或形态，这将非常有用。可能会影响性能。

3. **使用动画缓存**
    - 启用此复选框以在会话期间将所有播放的动画保留在内存中。分配给模型的所有动画将在模型首次出现时加载。将增加您首次加载模型的时间，但使所有动画切换瞬时。可能会影响性能。

### 调试设置

1. **显示网格**
    - 启用此复选框以可视化 3d 网格、模型拖拽框和身体碰撞盒。

2. **重新加载按钮**
    - 点击此按钮以重新加载 3d 场景，清除缓存和所有 VRM 模型。如果出现某些错误或缓存开始影响性能，请使用它。

### 场景设置

![UI 场景设置](/static/extensions/vrm-scene.png)

1. **光线颜色**
    - 设置 3D 场景中的光线颜色。点击重置按钮将其恢复为默认的白色。根据您的浏览器，您可以使用颜色选择器，例如，您可以选择背景图像的颜色以增加沉浸感。

2. **光线强度**
    - 使用滑块设置光线强度的百分比。点击重置按钮将其恢复为默认值 100%。VRM 模型对光线的反应可能因模型中烘焙的着色器而异，尝试调整该值并观察效果。

![UI 模型设置](/static/extensions/vrm-model.png)

## 角色选择

这些设置允许您管理角色并为其分配 VRM 模型。

1. **刷新按钮**：
   - 点击刷新按钮以更新当前聊天中的角色列表。

2. **选择角色**：
   - 使用下拉列表选择一个角色以分配 VRM 模型。

3. **移除按钮**：
   - 点击此按钮以删除角色的分配模型。

## 模型选择

1. **刷新按钮**：
   - 如果您的 VRM 模型未出现在列表中，请点击刷新按钮。

2. **选择模型**：
   - 从列表中选择一个模型，并将其分配给选定的角色。
   - 模型必须位于 `/data/<user-handle>/assets/vrm/model` 目录中。

3. **重置按钮**
    - 点击此按钮将模型设置重置为默认值。如果您有与默认值对应的动画文件，它们将自动映射。请参阅本 README 末尾的命名映射。

## 模型设置

1. **模型规模**:
   - 使用滑块调整模型的大小，使其变大或变小。

2. **模型中心 X/Y 偏移**:
   - 使用这些滑块更改模型相对于窗口中心的水平/垂直位置。

4. **模型 X/Y 旋转**
    - 使用这些滑块更改模型相对于模型臀部的水平/垂直旋转。

### 备注
    - 设置是按模型保存的，而不是按角色，并且会在不同的聊天中保留。
    - 如果您想对两个不同角色使用相同的模型但设置不同，请复制 .vrm 文件。
    - 您还可以用鼠标拖动模型，这些设置将会更新并保存。左键单击并按住以在屏幕上拖动模型。中键单击并按住以旋转模型，或使用 Shift+左键单击。将鼠标光标放在模型上并滚动鼠标滚轮以缩放模型，或使用 Ctrl+左键单击。
    - 如果您不小心将模型移出视图，请使用这些 UI 设置将模型带回屏幕上。同时，勾选“显示框架”复选框，以清楚地查看可以单击以拖动模型的位置。

![UI hitboxes settings](/static/extensions/vrm-hitboxes.png)

## 碰撞盒映射

    - 根据模型骨骼的定义，可以生成一些碰撞盒区域，它们将在用户界面的这一部分列出，您可以为每个区域分配一个表达式/动画/消息，当您点击该区域时会触发。

![UI classify settings](/static/extensions/vrm-classify.png)

## 分类表达映射

1. **需求**
    - 需要使用分类表达扩展；否则，将回退到默认动画。

2. **映射**
    - 对于分类扩展检测到的每种情感，您可以分配一个表达/动作/消息。消息可以包含命令。

## 命令

1. **/vrmlightcolor**
    - 设置光源颜色
    - 参数：color
    - 示例："/vrmlightcolor white" 或 "/vrmlightcolor purple"。
2. **/vrmlightintensity**
    - 设置光源强度（百分比）
    - 参数：intensity
    - 示例："/vrmlightintensity 0" 或 "/vrmlightintensity 100"
3. **/vrmmodel**
    - 将 vrm 模型分配给角色
    - 参数：character, model
    - 示例：在单人聊天中使用 "/vrmmodel Seraphina.vrm" 或在群聊中使用 "/vrmmodel character=Seraphina model=Seraphina.vrm"
4. **/vrmexpression**
    - 改变模型的表情
    - 参数：character, expression
    - 示例：在单人聊天中使用 "/vrmexpression happy" 或在群聊中使用 "/vrmexpression character=Seraphina expression=happy"

5. **/vrmmotion**
    - 改变模型的动画
    - 参数：character, motion, loop, random
    - 示例："/vrmmotion idle" 或 "/vrmmotion character=Seraphina motion=idle loop=true random=false"

## 动画默认映射
如果您的动画文件按以下方式命名，它们将在重置模型设置时自动映射。例如，名为 "assets/vrm/animation/neutral.bvh" 和 "assets/vrm/animation/neutral1.fbx" 的文件将作为默认和中性分类动画的组自动映射。命中框也是如此。

    // Fallback
    "default": "assets/vrm/animation/neutral",

    // Classify class
    "admiration": "assets/vrm/animation/admiration",
    "amusement": "assets/vrm/animation/amusement",
    "anger": "assets/vrm/animation/anger",
    "annoyance": "assets/vrm/animation/annoyance",
    "approval": "assets/vrm/animation/approval",
    "caring": "assets/vrm/animation/caring",
    "confusion": "assets/vrm/animation/confusion",
    "curiosity": "assets/vrm/animation/curiosity",
    "desire": "assets/vrm/animation/desire",
    "disappointment": "assets/vrm/animation/disappointment",
    "disapproval": "assets/vrm/animation/disapproval",
    "disgust": "assets/vrm/animation/disgust",
    "embarrassment": "assets/vrm/animation/embarrassment",
    "excitement": "assets/vrm/animation/excitement",
    "fear": "assets/vrm/animation/fear",
    "gratitude": "assets/vrm/animation/gratitude",
    "grief": "assets/vrm/animation/grief",
    "joy": "assets/vrm/animation/joy",
    "love": "assets/vrm/animation/love",
    "nervousness": "assets/vrm/animation/nervousness",
    "neutral": "assets/vrm/animation/neutral",
    "optimism": "assets/vrm/animation/optimism",
    "pride": "assets/vrm/animation/pride",
    "realization": "assets/vrm/animation/realization",
    "relief": "assets/vrm/animation/relief",
    "remorse": "assets/vrm/animation/remorse",
    "sadness": "assets/vrm/animation/sadness",
    "surprise": "assets/vrm/animation/surprise",

    // Hitboxes
    "head": "assets/vrm/animation/hitarea_head",
    "chest": "assets/vrm/animation/hitarea_chest",
    "groin": "assets/vrm/animation/hitarea_groin",
    "butt": "assets/vrm/animation/hitarea_butt",
    "leftHand": "assets/vrm/animation/hitarea_hands",
    "rightHand": "assets/vrm/animation/hitarea_hands",
    "leftLeg": "assets/vrm/animation/hitarea_leg",
    "rightLeg": "assets/vrm/animation/hitarea_leg",
    "rightFoot": "assets/vrm/animation/hitarea_foot",
    "leftFoot": "assets/vrm/animation/hitarea_foot"

感谢您遵循本指南！您的 SillyTavern 体验现在已通过动画和互动 3D 模型得到了丰富。

## 备注
    - 此扩展加载的 VRM 模型为 .vrm 文件，而非 .vroid 文件。
    - 动画文件应与 VRM 兼容，您可以使用像 XR animation (https://github.com/ButzYung/SystemAnimatorOnline) 这样的工具将 fbx/bvh 动画文件转换。
    - 您可以通过将文件命名为相同名称并以不同数字结尾来创建动画组，例如：“idle1.bvh”、“idle2.bhv”、“idle3.bvh” 将被视为一个组“idle”，在映射中选择时将随机播放一个，能够为动画增加多样性。
    - 您可以从这个库获取精选动画： https://github.com/test157t/VRM-Animations-Pack-For-Silly-Tavern
    - Nitral 有一些关于如何使用此扩展和动画库的教程视频： https://www.youtube.com/@nitralai