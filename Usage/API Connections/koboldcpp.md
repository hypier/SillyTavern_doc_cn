# KoboldCpp
KoboldCpp 是一个独立的 API，用于 GGML 和 GGUF 模型。

这个 [VRAM 计算器](https://huggingface.co/spaces/NyxKrage/LLM-Model-VRAM-Calculator) 由 Nyx 提供，可以告诉你你的模型大约需要多少 RAM/VRAM。

## Nvidia GPU 快速入门
本指南假设您正在使用 Windows。
* 下载最新版本： https://github.com/LostRuins/koboldcpp/releases
* 启动 KoboldCpp。您可能会看到来自 Microsoft Defender 的弹出窗口，点击 `Run Anyway`。
* 从版本 1.58 开始，KoboldCpp 应该看起来像这样：

![KoboldCpp 1.58](/static/koboldcpp.png)

* 在 `Quick Launch` 标签下，选择模型和您偏好的 `Context Size`。
* 选择 `Use CuBLAS`，并确保 `GPU ID` 旁边的黄色文本与您的 GPU 匹配。
* 即使您的 VRAM 较低，也不要勾选 `Low VRAM`。
* 除非您拥有 Nvidia 10 系列或更早的 GPU，否则请取消勾选 `Use QuantMatMul (mmq)`。
* 加载模型时，`GPU Layers` 应该已经填充。现在先保持不变。
* 在 `Hardware` 标签下，勾选 `High Priority`。
* 点击 `Save`，这样您就不必在每次启动时配置 KoboldCpp。
* 点击 `Launch`，等待模型加载。

您应该会看到类似以下内容：
```
Load Model OK: True
Embedded Kobold Lite loaded.
Starting Kobold API on port 5001 at http://localhost:5001/api/
Starting OpenAI Compatible API on port 5001 at http://localhost:5001/v1/
======
Please connect to custom endpoint at http://localhost:5001
```
您现在可以在 SillyTavern 中使用 `http://localhost:5001` 作为 API URL 连接到 KoboldCpp 并开始聊天。

**恭喜！您完成了！**

差不多。

### GPU Layers
KoboldCpp 正在运行，但您可以通过确保尽可能多的层被卸载到 GPU 来提高性能。您应该在终端中看到类似以下内容：
```
llm_load_tensors: offloading 9 repeating layers to GPU
llm_load_tensors: offloaded 9/33 layers to GPU
llm_load_tensors:        CPU buffer size = 25215.88 MiB
llm_load_tensors:      CUDA0 buffer size =  7043.34 MiB
....................................................................................................
llama_kv_cache_init:  CUDA_Host KV buffer size =  1479.19 MiB
llama_kv_cache_init:      CUDA0 KV buffer size =   578.81 MiB
```

不要害怕数字；这一部分比看起来要简单。`CPU buffer size` 指的是系统 RAM 的使用量。忽略它。`CUDA0 buffer size` 指的是 GPU VRAM 的使用量。`CUDA_Host KV buffer size` 和 `CUDA0 KV buffer size` 指的是为您的模型上下文分配的 GPU VRAM。在这种情况下，KoboldCpp 使用了大约 9 GB 的 VRAM。

我有 12 GB 的 VRAM，只有 2 GB 的 VRAM 用于上下文，所以我还有大约 10 GB 的 VRAM 可用于加载模型。由于 9 层使用了大约 7 GB 的 VRAM，`7000 / 9 = 777.77`，我们可以假设每层大约使用 `777.77 MIB` 的 VRAM。`10,000 MIB / 777.77 = 12.8`，所以我将取整并从现在开始加载 12 层模型。

现在使用您的模型、上下文大小和系统的 VRAM 进行计算，并重启 KoboldCpp：
* 如果您聪明，之前点击过 `Save`，现在可以用 `Load` 加载您之前的配置。否则，选择您之前选择的相同设置。
* 将 `GPU Layers` 更改为您新的、经过 VRAM 优化的数字（在我的例子中是 12 层）。
* 点击 `Save` 保存您的更新配置。

您现在应该看到类似以下内容：
```
llm_load_tensors: offloading 12 repeating layers to GPU
llm_load_tensors: offloaded 12/33 layers to GPU
llm_load_tensors:        CPU buffer size = 25215.88 MiB
llm_load_tensors:      CUDA0 buffer size =  9391.12 MiB
....................................................................................................
llama_kv_cache_init:  CUDA_Host KV buffer size =  1286.25 MiB
llama_kv_cache_init:      CUDA0 KV buffer size =   771.75 MiB
```

KoboldCpp 正在使用我 12 GB VRAM 的大约 11.5 GB。这应该比 KoboldCpp 自动生成的设置性能更好。

**恭喜！您（实际上）完成了！**

要更深入了解 KoboldCpp 设置，请查看 Kalmomaze 的 [简单 Llama + SillyTavern 设置指南](https://rentry.org/llama_v2_sillytavern)。