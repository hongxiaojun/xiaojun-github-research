# xiaojun-github-research - GitHub 项目架构考古

基于 Yao Meta Skill 方法论重构的 Production 级技能。

## 技能概述

**名称**：xiaojun-github-research  
**版本**：3.0.0  
**原型**：Production  
**描述**：深度架构分析 GitHub 项目 - 逆向拆解核心抽象、工程克制、代码美学，并触发架构师式对话。

## 重构亮点

### 从 237 行单文件到模块化结构

**重构前**：
- 单个 skill.md 文件（237 行）
- 混合了路由、执行、参考等多个职责
- 难以维护和扩展

**重构后**：
```
xiaojun-github-research/
├── SKILL.md                    # 核心入口（70 行，仅路由 + 最小执行骨架）
├── agents/
│   └── interface.yaml          # Agent 接口配置
├── references/                 # 详细参考文档
│   ├── four-dimensions.md      # 四维分析框架详解
│   ├── quality-standards.md    # 质量标准详解
│   ├── prohibitions.md         # 禁止事项详解
│   └── examples.md             # 示例分析
├── scripts/
│   └── quality-check.sh        # 质量检查脚本
├── evals/                      # 评估测试（待添加）
├── reports/                    # 分析报告（输出目录）
└── skill.md.backup             # 原始文件备份
```

## 核心改进

### 1. 精简的 SKILL.md（70 行）
- **触发词**：明确的路由规则
- **排除项**：清晰的边界定义
- **执行流程**：4 个阶段的简要说明
- **质量标准**：5 个必须通过的检查项

### 2. 模块化的参考文档
- **four-dimensions.md**：四维分析框架的详细指导
- **quality-standards.md**：每个维度的质量检查点和优秀示例
- **prohibitions.md**：禁止事项的详细说明和替代方案
- **examples.md**：完整的分析示例（micrograd、个人网站等）

### 3. 可执行的质量检查
- **scripts/quality-check.sh**：自动化的质量检查脚本
- 确保每次输出都符合标准

### 4. Agent 接口配置
- **agents/interface.yaml**：标准化的 agent 配置
- 清晰的角色、目标、约束和工作流程

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

## 与旧版本的对比

| 维度 | 旧版本 | 新版本 |
|------|--------|--------|
| **视角** | 文科生视角的代码考古 | 架构师视角的深度分析 |
| **文件结构** | 单文件 237 行 | 模块化 7 个文件 |
| **分析框架** | 4 个叙事性维度 | 4 个技术性深度分析维度 |
| **输出形式** | 单向输出文档 | 结对编程式对话 |
| **核心焦点** | 项目价值 | 架构直觉和工程美学 |
| **质量保证** | 自查清单 | 自动化脚本 + 自查清单 |

## 技术债务

### 待完成项
- [ ] 添加 `evals/` 目录的触发词评估脚本
- [ ] 添加 `evals/` 目录的质量门测试
- [ ] 创建 `manifest.json` 用于 Library 级别推广
- [ ] 添加 `reports/` 目录的示例报告

### 可选优化
- [ ] 添加 `scripts/trigger-eval.py` 用于优化触发词
- [ ] 添加 `scripts/optimize-description.py` 用于优化描述
- [ ] 添加 `scripts/resource-boundary-check.py` 用于资源边界检查

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

## 许可

遵循 Yao Meta Skill 的方法论和最佳实践。

---

**重构完成时间**：2026-05-24  
**重构方法**：Yao Meta Skill v1.0  
**原型级别**：Production
