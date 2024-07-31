---
icon: file-symlink-file
---

# STscript语言参考

## 什么是 STscript？

它是一种简单而强大的脚本语言，可以在不需要复杂编码的情况下扩展 SillyTavern 的功能，让您能够：

- 创建迷你游戏或速度挑战
- 构建 AI 驱动的聊天洞察
- 发挥您的创造力并与他人分享

STscript 是使用斜杠命令引擎构建的，利用命令批处理、数据管道、宏和变量。这些概念将在以下文档中描述。

> **STscript 仍在积极开发中。** 如果您计划使用其任何功能，建议使用 *staging* 分支。

### 安全预防措施

拥有强大能力意味着承担重大责任。请小心并始终在执行脚本之前进行检查。

## 你好，世界！

要运行你的第一个脚本，打开任何 SillyTavern 聊天并在聊天输入框中输入以下内容：

```stscript
/pass Hello, World! | /echo
```

| ![Hello World](/static/scripts/hello-world.png) |
| ----------------------------------------------- |

你应该在屏幕顶部的通知中看到消息。现在让我们逐步解析一下。

脚本是一组命令，每个命令以斜杠开头，可以有命名参数和未命名参数，最后以命令分隔符字符 `|` 结束。

命令按顺序执行，一个接一个，并在彼此之间传递数据。

1. `/pass` 命令接受一个常量值 "Hello, World!" 作为未命名参数，并将其写入管道。
2. `/echo` 命令通过管道从前一个命令接收该值，并将其显示为通知。

> **提示：** 要查看所有可用命令的列表，请在聊天中输入 `/help slash`。

由于常量未命名参数和管道是可以互换的，我们可以简单地将这个脚本重写为：

```stscript
/echo Hello, World!
```

## 用户输入

现在让我们为脚本添加一些互动性。我们将接受用户的输入值并在通知中显示它。

```stscript
/input Enter your name |
/echo Hello, my name is {{pipe}}
```

1. `/input` 命令用于显示一个带有未命名参数中指定提示的输入框，然后将输出写入管道。
2. 由于 `/echo` 已经有一个未命名参数来设置输出的模板，我们使用 `{{pipe}}` 宏来指定管道值将被渲染的位置。

| ![Slim Shady Input](/static/scripts/slim-input.png) | ![Slim Shady Output](/static/scripts/slim-output.png) |
| --------------------------------------------------- | ----------------------------------------------------- |

### 其他输入/输出命令

- `/popup (text)` — 显示一个阻塞弹窗，支持简单的 HTML 格式，例如：`/popup <font color=red>I'm red!</font>`。
- `/setinput (text)` — 用提供的文本替换用户输入栏的内容。
- `/speak voice="name" (text)` — 使用选定的 TTS 引擎和语音映射中的角色名称来朗读文本，例如：`/speak name="Donald Duck" Quack!`。
- `/buttons labels=["a","b"] (text)` — 显示一个带有指定文本和按钮标签的阻塞弹窗。`labels` 必须是一个 JSON 序列化的字符串数组或包含该数组的变量名。返回点击的按钮标签，如果取消则返回空字符串。文本支持简单的 HTML 格式。

#### `/popup` 和 `/input` 的参数

`/popup` 和 `/input` 支持以下附加命名参数：
- `large=on/off` - 增加弹窗的垂直大小。默认：`off`。
- `wide=on/off` - 增加弹窗的水平大小。默认：`off`。
- `okButton=string` - 添加自定义“确定”按钮文本的能力。默认：`Ok`。
- `rows=number` - （仅适用于 `/input`）增加输入控件的大小。默认：1。

示例：
```stscript
/popup large=on wide=on okButton="Accept" Please accept our terms and conditions....
```

#### `/echo` 的参数

`/echo` 支持以下值作为附加的 `severity` 参数，以设置显示消息的样式。
  - `warning`
  - `error`
  - `info`（默认）
  - `success`

示例：

```stscript
/echo severity=error Something really bad happened.
```

## 变量

变量用于在脚本中存储和操作数据，可以使用命令或宏。变量可以是以下类型之一：

- 本地变量 — 保存到当前聊天的元数据中，并且是唯一的。
- 全局变量 — 保存到 settings.json 中，并在整个应用程序中存在。

1. `/getvar name` 或 `{{getvar::name}}` — 获取本地变量的值。
2. `/setvar key=name value` 或 `{{setvar::name::value}}` — 设置本地变量的值。
3. `/addvar key=name increment` 或 `{{addvar::name::increment}}` — 将 `increment` 添加到本地变量的值中。
4. `/incvar name` 或 `{{incvar::name}}` — 将本地变量的值增加 1。
5. `/decvar name` 或 `{{decvar::name}}` — 将本地变量的值减少 1。
6. `/getglobalvar name` 或 `{{getglobalvar::name}}` — 获取全局变量的值。
7. `/setglobalvar key=name` 或 `{{setglobalvar::name::value}}` — 设置全局变量的值。
8. `/addglobalvar key=name` 或 `{{addglobalvar::name:increment}}` — 将 `increment` 添加到全局变量的值中。
9. `/incglobalvar name` 或 `{{incglobalvar::name}}` — 将全局变量的值增加 1。
10. `/decglobalvar name` 或 `{{decglobalvar::name}}` — 将全局变量的值减少 1。
11. `/flushvar name` — 删除本地变量的值。
12. `/flushglobalvar name` — 删除全局变量的值。

- 之前未定义变量的默认值是一个空字符串，或者在 `/addvar`、`/incvar`、`/decvar` 命令首次使用时为零。
- `/addvar` 命令中的增量会执行加法或减法，如果增量和变量值都可以转换为数字，否则将执行字符串连接。
- 如果命令参数接受变量名，并且存在同名的本地变量和全局变量，则 *本地变量* 优先。
- 所有用于变量操作的 *斜杠命令* 将结果值写入管道，以供下一个命令使用。
- 对于 *宏*，只有 "get"、"inc" 和 "dec" 类型的宏返回值，"add" 和 "set" 则替换为空字符串。

现在，让我们考虑以下示例：

```stscript
/input What do you want to generate? |
/setvar key=SDinput |
/echo Requesting an image of {{getvar::SDinput}} |
/getvar SDinput |
/imagine
```

1. 用户输入的值保存在名为 `SDinput` 的本地变量中。
2. `getvar` 宏用于在 `/echo` 命令中显示该值。
3. `getvar` 命令用于检索变量的值并通过管道传递。
4. 该值传递给 `/imagine` 命令（由图像生成插件提供），作为其输入提示。

由于变量在脚本执行之间被保存而未被清除，因此您可以在其他脚本和宏中引用该变量，并且它将解析为与示例脚本执行时相同的值。要确保值被丢弃，请将 `/flushvar` 命令添加到脚本中。

### 数组和对象

变量值可以包含 JSON 序列化的数组或键值对（对象）。

示例：
- 数组： `["apple","banana","orange"]`
- 对象： `{"fruits":["apple","banana","orange"]}`

可以对命令进行以下修改，以便处理这些变量：

- `/len` 命令获取数组中的项目数量。
- `index=number/string` 命名参数可以添加到 `/getvar` 或 `/setvar` 及其全局对应命令，以通过零基索引（数组）或字符串键（对象）获取或设置子值。
  - 如果在不存在的变量上使用数字索引，则该变量将被创建为空数组 `[]`。
  - 如果在不存在的变量上使用字符串索引，则该变量将被创建为空对象 `{}`。
- `/addvar` 和 `/addglobalvar` 命令支持将新值推送到数组类型的变量中。

## 流程控制 - 条件语句

您可以使用 `/if` 命令创建条件表达式，根据定义的规则分支执行。

`/if left=valueA right=valueB rule=comparison else="(command on false)" "(command on true)"`

让我们回顾以下示例：

```stscript
/input What's your favorite drink? |
/if left={{pipe}} right="black tea" rule=eq else="/echo You shall not pass \| /abort" "/echo Welcome to the club, \{\{user\}\}"
```

该脚本根据用户输入与所需值进行评估，并根据输入值显示不同的消息。

### `/if` 的参数

1. `left` 是第一个操作数。我们称之为 A。
2. `right` 是第二个操作数。我们称之为 B。
3. `rule` 是要应用于操作数的操作。
4. `else` 是可选的子命令字符串，如果布尔比较的结果为假，则执行该子命令。
5. 未命名参数是在布尔比较结果为真时要执行的子命令。

操作数值的评估顺序如下：

1. 数字字面量
2. 本地变量名
3. 全局变量名
4. 字符串字面量

命名参数的字符串值可以用引号转义，以允许多词字符串。引号随后会被丢弃。

### 布尔运算

支持的布尔比较规则如下。对操作数应用的操作结果为真值或假值。

1. `eq` (等于) => A = B
2. `neq` (不等于) => A != B
3. `lt` (小于) => A < B
4. `gt` (大于) => A > B
5. `lte` (小于或等于) => A <= B
6. `gte` (大于或等于) => A >= B
7. `not` (一元否定) => !A
8. `in` (包含子字符串) => A 包含 B，不区分大小写
9. `nin` (不包含子字符串) => A 不包含 B，不区分大小写

### 子命令

子命令是一个包含要执行的斜杠命令列表的字符串。

1. 要在子命令中使用命令批处理，命令分隔符字符应被转义（见下文）。
2. 由于宏值在条件进入时执行，而不是在子命令执行时执行，因此可以额外转义宏，以将其评估延迟到子命令执行时。
3. 子命令执行的结果被传递给 `/if` 后面的命令。
4. `/abort` 命令在遇到时会中断脚本执行。

`/if` 命令可以作为三元运算符使用。以下示例将在变量 `a` 等于 5 时将 "true" 字符串传递给下一个命令，否则传递 "false" 字符串。

```stscript
/if left=a right=5 rule=eq else="/pass false" "/pass true" |
/echo
```

## 转义序列

### 宏

宏的转义方式与之前一样。然而，使用闭包时，您需要转义宏的频率会比以前少得多。要么转义两个左花括号，要么转义成对的左花括号和右花括号。

```stscript
/echo \{\{char}} |
/echo \{\{char\}\}
```

### 管道

在闭包中，管道不需要转义（作为命令分隔符使用时）。在你想使用字面量管道字符而不是命令分隔符的地方，你需要对其进行转义。

```stscript
/echo title="a\|b" c\|d |
/echo title=a\|b c\|d |
```

使用解析器标志 `STRICT_ESCAPING` 时，你不需要在引用的值中转义管道。

```stscript
/parser-flag STRICT_ESCAPING |
/echo title="a|b" c\|d |
/echo title=a\|b c\|d |
```

### 引用

要在引用值中使用字面量引号字符，必须对该字符进行转义。

```stscript
/echo title="a \"b\" c" d "e" f
```

### 空格

要在命名参数的值中使用空格，您必须将值用引号括起来，或者转义空格字符。

```stscript
/echo title="a b" c d |
/echo title=a\ b c d
```

### 闭包定界符

如果您想使用用于标记闭包开始或结束的字符组合，您必须用一个反斜杠转义该序列。

```stscript
/echo \{: |
/echo \:}
```

## 管道分隔符

```stscript
||
```

为了防止前一个命令的输出被自动作为未命名参数注入到下一个命令中，请在两个命令之间放置双管道。

```stscript
/echo we don't want to pass this on ||
/world
```

## 闭包

```stscript
{: ... :}
```

闭包（块语句、lambda、匿名函数，无论你想怎么称呼它们）是一系列被 `{:` 和 `:}` 包裹的命令，只有在执行代码的那部分时才会被评估。

### 子命令

闭包使得使用子命令变得更加容易，并且消除了转义管道和宏的需要。

```stscript
// if without closures |
/if left=1 rule=eq right=1
    else="
        /echo not equal \|
        /return 0
    "
    /echo equal \|
    /return \{\{pipe}}
```

```stscript
// if with closures |
/if left=1 rule=eq right=1
    else={:
        /echo not equal |
        /return 0
    :}
    {:
        /echo equal |
        /return {{pipe}}
    :}
```

### 作用域

闭包具有自己的作用域，并支持作用域变量。作用域变量通过 `/let` 声明，其值通过 `/var` 设置和检索。获取作用域变量的另一种方式是 `{{var::}}` 宏。

```stscript
/let x |
/let y 2 |
/var x 1 |
/var y |
/echo x is {{var::x}} and y is {{pipe}}.
```

在闭包内，您可以访问在同一闭包或其祖先中声明的所有变量。您无法访问在闭包的后代中声明的变量。  
如果一个变量与在闭包的某个祖先中声明的变量同名，则在该闭包及其后代中无法访问祖先变量。

```stscript
/let x this is root x |
/let y this is root y |
/return {:
    /echo called from level-1: x is "{{var::x}}" and y is "{{var::y}}" |
    /delay 500 |
    /let x this is level-1 x |
    /echo called from level-1: x is "{{var::x}}" and y is "{{var::y}}" |
    /delay 500 |
    /return {:
        /echo called from level-2: x is "{{var::x}}" and y is "{{var::y}}" |
        /let x this is level-2 x |
        /echo called from level-2: x is "{{var::x}}" and y is "{{var::y}}" |
        /delay 500
    :}()
:}() |
/echo called from root: x is "{{var::x}}" and y is "{{var::y}}"
```

### 命名闭包

```stscript
/let x {: ... :} | /:x
```

闭包可以被赋值给变量（仅限作用域变量），以便在稍后的时间调用或用作子命令。

```stscript
/let myClosure {:
    /echo this is my closure
:} |
/:myClosure
```

```stscript
/let myClosure {:
    /echo this is my closure |
    /delay 500
:} |
/times 3 {{var::myClosure}}
```

`/:` 也可以用来执行快速回复，因为它只是 `/run` 的简写。

```stscript
/:QrSetName.QrButtonLabel |
/run QrSetName.QrButtonLabel
```

### 闭包参数

命名闭包可以像斜杠命令一样接受命名参数。这些参数可以具有默认值。

```stscript
/let myClosure {: a=1 b=
    /echo a is {{var::a}} and b is {{var::b}}
:} |
/:myClosure b=10
```

### 闭包和管道参数

!!! info "暂存功能"
以下部分记录了当前仅在 **暂存** 分支上可用的功能。
!!!

来自父闭包的管道值不会自动注入到子闭包的第一个命令中。  
您仍然可以使用 `{{pipe}}` 显式引用父级的管道值，但如果您在闭包内部留下第一个命令的未命名参数为空，则该值将 *不会* 被自动注入。

```stscript
/* This used to attempt to change the model to "foo"
   because the value "foo" from the /echo outside of
   the loop was injected into the /model command
   inside of the loop.
   Now it will simply echo the current model without 
   attempting to change it.
*|
/echo foo |
/times 2 {:
	/model |
	/echo |
:} |
```
```stscript
/* You can still recreate the old behavior by
   explicitly using the {{pipe}} macro.
*|
/echo foo |
/times 2 {:
	/model {{pipe}} |
	/echo |
:} |
```

### 立即执行的闭包

```stscript
{: ... :}()
```

闭包可以被立即执行，这意味着它们将被替换为其返回值。这在没有显式支持闭包的地方非常有用，并且可以缩短一些本来需要大量中间变量的命令。

```stscript
// 两个字符串的简单长度比较，无需闭包 |
/len foo |
/var lenOfFoo {{pipe}} |
/len bar |
/var lenOfBar {{pipe}} |
/if left={{var::lenOfFoo}} rule=eq right={{var:lenOfBar}} /echo yay!
```

```stscript
// 使用立即执行的闭包进行相同的比较 |
/if left={:/len foo:}() rule=eq right={:/len bar:}() /echo yay!
```

!!! info "暂存功能"
以下部分记录了当前仅在**暂存**分支上可用的功能。
!!!

除了运行保存在作用域变量中的命名闭包外，`/run`命令还可以用于立即执行闭包。

```stscript
/run {:
	/add 1 2 3 4 |
:} |
/echo |
```

## 注释

```stscript
// ... | /# ...
```

注释是脚本代码中可读的人类解释或注释。注释不会中断管道。

```stscript
// this is a comment |
/echo foo |
/# this is also a comment
```

### 块注释

!!! info "暂存功能"
以下部分记录了当前仅在 **暂存** 分支上可用的功能。
!!!

块注释可以用于快速注释多个命令。它们不会在管道中终止。

```stscript
/echo foo |
/*
/echo bar |
/echo foobar |
*|
/echo foo again |
```

## 流程控制

### 循环：`/while` 和 `/times`

如果您需要在某个条件满足之前循环执行某个命令，请使用 `/while` 命令。

```stscript
/while left=valueA right=valueB rule=operation guard=on "commands"
```

在循环的每一步，它将变量 A 的值与变量 B 的值进行比较，如果条件为真，则执行引号内的任何有效斜杠命令，否则退出循环。该命令不会向输出管道写入任何内容。

#### `/while` 的参数

**可用的布尔比较、变量处理、字面值和子命令的集合与 `/if` 命令相同。**

可选的 `guard` 命名参数（默认值为 `on`）用于防止无限循环，限制迭代次数为 100。
要禁用并允许无限循环，请设置 `guard=off`。

此示例将 1 加到 `i` 的值，直到其达到 10，然后输出结果值（在此情况下为 10）。

```stscript
/setvar key=i 0 |
/while left=i right=10 rule=lt "/addvar key=i 1" |
/echo {{getvar::i}} |
/flushvar i
```

#### `/times` 的参数

以指定的次数运行子命令。

`/times (repeats) "(command)"` – 任何有效的斜杠命令（用引号括起来）重复指定的次数，例如 `/setvar key=i 1 | /times 5 "/addvar key=i 1"` 将 1 加到 "i" 的值 5 次。
- {{timesIndex}} 被替换为迭代次数（从零开始），例如 `/times 4 "/echo {{timesIndex}}"` 输出数字 0 到 4。
- 循环默认限制为 100 次迭代，传递 `guard=off` 以禁用此限制。

### 跳出循环和闭包

!!! info "暂存功能"
以下部分记录了当前仅在 **暂存** 分支上可用的功能。
!!!

```stscript
/break |
```

`/break` 命令可用于提前跳出循环 (`/while` 或 `/times`) 或闭包。`/break` 的无名参数可用于传递与当前管道不同的值。  
`/break` 目前在以下命令中实现：
- `/while` - 提前退出循环
- `/times` - 提前退出循环
- `/run`（带闭包或通过变量的闭包）- 提前退出闭包
- `/:`（带闭包）- 提前退出闭包

```stscript
/times 10 {:
	/echo {{timesIndex}}
	/delay 500 |
	/if left={{timesIndex}} rule=gt right=3 {:
		/break
	:} |
:} |
```

```stscript
/let x {: iterations=2
	/if left={{var::iterations}} rule=gt right=10 {:
		/break too many iterations! |
	:} |
	/times {{var::iterations}} {:
		/delay 500 |
		/echo {{timesIndex}} |
	:} |
:} |
/:x iterations=30 |
/echo the final result is: {{pipe}}
```

```stscript
/run {:
	/break 1 |
	/pass 2 |
:} |
/echo pipe will be one: {{pipe}} |
```

```stscript
/let x {:
	/break 1 |
	/pass 2 |
:} |
/:x |
/echo pipe will be one: {{pipe}} |
```

## 数学运算

- 以下所有运算接受一系列数字或变量名，并将结果输出到管道。
- 无效运算（如除以零）以及导致 NaN 值或无穷大的运算返回零。
- 乘法、加法、最小值和最大值接受无限数量的以空格分隔的参数。
- 减法、除法、指数运算和取模接受两个以空格分隔的参数。
- 正弦、余弦、自然对数、平方根、绝对值和四舍五入接受一个参数。

**运算列表：**

1. `/add (a b c d)` – 对一组值进行加法运算，例如 `/add 10 i 30 j`
2. `/mul (a b c d)` – 对一组值进行乘法运算，例如 `/mul 10 i 30 j`
3. `/max (a b c d)` – 返回一组值中的最大值，例如 `/max 1 0 4 k`
4. `/min (a b c d)` – 返回一组值中的最小值，例如 `/min 5 4 i 2`
5. `/sub (a b)` – 对两个值进行减法运算，例如 `/sub i 5`
6. `/div (a b)` – 对两个值进行除法运算，例如 `/div 10 i`
7. `/mod (a b)` – 对两个值进行取模运算，例如 `/mod i 2`
8. `/pow (a b)` – 对两个值进行幂运算，例如 `/pow i 2`
9. `/sin (a)` – 对一个值进行正弦运算，例如 `/sin i`
10. `/cos (a)` – 对一个值进行余弦运算，例如 `/cos i`
11. `/log (a)` – 对一个值进行自然对数运算，例如 `/log i`
12. `/abs (a)` – 对一个值进行绝对值运算，例如 `/abs -10`
13. `/sqrt (a)`– 对一个值进行平方根运算，例如 `/sqrt 9`
14. `/round (a)` – 对一个值进行四舍五入到最近整数的运算，例如 `/round 3.14`
15. `/rand (round=round|ceil|floor from=number=0 to=number=1)` – 返回一个在 from 和 to 之间的随机数，例如 `/rand` 或 `/rand 10` 或 `/rand from=5 to=10`。范围是包含端点的。返回值将包含小数部分。使用 `round` 命名参数可获得整数值，例如 `/rand round=ceil` 向上取整，`round=floor` 向下取整，`round=round` 四舍五入到最近整数。

### 示例 1：获取半径为 50 的圆的面积。

```stscript
/setglobalvar key=PI 3.1415 |
/setvar key=r 50 |
/mul r r PI |
/round |
/echo Circle area: {{pipe}}
```

### 示例 2：计算 5 的阶乘。

```stscript
/setvar key=input 5 |
/setvar key=i 1 |
/setvar key=product 1 |
/while left=i right=input rule=lte "/mul product i \| /setvar key=product \| /addvar key=i 1" |
/getvar product |
/echo Factorial of {{getvar::input}}: {{pipe}} |
/flushvar input |
/flushvar i |
/flushvar product
```

## 使用 LLM

脚本可以使用以下命令向您当前连接的 LLM API 发起请求：

- `/gen (prompt)` — 使用提供的提示生成文本，适用于所选角色并包含聊天消息。
- `/genraw (prompt)` — 仅使用提供的提示生成文本，忽略当前角色和聊天。
- `/trigger` — 触发正常生成（相当于点击“发送”按钮）。如果是在群聊中，您可以选择提供一个基于 1 的群成员索引或角色名称以让他们回复，否则根据群设置触发一次群轮次。

### `/gen` 和 `/genraw` 的参数

```stscript
/genraw lock=on/off stop=[] instruct=on/off (prompt)
```

- `lock` — 可以是 `on` 或 `off`。指定在生成过程中是否应阻止用户输入。默认值：`off`。
- `stop` — JSON序列化的字符串数组。为此次生成添加自定义停止字符串（如果API支持）。默认值：无。
- `instruct`（仅适用于 `/genraw`）— 可以是 `on` 或 `off`。允许在输入提示中使用指令格式（如果启用了指令模式并且API支持）。设置为 `off` 以强制使用纯提示。默认值：`on`。
- `as`（适用于文本补全API）— 可以是 `system`（默认）或 `char`。定义最后一行提示的格式。`char` 将使用角色名称，`system` 将使用无或中立名称。

生成的文本随后通过管道传递给下一个命令，可以保存到变量中或使用I/O功能进行显示：

```stscript
/genraw Write a funny message from Cthulhu about taking over the world. Use emojis. |
/popup <h3>Cthulhu says:</h3><div>{{pipe}}</div>
```

| ![Cthulhu Says](/static/scripts/cthulhu-says.png) |
| ------------------------------------------------- |

## 提示注入

脚本可以添加自定义 LLM 提示注入，这基本上相当于无限的作者注释。

- `/inject (text)` — 将任何文本插入当前聊天的正常 LLM 提示中，并需要一个唯一标识符。保存到聊天元数据。
- `/listinjects` — 显示当前聊天中由脚本添加的所有提示注入的列表，以系统消息形式呈现。
- `/flushinjects` — 删除当前聊天中由脚本添加的所有提示注入。
- `/note (text)` — 设置当前聊天的作者注释值。保存到聊天元数据。
- `/interval` — 设置当前聊天的作者注释插入间隔。
- `/depth` — 设置聊天中位置的作者注释插入深度。
- `/position`  — 设置当前聊天的作者注释位置。

### `/inject` 的参数

```stscript
/inject id=IdGoesHere position=chat depth=4 My prompt injection
```

- `id` — 标识符字符串或变量的引用。对同一 ID 的后续调用 `/inject` 将覆盖之前的文本注入。**必填参数。**
- `position` — 设置注入的位置。默认值：`after`。可能的值：
  - `after`: 在主要提示之后。
  - `before`: 在主要提示之前。
  - `chat`: 在聊天中。
- `depth` — 设置聊天位置的注入深度。0 表示在最后一条消息之后插入，1 表示在最后一条消息之前，依此类推。默认值：4。
- 未命名参数是要注入的文本。空字符串将取消提供的标识符的先前值。

## 访问聊天消息

### 阅读消息

您可以使用 `/messages` 命令访问当前选定聊天中的消息。

```stscript
/messages names=on/off start-finish
```

- `names` 参数用于指定是否要包含角色名称，默认值：`on`。
- 在未命名参数中，它接受以 `start-finish` 格式表示的消息索引或范围。范围是包含的！
- 如果范围不满足，即请求无效索引或请求的消息数量超过现有数量，则返回空字符串。
- 从提示中隐藏的消息（由鬼魂图标表示）将被排除在输出之外。
- 如果您想知道最新消息的索引，请使用 `{{lastMessageId}}` 宏，`{{lastMessage}}` 将获取消息本身。

要计算范围的起始索引，例如，当您需要获取最后 N 条消息时，请使用变量减法。
此示例将为您获取聊天中的 3 条最新消息：

```stscript
/setvar key=start {{lastMessageId}} |
/addvar key=start -2 |
/messages names=off {{getvar::start}}-{{lastMessageId}} |
/setinput
```

### 发送消息

脚本可以以用户、角色、人格、中立叙述者的身份发送消息，或添加评论。

1. `/send (text)` — 以当前选定的人格添加消息。
2. `/sendas name=charname (text)` — 以任何角色的身份添加消息，通过他们的名字匹配。`name` 参数是必需的。使用 `{{char}}` 宏以当前角色身份发送。
3. `/sys (text)` — 以中立叙述者的身份添加消息，该消息不属于用户或角色。显示名称纯粹是装饰性的，可以通过 `/sysname` 命令自定义。
4. `/comment (text)` — 添加一个隐藏评论，该评论在聊天中显示，但对提示不可见。
5. `/addswipe (text)` — 向最后一条角色消息添加滑动。无法向用户或隐藏消息添加滑动。
6. `/hide (message id or range)` — 根据提供的消息索引或 `start-finish` 格式的包含范围隐藏一条或多条消息。
7. `/unhide (message id or range)` — 根据提供的消息索引或 `start-finish` 格式的包含范围将一条或多条消息返回到提示中。

`/send`、`/sendas`、`/sys` 和 `/comment` 命令可选择性地接受一个命名参数 `at`，其值为零基数的数字（或包含该值的变量名），指定消息插入的确切位置。默认情况下，新消息将插入到聊天记录的末尾。

这将在对话历史的开头插入一条用户消息：

```stscript
/send at=0 Hi, I use Linux.
```

### 删除消息

**这些命令可能会造成破坏，并且没有“撤销”功能。如果不小心删除了重要内容，请检查 /backups/ 文件夹。**

1. `/cut (消息 ID 或范围)` — 根据提供的消息索引或 `start-finish` 格式的包含范围，从聊天中剪切一条或多条消息。
2. `/del (数字)` — 删除聊天中最后 N 条消息。
3. `/delswipe (基于 1 的滑动 ID)` — 根据提供的基于 1 的滑动 ID，从最后一条字符消息中删除一个滑动。
4. `/delname (角色名称)` — 删除当前聊天中属于指定名称角色的所有消息。
5. `/delchat` — 删除当前聊天。

## 世界信息命令

世界信息（也称为Lorebook）是一个高度实用的工具，用于动态插入数据到提示中。有关更详细的说明，请参见专门页面：[世界信息](https://docs.sillytavern.app/usage/core-concepts/worldinfo/)。

1. `/getchatbook` – 获取与聊天绑定的世界信息文件的名称，如果未绑定则创建一个新文件，并将其传递下去。
2. `/findentry file=bookName field=fieldName [text]` – 使用模糊匹配指定文件（或指向文件名的变量）中的字段值与提供的文本（默认字段：`key`）查找记录的UID，并将UID传递下去，例如 `/findentry file=chatLore field=key Shadowfang`。
3. `/getentryfield file=bookName field=field [UID]` – 获取指定世界信息文件（或指向文件名的变量）中具有UID的记录的字段值（默认字段：`content`），并将值传递下去，例如 `/getentryfield file=chatLore field=content 123`。
4. `/setentryfield file=bookName uid=UID field=field [text]` – 设置指定世界信息文件（或指向文件名的变量）中具有UID（或指向UID的变量）的记录的字段值（默认字段：`content`）。要为关键字段设置多个值，请使用以逗号分隔的列表作为文本值，例如 `/setentryfield file=chatLore uid=123 field=key Shadowfang,sword,weapon`。
5. `/createentry file=bookName key=keyValue [content text]` – 在指定文件（或指向文件名的变量）中创建一个新记录，带有键和值（这两个参数都是*可选的*），并将UID传递下去，例如 `/createentry file=chatLore key=Shadowfang The sword of the king`。

### 有效输入字段

| 字段                | UI 元素           | 值类型          |
| :----------------- | :---------------- | :-------------- |
| `content`          | 内容              | 字符串          |
| `comment`          | 标题 / 备注       | 字符串          |
| `key`              | 主要关键词        | 字符串列表      |
| `keysecondary`     | 可选过滤          | 字符串列表      |
| `constant`         | 常量状态          | 布尔值 (1/0)    |
| `disable`          | 禁用状态          | 布尔值 (1/0)    |
| `order`            | 顺序              | 数字            |
| `selectiveLogic`   | 逻辑              | (见下文)        |
| `excludeRecursion` | 非递归           | 布尔值 (1/0)    |
| `probability`      | 触发%            | 数字 (0-100)    |
| `depth`            | 深度              | 数字 (0-999)    |
| `position`         | 位置              | (见下文)        |
| `role`             | 深度角色          | (见下文)        |
| `scanDepth`        | 扫描深度          | 数字 (0-100)    |
| `caseSensitive`    | 区分大小写        | 布尔值 (1/0)    |
| `matchWholeWords`  | 匹配完整单词      | 布尔值 (1/0)    |

**逻辑值**

- 0 = AND ANY
- 1 = NOT ALL
- 2 = NOT ANY
- 3 = AND ALL

**位置值**

- 0 = 在主提示之前
- 1 = 在主提示之后
- 2 = 在作者说明顶部
- 3 = 在作者说明底部
- 4 = 在深度的聊天中
- 5 = 在示例消息顶部
- 6 = 在示例消息底部

**角色值**（仅限位置 = 4）
- 0 = 系统
- 1 = 用户
- 2 = 助手

### 示例 1：通过键从聊天图书馆读取内容

```stscript
/getchatbook | /setvar key=chatLore |
/findentry file=chatLore field=key Shadowfang |
/getentryfield file=chatLore field=key |
/echo
```

### 示例 2：创建聊天资料册条目，包含关键字和内容

```stscript
/getchatbook | /setvar key=chatLore |
/createentry file=chatLore key="Milla" Milla Basset is a friend of Lilac and Carol. She is a hush basset puppy who possesses the power of alchemy. |
/echo
```

### 示例 3：根据聊天记录扩展现有的资料库条目

```stscript
/getchatbook | /setvar key=chatLore |
/findentry file=chatLore field=key Milla |
/setvar key=millaUid |
/getentryfield file=chatLore field=content |
/setvar key=millaContent |
/gen lock=on Tell me more about Milla Basset based on the provided conversation history. Incorporate existing information into your reply: {{getvar::millaContent}} |
/setvar key=millaContent |
/echo New content: {{pipe}} |
/setentryfield file=chatLore uid=millaUid field=content {{getvar::millaContent}}
```

## 文本处理

有多种有用的文本处理实用命令可用于各种脚本场景。

1. `/trimtokens` — 从开始或结束修剪输入到指定数量的文本标记，并将结果输出到管道。
2. `/trimstart` — 将输入修剪到第一个完整句子的开始，并将结果输出到管道。
3. `/trimend` — 将输入修剪到最后一个完整句子的结束，并将结果输出到管道。
4. `/fuzzy` — 对输入文本与字符串列表进行模糊匹配，将最佳字符串匹配输出到管道。
5. `/regex name=scriptName [text]` — 从正则表达式扩展执行指定文本的正则表达式脚本。该脚本必须已启用。

### `/trimtokens` 的参数

```stscript
/trimtokens limit=number direction=start/end (input)
```

1. `direction` 设置修剪的方向，可以是 `start` 或 `end`。默认值：`end`。
2. `limit` 设置输出中保留的令牌数量。也可以指定一个包含数字的变量名。**必需参数。**
3. 未命名参数是要修剪的输入文本。

### `/fuzzy` 的参数

```stscript
/fuzzy list=["candidate1","candidate2"] (input)
```

1. `list` 是一个 JSON 序列化的字符串数组，包含候选项。也可以指定包含列表的变量名。**必需参数。**
2. 未命名参数是要匹配的输入文本。输出是与输入最接近的候选项之一。

## 自动补全

- 自动补全在聊天输入框和大型快速回复编辑器中均已启用。
- 自动补全可以在输入的任何位置工作。即使有多个管道命令和嵌套闭包。
- 自动补全支持三种查找匹配命令的方式 (*用户设置* -> *STscript 匹配*)。

1. **以...开头** 旧的方式。只有完全以输入值开头的命令会显示。
2. **包含** 所有*包含*输入值的命令会显示。例如：输入 `/delete` 时，命令 `/qr-delete` 和 `/qr-set-delete` 会出现在自动补全列表中 (/qr-**delete**, /qr-set-**delete**)。
3. **模糊** 所有可以与输入值进行模糊匹配的命令会显示。例如：输入 `/seas` 时，命令 `/sendas` 会出现在自动补全列表中 (/**se**nd**as**)。

- 命令参数也支持自动补全。对于必需参数，列表会自动显示。对于可选参数，按 *Ctrl*+*Space* 打开可用选项列表。
- 输入 `/:` 来执行闭包或 QR 时，自动补全会显示作用域变量和 QR 的列表。
- 自动补全对宏（在斜杠命令中）有有限支持。输入 `{{` 以获取可用宏的列表。
- 使用 *上* 和 *下* *箭头键* 从自动补全选项列表中选择一个选项。
- 按 *Enter* 或 *Tab* 或 *点击* 一个选项将选项放置在光标位置。
- 按 *Escape* 关闭自动补全列表。
- 按 *Ctrl*+*Space* 打开自动补全列表或切换选定选项的详细信息。

## 解析器标志

```stscript
/parser-flag
```

解析器接受标志以修改其行为。这些标志可以在脚本中的任何时候切换开关，所有后续输入将相应地进行评估。  
您可以在用户设置中设置默认标志。

### 严格转义

```stscript
/parser-flag STRICT_ESCAPING on |
```

启用 `STRICT_ESCAPING` 的更改如下。

#### 管道

在引用值中，管道不需要转义。

```stscript
/echo title="a|b" c\|d
```

#### 反斜杠

符号前的反斜杠可以转义，以提供字面反斜杠后跟功能符号。

```stscript
// 这将输出 "foo \", 然后输出 "bar" |
/echo foo \\|
/echo bar
```

```stscript
/echo \\|
/echo \\\|
```

### 替换变量宏

```stscript
/parser-flag REPLACE_GETVAR on |
```

该标志有助于避免在变量值包含可能被解释为宏的文本时发生双重替换。`{{var::}}` 宏最后被替换，并且在结果文本/变量值上不会发生进一步的替换。

将所有 `{{getvar::}}` 和 `{{getglobalvar::}}` 宏替换为 `{{var::}}`。在后台，解析器将在替换宏的命令之前插入一系列命令执行器：

- 调用 `/let` 将当前 `{{pipe}}` 保存到一个作用域变量
- 调用 `/getvar` 或 `/getglobalvar` 获取宏中使用的变量
- 调用 `/let` 将检索到的变量保存到一个作用域变量
- 调用 `/return` 使用保存的 `{{pipe}}` 值来恢复下一个命令的正确管道值

```stscript
// the following will echo the last message's id / number |
/setvar key=x \{\{lastMessageId}} |
/echo {{getvar::x}}
```

```stscript
// this will echo the literal text {{lastMessageId}} |
/parser-flag REPLACE_GETVAR |
/setvar key=x \{\{lastMessageId}} |
/echo {{getvar::x}}
```

## 快速回复：脚本库和自动执行

快速回复是一个内置的SillyTavern扩展，提供了一种简单的方法来存储和执行您的脚本。

### 配置快速回复

为了开始，请启用扩展面板（堆叠块图标），并展开快速回复菜单。

<div style="display:flex;justify-content:center">

![Quick Reply](/static/scripts/quick-reply.png)

</div>

**快速回复默认是禁用的，您需要先启用它们。** 然后您会看到一个条出现在您的聊天输入框上方。

您可以设置显示的按钮文本标签（我们建议使用表情符号以简洁为主），以及在单击按钮时将执行的脚本。

按钮的数量由**插槽数量**设置控制（最大 = 100），根据您的需要进行调整，完成后单击“应用”。

**建议在使用STscript时禁用自动注入用户输入**，否则可能会干扰您的输入，使用`{{input}}`宏在脚本中获取输入框的当前值。

**快速回复预设**允许拥有多个预定义快速回复集，并可以手动切换或使用`/qrset (集名称)`命令进行切换。
在切换到不同的预设之前，请不要忘记单击“更新”以将您的更改写入当前使用的预设！

### 手动执行

现在您可以将第一个脚本添加到库中。选择任意空闲插槽（或创建一个），在左侧框中输入“Click me”以设置标签，然后将以下内容粘贴到右侧框中：

```stscript
/addvar key=clicks 1 |
/if left=clicks right=5 rule=eq else="/echo Keep going..." "/echo You did it!  \| /flushvar clicks"
```

然后在聊天栏上方出现的按钮上点击 5 次。
每次点击将变量 `clicks` 增加 1，当值等于 5 时显示不同的消息并重置变量。

### 自动执行

通过点击创建命令的 `⋮` 按钮打开模态菜单。

| ![自动执行](/static/scripts/autoexecute.png) |
| ------------------------------------------------------- |

在此菜单中，您可以执行以下操作：

- 在方便的全屏编辑器中编辑脚本
- 隐藏聊天栏中的按钮，使其仅对自动执行可用。
- 在以下一个或多个条件下启用自动执行：
  * 应用程序启动
  * 向聊天发送用户消息
  * 在聊天中接收AI消息
  * 打开角色或群组聊天
  * 触发群组成员的回复
  * 使用相同的自动化ID激活世界信息条目
- 为快速回复提供自定义工具提示（在您的用户界面中悬停在快速回复上时显示的文本）
- 出于测试目的执行脚本

仅在启用快速回复扩展时，命令才会自动执行。

例如，您可以通过添加以下脚本并将其设置为在用户消息时自动执行，在发送五条用户消息后显示一条消息。

```stscript
/addvar key=usercounter 1 |
/echo You've sent {{pipe}} messages. |
/if left=usercounter right=5 rule=gte "/echo Game over! \| /flushvar usercounter"
```

### 调试器

!!! info "暂存功能"
以下部分记录了当前仅在 **暂存** 分支上可用的功能。
!!!

在扩展的快速回复编辑器中存在一个基本的调试器。可以在脚本中的任何位置使用 `/breakpoint |` 设置断点。当从 QR 编辑器执行脚本时，执行将在该点中断，允许您检查当前可用的变量、管道、命令参数等，并逐步执行其余代码。

```stscript
/let x {: n=1
	/echo n is {{var::n}} |
	/mul n n |
:} |
/breakpoint |
/:x n=3 |
/echo result is {{pipe}} |
```

| ![QR Editor Debugger](/static/scripts/st-debugger.png) |
| ------------------------------------------------------ |

### 调用过程

一个 `/run` 命令可以通过其标签调用在快速回复中定义的脚本，基本上提供了定义过程和返回结果的能力。这允许有可重用的脚本块，其他脚本可以引用它们。过程管道的最后结果会传递给其后的下一个命令。

```stscript
/run ScriptLabel
```

让我们创建两个快速回复：

***
**标签：**

`GetRandom`

**命令：**

```stscript
/pass {{roll:d100}}
```
***
**标签：**

`GetMessage`

**命令：**
```stscript
/run GetRandom | /echo Your lucky number is: {{pipe}}
```
***

点击 `GetMessage` 按钮将调用 `GetRandom` 过程，该过程将解析 `{{roll}}` 宏并将数字传递给调用者，显示给用户。

- 过程不接受命名或未命名的参数，但可以引用与调用者相同的变量。
- 调用过程时避免递归，因为如果处理不当可能会产生“调用栈超出”错误。

#### 从不同的快速回复预设调用过程

您可以使用 `a.b` 语法从不同的快速回复预设调用过程，其中 a = QR 预设名称，b = QR 标签名称。

```stscript
/run QRpreset1.QRlabel1
```

默认情况下，系统将首先查找快速回复标签 `a.b`，因此如果您的标签确实是 "QRpreset1.QRlabel1"，它将尝试运行该标签。如果未找到此标签，它将搜索 QR 预设名称 "QRpreset1" 和标签为 "QRlabel1" 的 QR。

### 快速回复管理命令

#### 创建快速回复

* `/qr-create (arguments, [message])` – 创建一个新的快速回复，例如：`/qr-create set=MyPreset label=MyButton /echo 123`

参数：
- `label`    - 字符串 - 按钮上的文本，例如，`label=MyButton`
- `set`      - 字符串 - 快速回复集的名称，例如，`set=PresetName1`
- `hidden`   - 布尔值 - 按钮是否应隐藏，例如，`hidden=true`
- `startup`  - 布尔值 - 应用启动时自动执行，例如，`startup=true`
- `user`     - 布尔值 - 用户消息时自动执行，例如，`user=true`
- `bot`      - 布尔值 - AI 消息时自动执行，例如，`bot=true`
- `load`     - 布尔值 - 聊天加载时自动执行，例如，`load=true`
- `title`    - 布尔值 - 按钮上显示的标题/工具提示，例如，`title="My Fancy Button"`

#### 删除快速回复

* `/qr-delete (set=string [label])` – 删除快速回复

#### 更新快速回复

* `/qr-update (arguments, [message])` – 更新快速回复，例如：`/qr-update set=MyPreset label=MyButton newlabel=MyRenamedButton /echo 123`

参数：
- `newlabel` - 字符串 - 按钮的新文本，例如，`newlabel=MyRenamedButton`
- `label`    - 字符串 - 按钮上的文本，例如，`label=MyButton`
- `set`      - 字符串 - 快速回复集的名称，例如，`set=PresetName1`
- `hidden`   - 布尔值 - 按钮是否应隐藏，例如，`hidden=true`
- `startup`  - 布尔值 - 应用启动时自动执行，例如，`startup=true`
- `user`     - 布尔值 - 用户消息时自动执行，例如，`user=true`
- `bot`      - 布尔值 - AI 消息时自动执行，例如，`bot=true`
- `load`     - 布尔值 - 聊天加载时自动执行，例如，`load=true`
- `title`    - 布尔值 - 按钮上显示的标题/工具提示，例如，`title="My Fancy Button"`

####

!!! info "暂存功能"
以下部分记录了当前仅在 **暂存** 分支上可用的功能。
!!!

* `qr-get` - 检索快速回复的所有属性，例如：`/qr-get set=myQrSet id=42`

#### 创建或更新快速回复预设

* `/qr-presetupdate (arguments [label])` 或 `/qr-presetadd (arguments [label])`

参数：
- `enabled` - 布尔值 - 启用或禁用预设
- `nosend`  - 布尔值 - 禁用发送/插入用户输入（对斜杠命令无效）
- `before`  - 布尔值 - 在用户输入之前放置快速回复
- `slots`   - 整数  - 插槽数量
- `inject`  - 布尔值 - 自动注入用户输入（如果禁用，请使用 `{{input}}`）

创建一个新的预设（覆盖现有的），例如：`/qr-presetadd slots=3 MyNewPreset`

#### 添加快速回复上下文菜单

* `/qr-contextadd (set=string label=string chain=bool [preset name])` – 将上下文菜单预设添加到快速回复，例如：`/qr-contextadd set=MyPreset label=MyButton chain=true MyOtherPreset`

#### 移除所有上下文菜单

* `/qr-contextclear (set=string [label])` – 从快速回复中移除所有上下文菜单预设，例如：`/qr-contextclear set=MyPreset MyButton`

#### 移除一个上下文菜单

* `/qr-contextdel (set=string label=string [preset name])` – 从快速回复中移除上下文菜单预设，例如：`/qr-contextdel set=MyPreset label=MyButton MyOtherPreset`

### 快速回复值转义

`|{}` 可以在 QR 消息/命令中通过反斜杠进行转义。

例如，使用 `/qr-create label=MyButton /getvar myvar \| /echo \{\{pipe\}\}` 创建一个调用 `/getvar myvar | /echo {{pipe}}` 的 QR。

## 扩展命令

SillyTavern 扩展（包括内置、可下载和第三方）可以添加自己的斜杠命令。以下仅是官方扩展功能的示例。该列表可能不完整，请确保检查 `/help slash` 以获取可用命令的最完整列表。

1. `/websearch (query)` — 在网上搜索指定查询的网页片段，并将结果返回到管道中。由 Web Search 扩展提供。
2. `/imagine (prompt)` — 使用提供的提示生成图像。由 Image Generation 扩展提供。
3. `/emote (sprite)` — 通过模糊匹配名称为活动角色设置精灵。由 Character Expressions 扩展提供。
4. `/costume (subfolder)` — 为活动角色设置精灵集覆盖。由 Character Expressions 扩展提供。
5. `/music (name)` — 强制按名称更改播放的背景音乐文件。由 Dynamic Audio 扩展提供。
6. `/ambient (name)` — 强制按名称更改播放的环境音效文件。由 Dynamic Audio 扩展提供。
7. `/roll (dice formula)` — 在聊天中添加一条隐藏消息，显示掷骰结果。由 D&D Dice 扩展提供。

## UI 交互

脚本还可以与 SillyTavern 的 UI 进行交互：浏览聊天或更改样式参数。

### 角色导航

1. `/random` — 打开与随机角色的聊天。
2. `/go (name)` — 打开与指定名称角色的聊天。首先搜索完全匹配的名称，然后按前缀搜索，再按子字符串搜索。

### UI 样式

1. `/bubble` — 将消息样式设置为“气泡聊天”样式。
2. `/flat` — 将消息样式设置为“扁平聊天”样式。
3. `/single` — 将消息样式设置为“单一文档”样式。
4. `/movingui (name)` — 按名称激活一个 MovingUI 预设。
5. `/resetui` — 将 MovingUI 面板状态重置为其原始位置。
6. `/panels` — 切换 UI 面板的可见性：顶部栏、左侧和右侧抽屉。
6. `/bg (name)` — 查找并使用模糊名称匹配设置背景。尊重聊天背景锁定状态。
7. `/lockbg` — 锁定当前聊天的背景图像。
8. `/unlockbg` — 解锁当前聊天的背景图像。

## 更多示例

### 生成聊天摘要 (由 @IkariDevGIT)

```stscript
/setglobalvar key=summaryPrompt Summarize the most important facts and events that have happened in the chat given to you in the Input header. Limit the summary to 100 words or less. Your response should include nothing but the summary. |
/setvar key=tmp |
/messages 0-{{lastMessageId}} |
/trimtokens limit=3000 direction=end |
/setvar key=s1 |
/echo Generating, please wait... |
/genraw lock=on instruct=off {{instructInput}}{{newline}}{{getglobalvar::summaryPrompt}}{{newline}}{{newline}}{{instructInput}}{{newline}}{{getvar::s1}}{{newline}}{{newline}}{{instructOutput}}{{newline}}The chat summary:{{newline}} |
/setvar key=tmp |
/echo Done! |
/setinput {{getvar::tmp}} |
/flushvar tmp |
/flushvar s1
```

### 按钮弹出使用

```stscript
/setglobalvar key=genders ["boy", "girl", "other"] |
/buttons labels=genders Who are you? |
/echo You picked: {{pipe}}
```

### 获取第 N 个斐波那契数（使用比奈公式）

> **提示**：将 `fib_no` 的值设置为所需的数字

```stscript
/setvar key=fib_no 5 |
/pow 5 0.5 | /setglobalvar key=SQRT5 |
/setglobalvar key=PHI 1.618033 |
/pow PHI fib_no | /div {{pipe}} SQRT5 |
/round |
/echo {{getvar::fib_no}}th Fibonacci's number is: {{pipe}}
```

### 递归阶乘（使用闭包）

```stscript
/let fact {: n=
    /if left={{var::n}} rule=gt right=1
        else={:
            /return 1
        :}
        {:
            /sub {{var::n}} 1 |
            /:fact n={{pipe}} |
            /mul {{var::n}} {{pipe}}
        :}
:} |

/input 计算阶乘： |
/let n {{pipe}} |
/:fact n={{var::n}} |
/echo {{var::n}} 的阶乘是 {{pipe}}
```