这两个是ai基础模型的实现

### 概念范畴

基础工具：gemini cli 被动响应

IDE插件/Plugin： Copilot, Code Assist  补全代码句子

智能体/Agent: Cursor(Agent Mode) 主动执行  即是ide又是agent

前两者是辅助工具，
### CLI和Agents区别

开发者工具/cli/IDE：
	geminil cli
	Cursor
Agents/Workflows:
	ai gent
区别：

ai开发者工具通常是**被动**的。你输入一个指令（比如 `gemini "解释这段代码"`），它返回一个结果,侧重于交互。

Agent它具有**主动性（Agentic）**。一个智能体可以自己思考：“为了完成这个目标，我第一步该干什么，第二步该查什么，如果报错了该怎么修”。它不需要你一步步下指令，而是给你一个**结果**，侧重于自主能力。

