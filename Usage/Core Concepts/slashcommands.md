# 斜杠命令

!!!warning
**这不是一个详尽的列表，因为它更新得不频繁。**

要获取您实例中可用命令的最新列表，请在任何 SillyTavern 聊天中使用 `/help slash` 聊天命令。
!!!

- /help – 显示此帮助信息（别名：/?）
- /name (name) – 设置用户名和角色头像（如果已设置）（别名：/persona）
- /sync – 在当前聊天中同步用户属性消息中的用户名
- /bind – 将角色（名称和头像）绑定/解绑到当前聊天
- /bg (filename) – 根据文件名设置背景，允许部分名称，如果多个文件以提供的参数字符串开头，将按字母顺序设置第一个（别名：/background）
- /sendas – 以特定角色发送消息。

示例：

```
/sendas Chloe
Hello, guys!
将从 "Chloe" 发送 "Hello, guys!"。
如果角色列表中存在角色头像，则使用该头像。
```

- /sys (text) – 以系统叙述者身份发送消息
- /sysname (name) – 为此聊天中的未来系统叙述者消息设置名称（仅显示）。默认值：System。留空以重置。
- /note (text) – 为当前选定的聊天设置作者注释
- /depth (number) – 为聊天中的位置设置作者注释深度
- /freq (number) – 设置作者注释插入频率（别名：/interval）
- /pos (chat or scenario) – 设置作者注释位置（别名：/position）
- /roll (dice formula) – 掷骰子。例如，/roll 2d6（别名：/r）
- /taskcheck – 检查当前任务是否已完成
- /lock – 锁定当前选定聊天的背景
- /unlock – 解锁当前选定聊天的背景
- /autobg – 根据聊天上下文自动更改背景（别名：/bgauto）
- /sd (argument) – 请求 SD 制作图像。支持的参数：

```
you – AI 角色全身自拍
face – AI 角色仅面部自拍
me – 用户角色全身自拍
scene – 整个聊天场景的视觉回顾
last – 上一条聊天消息的视觉回顾
raw_last – 上一条聊天消息的视觉回顾，无摘要
```

任何其他内容将触发 "自由模式"，使 SD 生成您提示的任何内容。
示例：'/sd apple tree' 将生成一张苹果树的图片。