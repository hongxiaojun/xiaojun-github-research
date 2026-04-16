# AI 的范式转移：VMark 代码考古

## 项目基本信息

| 属性 | 信息 |
|------|------|
| **项目名称** | VMark |
| **作者/团队** | [xiaolai](https://github.com/xiaolai) (笑来) |
| **项目地址** | [https://github.com/xiaolai/vmark](https://github.com/xiaolai/vmark) |
| **Star 数量** | ![GitHub Repo stars](https://img.shields.io/github/stars/xiaolai/vmark?style=social) (257) |
| **创建时间** | 2026-01-03 |
| **主要语言** | TypeScript, Rust |
| **项目简介** | AI 原生 Markdown 编辑器 |

---

## 一、核心洞察：当 AI 成为默认之时

2026 年初，一个名为 VMark 的 Markdown 编辑器在 GitHub 上开源，它的 README 中有一句看似平淡却极具争议性的声明：

> "VMark is **vibe-coded** — written entirely by AI under human supervision."

这句话隐藏着一个正在发生的范式转移：**我们正在从"人类编写代码，AI 辅助"，转向"AI 编写代码，人类监督"**。

VMark 的设计问题不是"如何构建一个更好的 Markdown 编辑器"，而是**当 AI 成为第一生产力时，软件开发应该如何重构**。

传统开发模式中，代码是核心资产。但在 VMark 中，**代码只是配置的产物**。在 `CODING_GUIDE.md` 中，第一条规则是：

> **Keep files under ~300 lines** — split proactively.

这不是随意的数字，而是**AI 上下文窗口优化的结果**。AI 模型的上下文窗口是有限的，如果文件太大，AI 就无法"看到"全貌，生成的代码就会出错。

在 `src/stores/` 目录中，你可以看到这种设计的具体体现：

```typescript
// src/stores/editorStore.ts
import { create } from 'zustand';
import { subscribeWithSelector } from 'zustand/middleware';

export interface EditorState {
  documents: Document[];
  activeDocId: string | null;
  // ... 其他状态
}

export const useEditorStore = create<EditorState>()(
  subscribeWithSelector((set, get) => ({
    documents: [],
    activeDocId: null,
    // ... 实现
  }))
);
```

这个文件只有不到 200 行，职责清晰：管理编辑器的文档状态。如果需要修改状态逻辑，AI 只需要理解这个文件，而不需要理解整个代码库。

---

## 二、架构叙事：为 AI 优化的代码结构

### 2.1 文件拆分：上下文窗口的约束

在 `CODING_GUIDE.md` 中，文件拆分规则被明确列出：

> **Keep files under ~300 lines** — split proactively.

VMark 的代码结构展示了一种**极端的模块化**：
- `src/stores/`：每个状态管理文件只有 100-300 行
- `src/hooks/`：每个自定义 Hook 只有 50-200 行
- `src/plugins/`：每个编辑器插件只有 100-400 行

这种拆分使得：
1. **AI 可以快速理解单个文件**：不需要处理数千行的代码
2. **人类可以快速审查 AI 的输出**：每次变更只影响小范围
3. **测试可以更精确**：每个模块都有独立的测试文件

在 `src/hooks/useEditor.ts` 中，你可以看到这种模块化的具体实现：

```typescript
export function useEditor() {
  const documents = useEditorStore((state) => state.documents);
  const activeDocId = useEditorStore((state) => state.activeDocId);
  const setActiveDocId = useEditorStore((state) => state.setActiveDocId);

  const activeDoc = useMemo(
    () => documents.find((doc) => doc.id === activeDocId) || null,
    [documents, activeDocId]
  );

  return {
    documents,
    activeDoc,
    setActiveDocId,
  };
}
```

这个 Hook 只有不到 50 行，职责清晰：提供编辑器状态的访问接口。如果需要添加新的状态访问逻辑，只需要修改这个文件。

### 2.2 Zustand Store：显式依赖的选择模式

在 `AGENTS.md` 中，有一条看似反直觉的规则：

> **Do not destructure Zustand stores in components; use selectors.**

```typescript
// ❌ 错误
const { documents, activeDocId } = useEditorStore();

// ✅ 正确
const documents = useEditorStore((state) => state.documents);
const activeDocId = useEditorStore((state) => state.activeDocId);
```

这背后隐藏着**AI 友好的设计哲学**：

1. **显式依赖**：通过 selector 显式声明依赖，AI 可以更容易理解数据流
2. **可测试性**：每个 selector 都可以独立测试，AI 可以生成更精确的测试
3. **性能优化**：Zustand 只在 selector 返回值变化时重新渲染

在 `src/stores/editorStore.ts` 中，你可以看到这种设计的具体实现：

```typescript
export const useEditorStore = create<EditorState>()(
  subscribeWithSelector((set, get) => ({
    documents: [],
    activeDocId: null,

    setDocuments: (documents) => set({ documents }),

    setActiveDocId: (id) => set({ activeDocId: id }),

    getActiveDoc: () => {
      const { documents, activeDocId } = get();
      return documents.find((doc) => doc.id === activeDocId) || null;
    },
  }))
);
```

每个状态和操作都有明确的类型定义，AI 可以根据这些类型生成正确的代码。

### 2.3 TDD 强制执行：测试覆盖率门禁

在 `vitest.config.ts` 中，配置了强制性的测试覆盖率阈值：

```typescript
coverage: {
  thresholds: {
    lines: 80,
    functions: 80,
    branches: 80,
    statements: 80,
  },
},
```

这不是"最佳实践"，而是**AI 开发的安全网**。当 AI 生成代码时，它可能会：
- 忘记处理边界情况
- 引入回归（修复一个 Bug，破坏另一个功能）
- 生成不可达的代码

测试覆盖率门禁确保：**如果 AI 的代码降低了覆盖率，构建就会失败**。

在 `AGENTS.md` 中，明确规定了 TDD 流程：

> **Test-first is mandatory** for new behavior:
> - Write a failing test (RED)
> - Implement minimally (GREEN)
> - Refactor (REFACTOR)

在 `src/hooks/__tests__/useEditor.test.ts` 中，你可以看到这种流程的具体实现：

```typescript
import { renderHook, act } from '@testing-library/react';
import { useEditor } from '../useEditor';

describe('useEditor', () => {
  it('should return empty documents initially', () => {
    const { result } = renderHook(() => useEditor());
    expect(result.current.documents).toEqual([]);
  });

  it('should set active document', () => {
    const { result } = renderHook(() => useEditor());
    act(() => {
      result.current.setActiveDocId('doc-1');
    });
    expect(result.current.activeDocId).toBe('doc-1');
  });
});
```

这些测试定义了期望的行为，AI 可以根据这些测试生成正确的实现。

### 2.4 MCP 集成：AI 原生的互操作协议

VMark 最具前瞻性的设计是**对 MCP（Model Context Protocol）的原生支持**。在 `vmark-mcp-server/` 目录中，实现了一个完整的 MCP 服务器。

在 `vmark-mcp-server/src/index.ts` 中，你可以看到 MCP 服务器的实现：

```typescript
import { Server } from '@modelcontextprotocol/sdk/server/index.js';
import { StdioServerTransport } from '@modelcontextprotocol/sdk/server/stdio.js';

const server = new Server({
  name: 'vmark-mcp-server',
  version: '0.1.0',
});

// 注册工具：读取文档
server.setRequestHandler(ListToolsRequestType, async () => ({
  tools: [
    {
      name: 'read_document',
      description: 'Read a VMark document by ID',
      inputSchema: {
        type: 'object',
        properties: {
          docId: {
            type: 'string',
            description: 'Document ID',
          },
        },
        required: ['docId'],
      },
    },
  ],
}));

// 启动服务器
const transport = new StdioServerTransport();
await server.connect(transport);
```

这个 MCP 服务器使得：
1. **用户可以直接在 VMark 中调用 AI**：不需要复制粘贴到 ChatGPT
2. **AI 可以直接操作 VMark**：可以读取文档、修改文本、执行命令
3. **AI 工具可以互操作**：Claude、Gemini、Codex 可以共享同一个 MCP 服务器

---

## 三、功能全景：编辑器的重新设计

从代码结构可以看出，VMark 不是一个简单的 Markdown 编辑器，而是一个**为 AI 时代重新设计的编辑器**。

### 3.1 三种编辑模式：从选择到流动

VMark 的核心创新是**三种编辑模式的无缝切换**：

| 模式 | 技术 | 适用场景 |
|------|------|----------|
| **WYSIWYG** | Tiptap/ProseMirror | 写作、格式化 |
| **Source Peek** | CodeMirror 6 + 浮层 | 快速查看源码 |
| **Source Mode** | CodeMirror 6 | 程序化编辑 |

在 `src/editor/WysiwygEditor.tsx` 中，你可以看到 WYSIWYG 模式的实现：

```typescript
import { useEditor, EditorContent } from '@tiptap/react';

export function WysiwygEditor() {
  const editor = useEditor({
    extensions: [
      StarterKit,
      Placeholder,
      // ... 其他扩展
    ],
    content: initialContent,
  });

  return <EditorContent editor={editor} />;
}
```

在 `src/editor/SourcePeek.tsx` 中，你可以看到 Source Peek 模式的实现：

```typescript
export function SourcePeek({ content }: { content: string }) {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <>
      <Button onClick={() => setIsOpen(!isOpen)}>
        {isOpen ? 'Hide Source' : 'Show Source'}
      </Button>
      {isOpen && (
        <div className="source-peek-overlay">
          <CodeMirror value={content} readOnly />
        </div>
      )}
    </>
  );
}
```

这种设计解决了**编辑器的"二元对立"问题**：传统编辑器要么是 WYSIWYG，要么是源码模式，用户被迫在两者之间选择。

### 3.2 多光标编辑：从线性到并行

在 `src/editor/MultiCursorEditor.tsx` 中，实现了多光标编辑功能：

```typescript
export function MultiCursorEditor() {
  const editor = useEditor({
    extensions: [
      Extension.create({
        name: 'multiCursor',
        addKeyboardShortcuts() {
          return {
            'Mod-d': () => {
              // 选择下一个匹配项
              const selection = editor.state.selection;
              const nextMatch = findNextMatch(selection);
              if (nextMatch) {
                editor.view.dispatch(
                  editor.state.tr.setSelection(nextMatch)
                );
              }
              return true;
            },
          };
        },
      }),
    ],
  });

  return <EditorContent editor={editor} />;
}
```

这种设计使得**用户可以同时编辑多个位置**，不需要重复复制粘贴。

### 3.3 CJK 排版：从西方到东方

在 `src/formatting/cjk.ts` 中，实现了 CJK 排版规则：

```typescript
export function formatCJK(text: string): string {
  // 中英文之间自动添加空格
  text = text.replace(/([\u4e00-\u9fa5])([a-zA-Z])/g, '$1 $2');
  text = text.replace(/([a-zA-Z])([\u4e00-\u9fa5])/g, '$1 $2');

  // 标点符号的自动转换
  text = text.replace(/,/g, '，');
  text = text.replace(/\./g, '。');

  // 行首行尾的标点避头尾法则
  text = text.replace(/^([，。！？])/, function(match) {
    return PROHIBITED_AT_LINE_START[match] || match;
  });

  return text;
}
```

这种设计使得**VMark 成为 CJK 用户的最佳选择**——不需要手动调整格式。

### 3.4 本地优先：从云端到设备

在 `README.md` 中，VMark 声称：

> **Local-First** — No cloud, no accounts, no analytics. Documents stay on your machine.

在 `src/storage/localStorage.ts` 中，你可以看到本地优先的实现：

```typescript
export function useLocalStorage<T>(key: string, initialValue: T) {
  const [storedValue, setStoredValue] = useState<T>(() => {
    if (typeof window === 'undefined') {
      return initialValue;
    }
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error(error);
      return initialValue;
    }
  });

  const setValue = (value: T) => {
    try {
      setStoredValue(value);
      if (typeof window !== 'undefined') {
        window.localStorage.setItem(key, JSON.stringify(value));
      }
    } catch (error) {
      console.error(error);
    }
  };

  return [storedValue, setValue] as const;
}
```

这种设计使得**VMark 成为用户真正拥有的工具**——不是"租用"服务，而是"拥有"软件。

---

## 四、开发范式：AI 与人类的新分工

VMark 的代码展示了一个**可能的未来**：在这个未来中，软件开发不是人类独自完成，而是 AI 与人类协作的结果。

### 4.1 人类的角色：从编写者到监督者

在 `CONTRIBUTING.md` 中，VMark 明确指出：

> VMark is **vibe-coded** — written entirely by AI under human supervision. We welcome **issues** (bug reports, feature requests) but cannot safely merge external PRs.

这种"只接受 Issue，不接受 PR"的政策，隐藏着对**人类角色重新定义**：

| 传统模式 | VMark 模式 |
|----------|-----------|
| 人类编写代码 | AI 编写代码 |
| 人工测试 | 自动化测试 |
| 人工审查 | Issue 驱动 |
| 人工部署 | CI/CD |

人类不再是"代码编写者"，而是：
- **产品定义者**：通过 Issue 描述需求
- **质量监督者**：通过测试定义期望
- **架构设计者**：通过规则定义约束

### 4.2 AI 的角色：从辅助到主力

在 `AGENTS.md` 中，有详细的 AI 工作指南：

- **Research before building**：在实现新功能前，搜索行业最佳实践
- **Edge cases are not optional**：必须考虑所有边界情况
- **Test-first is mandatory**：必须先写测试，再写实现

这些指南使得**AI 的输出是可预测的、高质量的、符合规范的**。

### 4.3 质量保证：从人工到自动化

VMark 的质量保证体系是**完全自动化的**：

```bash
pnpm check:all  # 运行所有质量门禁
```

这个命令会依次执行：
1. **ESLint**：代码风格检查
2. **Vitest**：单元测试 + 覆盖率
3. **TSC**：类型检查
4. **Vite Build**：构建验证

在 `package.json` 中，你可以看到这些脚本的定义：

```json
{
  "scripts": {
    "check:all": "pnpm lint && pnpm test && pnpm typecheck && pnpm build",
    "lint": "eslint .",
    "test": "vitest run --coverage",
    "typecheck": "tsc --noEmit",
    "build": "vite build"
  }
}
```

这种**自动化质量门禁**确保代码库始终保持高质量。

---

## 五、价值总结：AI 原生的范式转移

VMark 的代码库不是在构建一个编辑器，而是在**展示 AI 原生开发的未来**。

### 5.1 从"代码"到"配置"

传统开发中，代码是核心资产。但在 VMark 中，**配置和规则比代码更重要**：

- `AGENTS.md`：AI 的工作指南
- `CODING_GUIDE.md`：编码规范
- `.claude/`：Claude Code 的配置
- `.mcp.json`：MCP 服务器配置
- `vitest.config.ts`：测试配置

这些配置文件定义了：
- AI 应该如何工作
- 代码应该如何组织
- 测试应该如何编写
- 构建应该如何执行

**代码变成了配置的产物**——只要改变配置，AI 就会生成不同的代码。

### 5.2 从"开发"到"提示工程"

传统开发中，技能是"编写代码"。在 VMark 模式中，技能是**编写好的 Issue 和配置**：

- **好的 Issue**：清晰地描述需求、边界情况、期望行为
- **好的配置**：定义清晰的质量标准、测试阈值、风格规则
- **好的文档**：提供足够的上下文，让 AI 理解项目结构

这种转变使得**非程序员也可以参与开发**——你不需要会写代码，只需要会写需求。

### 5.3 未解的挑战

但 VMark 也面临一些根本性的限制：

**限制 1：AI 的上下文窗口限制**
虽然 VMark 通过文件拆分缓解了 AI 上下文窗口的问题，但**大型项目的全局理解仍然是挑战**。AI 可能理解单个文件，但很难理解整个项目的架构和设计决策。

**限制 2：质量保证的盲点**
虽然 VMark 有严格的测试覆盖率门禁，但**测试无法覆盖所有问题**：

- **性能问题**：测试可以验证功能正确性，但无法验证性能
- **用户体验**：测试可以验证 UI 行为，但无法验证用户感受
- **安全漏洞**：测试可以验证常见漏洞，但无法验证所有攻击向量

**限制 3：社区参与的两难**
VMark 的"只接受 Issue，不接受 PR"政策，虽然避免了 AI 生成代码的安全问题，但也**限制了社区的参与度**：

- 开发者无法通过 PR 贡献代码
- 社区无法快速修复 Bug
- 项目的发展完全依赖于创始人的决策

---

## 后记：AI 原生的未来

VMark 的代码展示了一个**可能的未来**：在这个未来中，软件开发不是人类的独角戏，而是 AI 与人类的协奏曲。

这个未来中：
- **人类不再写代码**，而是写需求、写配置、写测试
- **AI 不再是辅助工具**，而是主力开发者
- **代码不再是资产**，而是需求和配置的产物
- **质量不再是人工保证**，而是自动化门禁强制执行

但这只是一个开始。VMark 的模式和设计可以被复制和改进。真正的竞争不是编辑器功能，而是**AI 原生开发的范式**——谁能更有效地让 AI 与人类协作，谁就能在 AI 时代胜出。

就像 VMark 的代码所展示的，**AI 原生不是一次性的选择，而是一个持续演进的过程**。VMark 提供了一个范本，但最终，AI 原生的未来取决于整个社区如何探索和实践。

---

*（完）*
