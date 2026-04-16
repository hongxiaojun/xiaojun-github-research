---
name: xiaojun-github-research
description: GitHub 项目考古研究 - 深度分析 GitHub 项目的核心洞察、架构设计和价值意义。适用于"分析 GitHub 项目"、"项目考古"、"代码考古"等请求。
version: 1.0.0
---

# GitHub 项目考古研究 Skill

## 用途

当用户请求分析 GitHub 项目时（如"分析 https://github.com/user/repo"），执行以下流程：

1. **克隆项目**：使用 `git clone --depth 1 --single-branch`（不用 MCP）
2. **获取项目信息**：通过 GitHub API 获取 star 数量、创建时间、作者等
3. **分析代码结构**：读取 README、核心代码文件，理解架构设计
4. **撰写深度报告**：遵循"思想考古学家"方法论，输出高质量分析文档
5. **保存文档**：保存到指定路径
6. **清理临时文件**：删除克隆的仓库

## 输出文档结构

```
[项目基本信息表格]

---

## 一、核心洞察：[元问题]
- 逻辑逆向：作者看到了什么别人没看到的痛点？

---

## 二、架构叙事：[规则逻辑]
- 2-4 个核心技术点
- 引用具体代码文件
- 解释设计的"变态之处"
- 使用高阶类比（如果满足金线标准）

---

## 三、现实世界的镜像：[跨领域映射]
- 对行业/生产力模式的冲击
- 与传统解决方案的对比

---

## 四、价值总结：[哲学/政治经济学]
- 系统的优缺点
- 未解的挑战

---

## 后记：[设计者意图]
```

## 写作准则（严格遵守）

### 禁止词汇
- 本质、精妙、博弈、结晶、颠覆、革命性、前所未有、完美、极致、终极

### 禁止类比
- 炒菜、排队、邻居、盖房子、拆砖头、购物车、餐厅、快递

### 类比使用四准则
1. **结构等价性**：逻辑链路是否严丝合缝对应？
2. **认知杠杆率**：是否用极低成本撬动极高深度？
3. **生成预测力**：读者能否预判出项目的下一个特征？
4. **审美溢价**：是否使用了跨学科的高级概念？

### 最重要的原则：克制
- 如果逻辑已清晰，用术语替代类比
- 当类比不满足金线标准时，删除类比

## 执行命令

```bash
# 克隆项目
cd /tmp && git clone --depth 1 --single-branch <项目 URL>

# 获取项目信息
curl -s "https://api.github.com/repos/<owner>/<repo>" | jq '{name, stargazers_count, created_at, html_url, owner: .owner.login}'

# 查看项目结构
ls -la /tmp/<项目目录>

# 读取 README
find /tmp/<项目目录> -name "README*"

# 根据项目类型选择核心代码
# Go: main.go, pkg/, internal/
# JS/TS: src/, package.json
# Rust: src/main.rs, Cargo.toml
# C++: src/, include/
# Swift: *.swift, AppDelegate.swift

# 保存文档
mkdir -p "/Users/xiaojun/Documents/文科生视角看 GitHub 项目文档"
# 使用 Write 工具保存

# 清理临时文件
rm -rf /tmp/<项目目录>
```

## 保存路径

```
/Users/xiaojun/Documents/文科生视角看 GitHub 项目文档/[核心概念]-[项目名]代码考古.md
```

## 示例

参考已完成的报告：
- 时间的结晶-Bitcoin核心代码考古.md
- 网络的网络-Mixin Network代码考古.md
- 信任的分解-Mixin Safe代码考古.md
- 入口的争夺-Mixin iOS App代码考古.md
- AI的范式转移-VMark代码考古.md

## 相关文件

- 写作指南：`/Users/xiaojun/.claude/projects/-Users-xiaojun/memory/GITHUB_ARCHAEOLOGY_WRITING_GUIDE.md`
- 主记忆：`/Users/xiaojun/.claude/projects/-Users-xiaojun/memory/MEMORY.md`
