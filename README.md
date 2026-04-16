# GitHub 项目考古研究

> 深度分析开源项目的核心洞察、架构设计与价值意义

## 📚 文章列表

### [网络的网络：Mixin Network 代码考古](网络的网络-Mixin%20Network代码考古.md)
- **项目**：[MixinNetwork/mixin](https://github.com/MixinNetwork/mixin)
- **Stars**：⭐ 525 | **语言**：Go | **创建时间**：2018-04-27
- **核心洞察**：基于 BFT-DAG 的分布式网络，通过异步共识实现可扩展性。从单核到多核：将区块链从"全球单线程计算机"转变为"分布式多线程网络"。

### [信任的分解：Mixin Safe 代码考古](信任的分解-Mixin%20Safe代码考古.md)
- **项目**：[MixinNetwork/safe](https://github.com/MixinNetwork/safe)
- **Stars**：⭐ 24 | **语言**：Go, Rust | **创建时间**：2023-03-05
- **核心洞察**：基于 MPC 的非托管多重签名钱包，通过分布式密钥生成消除单点故障。从"信任一个人"到"信任一群人"：密码学实现的多维度信任分解。

### [入口的争夺：Mixin iOS App 代码考古](入口的争夺-Mixin%20iOS%20App代码考古.md)
- **项目**：[MixinNetwork/ios-app](https://github.com/MixinNetwork/ios-app)
- **Stars**：⭐ 518 | **语言**：Swift, Objective-C | **创建时间**：2018-04-27
- **核心洞察**：Mixin Network 官方 iOS 客户端，整合消息、钱包和 DApp 浏览器。在中心化平台上构建去中心化体验：平台政治与超级应用的困境。

### [AI 的范式转移：VMark 代码考古](AI的范式转移-VMark代码考古.md)
- **项目**：[xiaolai/vmark](https://github.com/xiaolai/vmark)
- **Stars**：⭐ 257 | **语言**：TypeScript, Rust | **创建时间**：2026-01-03
- **核心洞察**：AI 原生 Markdown 编辑器，完全由 AI 在人类监督下编写。从"AI 辅助人类"到"人类监督 AI"：软件开发的新分工模式。

## 🤖 自动化工具

本系列文章使用 **xiaojun-github-research** 技能自动生成。

该技能封装了完整的 GitHub 项目考古工作流程：
1. 克隆仓库（使用 `git clone --depth 1 --single-branch`）
2. 获取项目元数据（通过 GitHub API）
3. 分析代码结构和核心文件
4. 撰写深度分析报告
5. 清理临时文件

技能文档位于：[`xiaojun-github-research/`](xiaojun-github-research/)

## 📝 写作方法论

本系列文章遵循"思想考古学家与科技散文家"方法论：

- **逻辑逆向**：从元问题出发，理解作者看到了什么别人没看到的痛点
- **架构叙事**：通过代码片段和设计决策，解释技术的"变态之处"
- **价值升华**：从哲学/政治经济学角度，分析系统的优缺点和未解挑战
- **类比金线**：使用高阶类比，确保结构等价性、认知杠杆率、生成预测力、审美溢价

### 禁止使用的词汇
- 本质、精妙、博弈、结晶、颠覆、革命性
- 以及其他技术黑话和陈词滥调

### 禁止使用的类比
- 炒菜、排队、邻居、盖房子、拆砖头
- 以及其他低幼化、过度简化的类比

## 👤 作者

- **GitHub**：[hongxiaojun](https://github.com/hongxiaojun)
- **工具**：Claude Code
- **更新时间**：2026-04-16

## 📄 许可证

本项目内容采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) 许可证。
