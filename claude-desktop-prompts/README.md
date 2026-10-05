# Claude Desktop 内置 Cowork 提示词：英文原文与中文翻译

本仓库保存从 **Claude Desktop 2.19675.0.0（Windows ARM64）** 本地安装包中提取的 Cowork 静态提示词模板，以及对应的完整简体中文翻译。检查日期：2026-10-05。

## 阅读

- [第三轮：会话状态、停止与恢复、后台调度、审批，以及仪表盘单次模型调用](round3/README.md)
- [第二轮：提示词装配、记忆与执行边界，以及五份补充提示词的中英对照](round2/README.md)
- [完整中文翻译](prompts/cowork-bundled-system-prompt.zh-CN.txt)
- [完整英文原文](prompts/cowork-bundled-system-prompt.en.txt)
- [来源与文件校验信息](provenance.json)
- [翻译结构检查结果](translation-check.json)

英文原文 441 行，37,811 个字符，按空白分隔约 5,701 个词；中文译文保持相同的行数、空行位置、XML 标签、运行时占位符和行内代码。结构检查不等于人工认证的翻译质量保证；原文中的日期、示例、重复和不一致之处均按原文保留。

## 这份提示词的范围

提取位置是安装包 `app/resources/app.asar` 中的 `.vite/build/index.chunk-BcYASoX4.js`。模板变量名为 `jWt`，导出别名为 `TU`。

另一个打包文件 `.vite/build/index.chunk-cz5TcuPg.js` 中，模板选择函数 `$u(e,n)` 在 provider 类型为 `3p` 时选择该内置模板，并拼接附加内容；其他分支返回传入的基础提示词。

会话启动还会读取 `coworkSyspromptMap`，按模型选择提示词变体，再组合技能、项目上下文、记忆、工具和组织策略。因此，这里收录的是**特定版本客户端中存在的静态模板**，不能据此认定它等于任意普通 Claude 账号某次会话收到的完整最终系统提示词。

模板内容已与同版本安装包的汉化前备份进行比较，内容完全一致。`provenance.json` 中的安装包哈希对应本次实际检查的本地安装包；该安装包所在客户端曾进行汉化，不将这一哈希标称为官方原版安装包哈希。

## 内容概览

模板包含产品身份与表达风格、如何回应批评、澄清需求、任务列表、最终验收、工具与网络使用规则、技能加载以及文件创建与交付要求。

其中写明产品基于 Claude Code 和 Agent SDK，并规定何时讨论这些底层实现。它也约束语气和措辞、工具失败后的替代行为，以及任务在界面中的呈现方式。这些文字说明了产品如何引导模型执行任务；静态检查本身不能证明规则每次都被执行，也不能用来衡量模型的实际能力。

## 整理方法

只读检查本地安装文件，提取模板字符串并还原 JavaScript 字符转义，再逐行翻译为简体中文。校验包括行数、空行位置、占位符、XML 标签、行内代码以及文件 SHA-256。没有通过向模型发送诱导请求获取文本。

本仓库只收录模板文本、译文和相关说明，不包含完整应用程序、聊天记录、账号信息、访问凭据或个人文件路径。

## 归属

Claude 与 Claude Desktop 是 Anthropic 的产品；英文模板内容归其相应权利人所有。本仓库为非官方研究整理与翻译，与 Anthropic 无隶属关系。收录内容不因本仓库公开而自动获得新的开源许可。

## English scope note

This is an unofficial archive and Simplified Chinese translation of a static Cowork prompt template extracted from Claude Desktop 2.19675.0.0 for Windows ARM64. The bundled template is selected explicitly in the `3p` provider branch. Runtime prompts may incorporate model-specific and session-specific content; this archive does not establish the complete effective system prompt of a normal Claude session. Anthropic and the respective rights holders retain rights in the original content.
