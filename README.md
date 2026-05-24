# xiaojun-github-research - GitHub 项目架构考古

深度架构分析 GitHub 项目的技能 - 逆向拆解核心抽象、工程克制、代码美学，并触发架构师式对话。

## 技能概述

**名称**：xiaojun-github-research  
**版本**：3.0.0  
**描述**：像顶级系统架构师（Andrej Karpathy 风格）逆向拆解项目的架构直觉、工程克制和代码美学

## 技能结构

```
xiaojun-github-research/
├── SKILL.md                    # 核心技能定义（70 行）
├── README.md                   # 项目文档
├── agents/
│   └── interface.yaml          # Agent 接口配置
├── references/                 # 详细参考文档
│   ├── four-dimensions.md      # 四维分析框架详解
│   ├── quality-standards.md    # 质量标准详解
│   ├── prohibitions.md         # 禁止事项详解
│   └── examples.md             # 示例分析（micrograd 等）
├── scripts/
│   └── quality-check.sh        # 质量检查脚本
└── evals/                      # 评估测试（待添加）
```

## 四维分析框架

### 维度 1：架构思想 - 寻找第一性原理
- 识别核心数据结构
- 解释最小不可分割单元
- 分析数据流动的单一事实来源

### 维度 2：工程思想 - 识别"刻意的克制"
- 指出 3 件作者"故意不做"的事
- 分析这些选择带来的反馈回路优势
- 检查 Fail-Fast 原则的体现

### 维度 3：设计美学 - 定位"认知降噪点"
- 展示 10-30 行最优雅代码
- 分析线性控制流、命名精确性、高内聚性
- 量化认知负荷（思维栈帧数量）

### 维度 4：Sparring Session - 挑战性对话
- 基于代码分析提出深度架构问题
- 等待用户回应，形成交互式对话

## 质量标准

### 必须通过的检查
- ✅ 找到项目的"最小不可分割单元"
- ✅ 指出 3 件作者"故意不做"的事
- ✅ 展示 10-30 行具体代码并量化认知负荷
- ✅ 提出触发深度思考的架构问题
- ✅ 无平庸的按文件总结、API 罗列、空泛赞美

## 使用示例

```bash
# 触发技能
/xiaojun-github-research https://github.com/karpathy/micrograd

# 技能将自动：
# 1. 克隆项目
# 2. 执行四维分析
# 3. 生成报告
# 4. 保存到 ~/Documents/架构师视角看 GitHub 项目/
```

## 触发词

分析以下请求时技能会被激活：
- "分析 GitHub 项目 [url]"
- "项目考古 [url]"
- "代码架构分析 [url]"
- "深度拆解 [project]"
- "从架构师视角分析 [repo]"

## 排除项

这些请求不会激活此技能：
- 简单的代码说明或功能介绍
- API 文档生成
- 使用教程编写
- 快速代码浏览

## 核心特性

### 1. 精简的 SKILL.md（70 行）
- 明确的路由规则
- 清晰的边界定义
- 4 个阶段的执行流程
- 5 个必须通过的质量门

### 2. 模块化的参考文档
- **four-dimensions.md**：四维分析框架的详细指导和质量检查点
- **quality-standards.md**：每个维度的质量标准和优秀示例
- **prohibitions.md**：禁止事项的详细说明和替代方案
- **examples.md**：完整的分析示例（micrograd、个人网站等）

### 3. 可执行的质量检查
- **scripts/quality-check.sh**：自动化的质量检查脚本
- 确保每次输出都符合标准

### 4. Agent 接口配置
- **agents/interface.yaml**：标准化的 agent 配置
- 清晰的角色、目标、约束和工作流程

## 安装

### 方法 1：从 GitHub 安装
```bash
cd ~/.claude/skills/
git clone https://github.com/hongxiaojun/xiaojun-github-research.git
```

### 方法 2：手动安装
```bash
cd ~/.claude/skills/
mkdir -p xiaojun-github-research
# 复制所有技能文件到该目录
```

## 维护指南

### 更新四维分析框架
编辑 `references/four-dimensions.md`

### 更新质量标准
编辑 `references/quality-standards.md`

### 更新禁止事项
编辑 `references/prohibitions.md`

### 添加新示例
编辑 `references/examples.md`

### 修改执行流程
编辑 `SKILL.md` 的执行流程部分

## 输出报告示例

分析报告包含以下部分：
- 核心抽象（最小不可分割单元）
- 工程克制（作者拒绝的 3 个复杂性）
- 设计美学（最优雅代码的认知负荷分析）
- 架构师对白（挑战性架构问题）

## 许可

MIT License

---

**版本**：3.0.0  
**最后更新**：2026-05-24
