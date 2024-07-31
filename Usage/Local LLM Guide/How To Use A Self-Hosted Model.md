---
order: 103
---

# 自托管 AI 模型

!!! warning
本指南基于作者的个人经验和知识，并非绝对真理。所有陈述都应持保留态度。如有任何更正或建议，请通过 Discord 联系我们或向 [SillyTavern 文档库](https://github.com/SillyTavern/SillyTavern-Docs) 提交 PR。
!!!

## 介绍

本指南旨在帮助您在本地计算机上使用 SillyTavern 设置一个运行中的 AI（我们将从现在开始使用正确的术语，称其为 LLM）。在打扰他人进行技术支持问题之前，请先阅读此内容。

### 最好的大型语言模型是什么？

这个问题无法回答，因为没有标准化的“最佳”评估标准。社区在Reddit和Discord上有足够的资源和讨论，可以形成对首选/常用模型的某种看法。你的体验可能会有所不同。

### 什么是最佳配置？

如果有最佳或显而易见的设置，是否还需要配置？最佳配置就是适合你的配置。这是一个试错的过程。

## 硬件要求和方向

这是一个复杂的话题，所以我将坚持基本要点并进行概括。

* 互联网上有成千上万的免费 LLM 可供下载，类似于 Stable Diffusion 拥有大量可用于生成图像的模型。
* 运行未修改的 LLM 需要一张强大的 GPU，配备大量 VRAM（GPU 内存）。这超出了你所能拥有的。
* 可以通过使用量化技术（如 GPTQ 或 AWQ）压缩模型，从而减少 VRAM 的需求。这使得模型的能力有所降低，但大大减少了运行所需的 VRAM。突然间，这使得拥有像 3080 这样的游戏 GPU 的人能够运行一个 13B 的模型。尽管它不如未量化模型好，但仍然不错。
* 更好的是：还有一种模型格式和量化方法称为 GGUF（之前称为 GGML），已经成为普通用户的首选格式，而不需要强大的 GPU。这使得你可以完全不使用 GPU 来使用 LLM。它只会使用 CPU 和 RAM。这比在 GPU 上使用 GPTQ/AWQ 运行 LLM 慢得多（可能慢 15 倍），尤其是在提示处理期间，但模型的能力仍然很好。GGUF 的创建者进一步优化了 GGUF，添加了一个配置选项，允许拥有游戏级 GPU 的人将模型的部分内容卸载到 GPU，从而使他们能够以 GPU 速度运行部分模型（注意，这并不会减少 RAM 的需求，只会提高生成速度）。
* 模型有不同的大小，名称是根据它们训练时的参数数量命名的。你会看到像 7B、13B、30B、70B 等名称。你可以把这些看作模型的“大脑”大小。一个 13B 模型会比同一系列中的 7B 模型更强大：它们是在相同的数据上训练的，但更大的“大脑”能够更好地保留知识并更连贯地思考。更大的模型也需要更多的 VRAM/RAM。
* 量化有不同的级别（8 位、5 位、4 位等）。你越往下走，模型的性能就越差，但硬件要求也越低。因此，即使在较差的硬件上，你也可能能够运行所需模型的 4 位版本。甚至还有 3 位和 2 位量化，但这时你已经在无效地重复了。还有进一步的量化子类型，如 k_s、k_m、k_l 等。k_m 比 k_s 更好，但需要更多资源。
* 上下文大小（你的对话可以变得多长而不被模型丢弃部分内容）也会影响 VRAM/RAM 的需求。值得庆幸的是，这是一个可配置的设置，允许你使用更小的上下文以减少 VRAM/RAM 的需求。（注意：基于 Llama2 的模型的上下文大小为 4k。Mistral 宣传为 8k，但实际上是 4k。）
* 在 2023 年的某个时候，NVIDIA 更改了他们的 GPU 驱动程序，因此如果你需要的 VRAM 超过了 GPU 的容量，任务不会崩溃，而是会开始使用常规 RAM 作为后备。这将降低 LLM 的写入速度，但模型仍然可以工作并提供相同质量的输出。值得庆幸的是，这种行为 [可以被禁用](https://nvidia.custhelp.com/app/answers/detail/a_id/5490)。

考虑到以上所有因素，硬件要求和性能完全取决于模型的系列、模型类型、模型大小、量化方法等。

#### 模型大小计算器
你可以使用 [Nyx's Model Size Calculator](https://huggingface.co/spaces/NyxKrage/LLM-Model-VRAM-Calculator) 来确定你需要多少 RAM/VRAM。

记住，你想要运行的模型应该是最大且量化程度最低的，能够适应你的内存，即不导致 [磁盘交换](https://serverfault.com/a/48487)。

## 下载 LLM

要开始，你需要下载一个 LLM。最常见的查找和下载 LLM 的地方是 HuggingFace。这里有成千上万的模型可供选择。查找模型的一个好方法是查看 TheBloke 的账户页面：<https://huggingface.co/TheBloke>。他是一位致力于将每个模型转换为 GGUF 的独行侠。如果你不想要 GGUF，他会链接到原始模型页面，你可能会在该页面找到其他格式的同一模型。

在某个模型的页面上，你会发现一堆文件。

* 你可能不需要所有文件！对于 GGUF，你只需要 .gguf 模型文件（通常为 4-11GB）。如果你发现多个大型文件，通常都是同一模型的不同量化版本，你只需选择一个。
* 对于 .safetensors 文件（可以是 GPTQ、AWQ 或 HF 量化或未量化），如果你在文件名中看到像 model-00001-of-00003.safetensors 的数字序列，那么你需要所有 3 个 .safetensors 文件 + 存储库中的所有其他文件（tokenizer、configs 等）来获取完整模型。
* 截至 2024 年 1 月，Mixtral MOE 8x7B 被广泛认为是本地 LLM 的最先进技术。如果你有 32GB 的 RAM 来运行它，绝对可以尝试。如果你的 RAM 少于 32GB，那么使用 Kunoichi-DPO-v2-7B，尽管它的大小，但它的表现非常出色。

### 下载 Kunoichi-DPO-v2-7B 的步骤

我们将在本指南的其余部分使用 Kunoichi-DPO-v2-7B 模型。它是一个基于 Mistral 7B 的优秀模型，仅需 7GB RAM，性能远超预期。注意：Kunoichi 使用 Alpaca 提示。

* 前往 <https://huggingface.co/brittlewis12/Kunoichi-DPO-v2-7B-GGUF>
* 点击“文件和版本”。您将看到几个文件的列表。这些都是相同的模型，但提供了不同的量化选项。点击文件 'kunoichi-dpo-v2-7b.Q6_K.gguf'，它提供了 6 位量化。
* 点击“下载”按钮。您的下载应该开始。

### 如何识别模型类型

好的模型上传者如 TheBloke 会给出描述性的名称。但如果没有：

* 文件名以 .gguf 结尾：GGUF CPU 模型（显而易见）
* 文件名以 .safetensors 结尾：可以是未量化的，或 HF 量化的，或 GPTQ，或 AWQ
* 文件名是 pytorch-***.bin：与上述相同，但这是一个较旧的模型文件格式，允许模型在加载时执行任意 Python 脚本，被认为是不安全的。如果你信任模型创建者，或者非常需要，可以继续使用，但如果有选择，建议使用 .safetensors。
* config.json 是否存在？查看它是否有 quant_method。
* q4 表示 4 位量化，q5 表示 5 位量化，依此类推。
* 你看到一个数字如 -16k？那是增加的上下文大小（即在模型忘记你聊天开始之前，你的对话可以有多长）！请注意，较高的上下文大小需要更多的 VRAM。

## 安装 LLM 服务器：Oobabooga 或 KoboldAI

在您的 PC 上安装 LLM 后，我们需要下载一个工具，它将充当 SillyTavern 和模型之间的中介：它将加载模型，并将其功能作为本地 HTTP Web API 暴露出来，以便 SillyTavern 可以与之通信，方式与 SillyTavern 与 OpenAI GPT 或 Claude 等付费 Web 服务的通信方式相同。您使用的工具应该是 KoboldAI 或 Oobabooga（或其他兼容工具）。

本指南的其余部分将继续使用 Oobabooga，但这些工具应视为等效。

### 安装 Oobabooga

这是一个更正确/简单易懂的安装步骤：

1. git clone <https://github.com/oobabooga/text-generation-webui>（或者在浏览器中下载他们的仓库为 .zip 文件，然后解压）
2. 运行 start_windows.bat 或者根据你的操作系统选择相应的文件
3. 当被询问时，选择你的 GPU 类型。即使你打算使用 GGUF/CPU，如果你的 GPU 在列表中，现在也请选择，因为这将给你提供一个名为 GPU sharding 的速度优化选项（而不需要从头重新安装）。如果你没有游戏级的 dGPU（NVIDIA, AMD），请选择 None。
4. 等待安装完成
5. 将 kunoichi-dpo-v2-7b.Q6_K.gguf 放入 text-generation-webui/models
6. 打开 text-generation-webui/CMD_FLAGS.txt，删除里面的所有内容并写入： --api
7. 重启 Oobabooga
8. 访问 <http://127.0.0.1:5000/docs>。是否加载了 FastAPI 页面？如果没有，你在某个地方出错了。

### 在 Oobabooga 中加载我们的模型

1. 在浏览器中打开 <http://127.0.0.1:7860/>
2. 点击模型标签
3. 在下拉菜单中，选择我们的 Kunoichi DPO v2 模型。它应该会自动选择 llama.cpp 加载器。
4. （可选）我们在之前提到过“GPU 卸载”：这就是此页面上的 n-gpu-layers 设置。如果您想使用它，请在加载模型之前设置一个值。作为基本参考，将其设置为 30 将为 13B 及更小的模型使用不到 6GB 的 VRAM。（这因模型架构和大小而异）
5. 点击加载

### 配置 SillyTavern 与 Oobabooga 进行对话

1. 点击 API 连接（顶部栏的第二个按钮）
2. 将 API 设置为文本完成
3. 将 API 类型设置为默认（Oobabooga）
4. 将服务器 URL 设置为 <http://127.0.0.1:5000/>
5. 点击连接。它应该成功连接并检测到 kunoichi-dpo-v2-7b.Q6_K.gguf 作为模型。
6. 与角色聊天以测试其是否正常工作

## 结论

恭喜，您现在应该拥有一个可工作的本地 LLM。

## 第2部分：配置SillyTavern以获得更好的LLM输出

所以你和机器人聊天，感觉效果不太好。可能是模型不佳。也许你无法运行更好的模型，这就是最好的结果。但也许你可以修复它。

请在下一个指南中阅读，[如何提高输出质量](https://docs.sillytavern.app/usage/local-llm-guide/how-to-improve-output-quality/)