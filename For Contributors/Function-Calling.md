---
order: -1
icon: code-review
---

# 函数调用

函数调用允许通过让 LLM 使用结构化数据来为您的扩展添加动态功能，您可以使用这些数据触发扩展的特定功能。

## 限制和前提条件

1. 此功能仅适用于某些聊天完成源：OpenAI、Claude、MistralAI、Cohere、Groq、OpenRouter 和 Perplexity。
2. 文本完成 API 不支持函数调用，但其中一些选择性地允许 JSON 语法模式。它们的使用超出了本文档的范围。
3. 对于使用流式传输的请求，函数调用功能不可用。注意：后台（“安静”）请求永远不会流式传输，即使启用了流式传输。
4. 函数调用的支持必须首先由用户明确允许。这可以通过在 AI 响应配置面板中启用“启用函数调用”选项来完成。
5. 当调用一个函数时，正常的响应生成将被抑制，因此在调用 `generateQuietPrompt` 时，您可能会收到一个空响应。
6. 无法保证 LLM 会执行任何函数调用。始终提供备份计划，以防函数调用未执行。
7. 要确定是否支持函数调用功能，可以调用从 `openai.js` 模块导出的 `isFunctionCallingSupported`。

## 注册一个函数

当准备一个聊天完成请求时，通过 `eventSource` 对象发出一个类型为 `LLM_FUNCTION_TOOL_REGISTER` 的事件。注册是临时的，在请求之间不会持久化，因此您可以始终根据事件数据决定是否跳过注册。

事件数据包括以下内容：触发事件的生成类型、生成数据（包括采样参数和组装的聊天提示）以及用于注册函数工具的回调函数。要注册一个函数，您必须调用 `registerFunctionTool` 函数，并传入以下参数：

1. `name`: 要注册的函数工具的名称。这将在后续作为调用该函数的标识符使用。
2. `description`: 函数工具的描述。它帮助语言模型更好地理解该工具的用途。
3. `params`: 函数工具参数的 JSON 架构。模型将尝试生成一个与此架构匹配的 JSON 对象，用于函数调用的参数。
4. `required`: 是否强制使用该函数工具。这只是一个提示，并不能保证您的函数会被执行，特别是如果 API 不支持同时调用或没有提供强制执行的方式。

```ts
/**
 * Callback data for the `LLM_FUNCTION_TOOL_REGISTER` event type that is triggered when a function tool can be registered.
 */
interface FunctionToolRegister {
    /**
     * The type of generation that is being used
     */
    type?: string;
    /**
     * Generation data, including messages and sampling parameters
     */
    data: Record<string, object>;
    /**
     * Callback to register an LLM function tool.
     */
    registerFunctionTool: typeof registerFunctionTool;
}

/**
 * Callback data for the `LLM_FUNCTION_TOOL_REGISTER` event type that is triggered when a function tool is registered.
 * @param name Name of the function tool to register
 * @param description Description of the function tool
 * @param params JSON schema for the parameters of the function tool
 * @param required Whether the function tool should be forced to be used
 */
declare function registerFunctionTool(name: string, description: string, params: object, required: boolean): Promise<void>;
```

## 处理成功的调用

当在LLM的输出中检测到函数调用时，将通过`eventSource`对象发出类型为`LLM_FUNCTION_TOOL_CALL`的事件。事件数据包括以下内容：被调用的函数工具的名称和以JSON序列化字符串形式传递给函数的参数。

**注意：** 在使用参数之前，请务必检查参数是否可以正确解析并进行基本验证！有时模型生成的JSON可能不正确。

```ts
/**
 * Callback data for the `LLM_FUNCTION_TOOL_CALL` event type that is triggered when a function tool is called.
 */
interface FunctionToolCall {
    /**
     * Name of the function tool to call
     */
    name: string;
    /**
     * JSON object with the parameters to pass to the function tool
     */
    arguments: string;
}
```

## 示例代码

函数注册和使用的示例：

```js
// Not supported? Alas, resort to other methods.
if (!isFunctionCallingSupported()) {
    return;
}

// Setup a callback for the `LLM_FUNCTION_TOOL_REGISTER` event.
// Use `on` to always listen to the event, or `once` to listen only once.
eventSource.on(event_types.LLM_FUNCTION_TOOL_REGISTER, (args) => {
    // Only trigger on quiet mode
    if (args.type !== 'quiet') {
        return;
    }

    // Register the function tool
    args.registerFunctionTool(
        // name
        'getWeather',
        // description
        '获取特定位置的天气',
        // params
        {
            $schema: 'http://json-schema.org/draft-04/schema#',
            type: 'object',
            properties: {
                location: {
                    type: 'string',
                    description: '获取天气的地点',
                },
            },
            required: [
                'location',
            ],
        },
        // required
        true,
    );
});

// Setup a callback for the `LLM_FUNCTION_TOOL_CALL` event.
// Respond to a function call if it matches the registered function tool.
eventSource.on(event_types.LLM_FUNCTION_TOOL_CALL, (args) => {
    if (args.name !== 'getWeather') {
        return;
    }

    try {
        // Expecting JSON data like: { "location": "Sacramento" }
        const data = JSON.parse(args?.arguments);

        // No actual data to work with
        if (!data.location) {
            return;
        }

        // Do something with the data
        getWeather(data.location);
    } catch {
        // Model did not generate correct JSON
    }
});

// Make a quiet prompt generation request. This will trigger the `LLM_FUNCTION_TOOL_REGISTER` event.
// Imagine that the prompt is coming from a user input.
const response = await generateQuietPrompt('萨克拉门托的天气如何？', false, false);
// Response may contain a plain text response in case no tools were called. Always check for this.

if (response) {
    // Do something with the response
}
```