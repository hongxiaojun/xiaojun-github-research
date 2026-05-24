---
name: xiaojun-github-research
description: 深度架构分析 GitHub 项目 - 逆向拆解核心抽象、工程克制、代码美学，并触发架构师式对话。适用于"分析 GitHub 项目"、"项目考古"、"代码架构分析"等请求。不适用：简单代码说明、功能罗列、使用文档生成。
version: 3.0.0
archetype: Production
---

# GitHub 项目架构考古

## 触发词

分析以下请求时应路由到此技能：
- "分析 GitHub 项目 [url]"
- "项目考古 [url]"
- "代码架构分析 [url]"
- "深度拆解 [project]"
- "从架构师视角分析 [repo]"

## 排除项

这些请求**不应**路由到此技能：
- 简单的代码说明或功能介绍 → 使用通用代码分析
- API 文档生成 → 使用文档生成工具
- 使用教程编写 → 使用技术写作工具
- 快速代码浏览 → 使用 IDE 或 GitHub Web

## 执行流程

### Phase 1: 项目克隆与元数据提取
\`\`\`bash
# 克隆项目（浅克隆）
cd /tmp && git clone --depth 1 --single-branch <URL>

# 获取 GitHub 元数据
curl -s "https://api.github.com/repos/<owner>/<repo>" | \
  jq '{name, stargazers_count, created_at, language, topics}'

# 扫描项目结构
tree -L 2 -I 'node_modules|vendor|dist|build' /tmp/<dir>
\`\`\`

### Phase 2: 四维深度分析

执行四维分析框架（详见 `references/four-dimensions.md`）：

1. **架构思想**：识别核心数据结构和第一性原理
2. **工程克制**：找出作者刻意拒绝的 3 个复杂性
3. **设计美学**：展示 10-30 行最优雅代码并分析认知负荷
4. **Sparring Session**：基于代码提出挑战性架构问题

### Phase 3: 质量检查

执行 `scripts/quality-check.sh` 确保输出符合标准。

### Phase 4: 文档保存与清理
\`\`\`bash
# 保存报告
mkdir -p "/Users/add/Documents/架构师视角看 GitHub 项目"
# 文件命名：[核心抽象]-[项目名]架构拆解.md

# 清理临时文件
rm -rf /tmp/<项目目录>
\`\`\`

## 质量标准

必须通过的质量门（详见 `references/quality-standards.md`）：
- ✅ 找到项目的"最小不可分割单元"
- ✅ 指出 3 件作者"故意不做"的事
- ✅ 展示 10-30 行具体代码并量化认知负荷
- ✅ 提出触发深度思考的架构问题
- ✅ 无平庸的按文件总结、API 罗列、空泛赞美

## 详细参考

- 四维分析框架：`references/four-dimensions.md`
- 质量标准：`references/quality-standards.md`
- 禁止事项：`references/prohibitions.md`
- 示例分析：`references/examples.md`
