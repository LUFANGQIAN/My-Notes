# TermRepo VS Code 插件 - 项目开发文档

------

## 📋 项目信息

| 项目             | 信息                                |
| ---------------- | ----------------------------------- |
| **项目名称**     | TermRepo VS Code Plugin             |
| **仓库**         | github.com/termrepo/termrepo-vscode |
| **版本**         | v0.1.0 (MVP)                        |
| **许可证**       | MIT                                 |
| **开发语言**     | TypeScript                          |
| **VS Code 要求** | ^1.85.0                             |
| **启动日期**     | 2026-03-24                          |
| **MVP 目标日期** | 2026-05-01                          |

------

## 🎯 项目目标

### 核心目标

```
让开发者在编码过程中一键收藏专业术语，建立个人技术词库
```

### MVP 功能范围

| 功能                 | 状态 |
| -------------------- | ---- |
| 选中单词一键收藏     | ✅ P0 |
| 本地 SQLite 存储     | ✅ P0 |
| 侧边栏查看管理       | ✅ P0 |
| 基础云端同步接口预留 | ✅ P0 |
| 术语搜索             | ✅ P0 |
| 单词导入导出         | ✅ P0 |

------

## 🛠️ 技术栈

### 核心依赖

```json
{
  "runtime": "Node.js 18+",
  "language": "TypeScript 5.0+",
  "bundler": "esbuild",
  "database": "better-sqlite3",
  "http": "axios",
  "testing": "Jest"
}
```

> 待定



### 完整依赖清单

```bash
# 生产依赖
npm install better-sqlite3 axios uuid dayjs

# 开发依赖
npm install -D typescript @types/vscode @types/node esbuild eslint prettier jest @vscode/test-electron @vscode/vsce
```

> 待定



## 📁 项目结构

```
termrepo-vscode/
├── .vscode/                    # VS Code 配置
│   ├── launch.json
│   └── settings.json
├── src/
│   ├── extension.ts            # 入口文件
│   ├── commands/               # 命令处理
│   │   ├── collectWord.ts
│   │   ├── sync.ts
│   │   └── index.ts
│   ├── providers/              # 视图提供者
│   │   ├── termsTreeView.ts
│   │   └── index.ts
│   ├── storage/                # 数据存储
│   │   ├── database.ts
│   │   ├── repository.ts
│   │   └── index.ts
│   ├── sync/                   # 同步服务
│   │   ├── client.ts
│   │   ├── queue.ts
│   │   └── index.ts
│   ├── models/                 # 数据模型
│   │   └── term.ts
│   └── utils/                  # 工具函数
│       ├── logger.ts
│       └── notifier.ts
├── resources/                  # 资源文件
│   └── icon.svg
├── tests/                      # 测试文件
├── .eslintrc.json
├── .prettierrc
├── .gitignore
├── tsconfig.json
├── package.json
├── LICENSE
└── README.md
```

------





## 🚀 快速开始

### 1. 环境准备

```bash
# 检查 Node.js 版本 (需要 18+)
node -v

# 安装依赖
npm install

# 验证安装
npm run build
```

### 2. 开发模式

```bash
# 启动监听构建
npm run watch

# 在 VS Code 中按 F5 启动调试
```

### 3. 打包发布

```bash
# 打包为 .vsix
npm run package

# 发布到市场 (需要发布者权限)
vsce publish
```

------





## 📦 核心功能实现

### 1. 收藏命令 (collectWord.ts)

```typescript
import * as vscode from 'vscode';
import { TermRepository } from '../storage';

export async function collectWord() {
  const editor = vscode.window.activeTextEditor;
  if (!editor) return;
  
  const selection = editor.selection;
  const word = editor.document.getText(selection);
  
  if (!word) {
    vscode.window.showWarningMessage('请先选中要收藏的单词');
    return;
  }
  
  const term = {
    word,
    context: {
      filePath: editor.document.fileName,
      lineNumber: selection.start.line + 1,
      codeSnippet: editor.document.lineAt(selection.start).text,
      language: editor.document.languageId
    },
    createdAt: Date.now()
  };
  
  await TermRepository.create(term);
  vscode.window.showInformationMessage(`已收藏: ${word}`);
}
```

### 2. 数据模型 (term.ts)

```typescript
export interface Term {
  id: string;
  word: string;
  translation?: string;
  context: {
    filePath: string;
    lineNumber: number;
    codeSnippet: string;
    language: string;
  };
  tags: string[];
  mastered: boolean;
  createdAt: number;
  updatedAt: number;
  syncedAt?: number;
}
```

### 3. 数据库 Schema (database.ts)

```sql
CREATE TABLE IF NOT EXISTS terms (
  id TEXT PRIMARY KEY,
  word TEXT NOT NULL,
  translation TEXT,
  filePath TEXT,
  lineNumber INTEGER,
  codeSnippet TEXT,
  language TEXT,
  tags TEXT,
  mastered INTEGER DEFAULT 0,
  createdAt INTEGER,
  updatedAt INTEGER,
  syncedAt INTEGER
);

CREATE INDEX IF NOT EXISTS idx_word ON terms(word);
CREATE INDEX IF NOT EXISTS idx_created ON terms(createdAt);
```

### 4. 侧边栏视图 (termsTreeView.ts)

```typescript
export class TermsTreeView implements vscode.TreeDataProvider<TermItem> {
  private _onDidChangeTreeData = new vscode.EventEmitter<TermItem | undefined>();
  readonly onDidChangeTreeData = this._onDidChangeTreeData.event;
  
  async getChildren(element?: TermItem): Promise<TermItem[]> {
    const terms = await TermRepository.findAll();
    return terms.map(term => new TermItem(term));
  }
  
  getTreeItem(element: TermItem): vscode.TreeItem {
    return element;
  }
  
  refresh(): void {
    this._onDidChangeTreeData.fire(undefined);
  }
}
```

------



## 📅 开发计划

### 阶段一：MVP (4 周)

| 周次 | 日期          | 任务                | 交付物      |
| ---- | ------------- | ------------------- | ----------- |
| W1   | 03-24 ~ 03-30 | 项目搭建 + 收藏功能 | 可收藏单词  |
| W2   | 03-31 ~ 04-06 | 本地存储 + 侧边栏   | 可查看词库  |
| W3   | 04-07 ~ 04-13 | 同步功能            | 可云端同步  |
| W4   | 04-14 ~ 04-20 | 搜索 + 测试 + 发布  | v0.1.0 发布 |

### 关键里程碑

```
□ M1: 03-24 项目启动
□ M2: 03-31 收藏功能完成
□ M3: 04-07 侧边栏完成
□ M4: 04-14 同步功能完成
□ M5: 04-21 MVP 发布
```

------

## 🧪 测试要求

### 单元测试

```typescript
// tests/unit/storage.test.ts
describe('TermRepository', () => {
  it('should create a term', async () => {
    const term = await TermRepository.create({...});
    expect(term.id).toBeDefined();
  });
  
  it('should find term by word', async () => {
    const terms = await TermRepository.findByWord('test');
    expect(terms.length).toBeGreaterThan(0);
  });
});
```

### 测试覆盖率目标

| 模块      | 目标覆盖率 |
| --------- | ---------- |
| storage/  | > 80%      |
| sync/     | > 70%      |
| commands/ | > 60%      |

------

## 📝 开发规范

### 代码规范

```typescript
// 必须使用 TypeScript 严格模式
// 遵循 ESLint + Prettier 配置
// 函数必须有类型注解
// 禁止使用 any 类型
```

### 提交信息规范

```bash
# 格式
<type>(<scope>): <subject>

# 示例
feat(commands): 添加收藏命令
fix(storage): 修复数据库连接问题
docs(readme): 更新开发指南
```

### Type 类型

| 类型     | 说明      |
| -------- | --------- |
| feat     | 新功能    |
| fix      | 修复 Bug  |
| docs     | 文档更新  |
| style    | 代码格式  |
| refactor | 重构      |
| test     | 测试相关  |
| chore    | 构建/配置 |

------

## ⚠️ 注意事项

### 平台兼容性

| 平台    | 测试要求   |
| ------- | ---------- |
| Windows | ✅ 必须测试 |
| macOS   | ✅ 必须测试 |
| Linux   | ✅ 必须测试 |

### SQLite 注意事项

```
⚠️ better-sqlite3 需要原生编译
⚠️ 确保各平台都能正确构建
⚠️ 准备降级方案 (纯 JSON 存储)
```

### 发布前检查清单

```
□ 所有 P0 功能完成
□ 单元测试通过率 > 80%
□ 跨平台测试完成
□ README 文档完善
□ LICENSE 文件正确
□ package.json 元数据完整
```

------

## 📞 联系方式

| 角色          | 联系方式                                          |
| ------------- | ------------------------------------------------- |
| 项目负责人    | [pm@termrepo.io](mailto:pm@termrepo.io)           |
| 技术支持      | [support@termrepo.io](mailto:support@termrepo.io) |
| GitHub Issues | github.com/termrepo/termrepo-vscode/issues        |

------

## 🔄 文档更新记录

| 版本 | 日期       | 更新内容 | 作者       |
| ---- | ---------- | -------- | ---------- |
| v0.1 | 2026-03-24 | 初始版本 | LUFANGQIAN |

------

**项目正式启动！祝开发顺利！🚀**





## 项目拆分重构报告  
**项目名称**：`termrepoplugin-vscode`  
**目标**：提升代码可维护性、可读性，为后续功能扩展和社区贡献奠定清晰基础  

---

### 一、当前代码存在的问题（心路历程起点）

在 `extension.ts` 中，我们集中实现了所有功能：  
- 激活扩展时的 CPU 信息展示  
- 多个命令（`helloworld`、`showCPUInfo`、`showCPUNum`、`printSelection`）  
- 文件存储操作（`ensureStorageDir`、`loadWords`、`saveWords`、`addWord`）  
- 与编辑器交互的选中文本处理  

这种“大杂烩”式的写法在原型阶段很方便，但随着功能增多，会带来以下痛点：  
1. **职责混杂**：一个文件既管存储，又管命令，还管视图逻辑，修改某一功能时容易牵一发而动全身。  
2. **可读性差**：新增功能时，文件会越来越长，新人难以快速定位到特定模块。  
3. **难以测试**：所有逻辑都耦合在同一个函数中，无法单独对存储模块或拆分算法进行单元测试。  
4. **协作困难**：多人同时修改同一文件容易产生冲突，且分工不明确。  

因此，**必须对项目进行模块化拆分**，这是保证项目长期健康发展的关键一步。

---

### 二、拆分原则

我们将遵循以下工程化原则：  
- **单一职责**：每个模块只负责一件事。  
- **依赖注入**：模块间通过接口或参数传递依赖，避免硬编码。  
- **高内聚低耦合**：相关功能放在同一模块，模块间通过清晰接口交互。  
- **可扩展性**：为后续增加新命令、新视图、新存储方式预留空间。  

---

### 三、建议的项目结构

```
termrepoplugin-vscode/
├── .vscode/                     # 调试配置
├── src/
│   ├── extension.ts             # 入口文件：仅负责激活、注册命令和视图
│   ├── commands/                # 所有命令实现
│   │   ├── index.ts             # 统一注册命令
│   │   ├── helloWorld.ts
│   │   ├── showCPUInfo.ts
│   │   ├── showCPUNum.ts
│   │   └── printSelection.ts
│   ├── storage/                 # 数据持久化
│   │   ├── storageManager.ts    # 封装文件读写（或 Memento）
│   │   ├── types.ts             # 定义单词数据结构（将来扩展）
│   │   └── words.json           # 实际数据文件（运行时生成）
│   ├── utils/                   # 工具函数
│   │   ├── fileHelper.ts        # 文件操作辅助（目前可能不需要单独，但可预留）
│   │   └── logger.ts            # 日志封装（统一控制台输出）
│   ├── models/                  # 数据模型（如果未来数据结构复杂）
│   │   └── word.ts              # Word 类的定义或接口
│   └── types/                   # 全局类型定义（可选）
└── ... 其他配置文件
```

**说明**：  
- 目前功能较简单，可以先从 `commands` 和 `storage` 两个核心模块开始拆分。  
- `utils` 用于存放跨模块使用的通用函数（如字符串处理、日志）。  
- `models` 用于定义数据结构（当单词对象包含备注、标签等属性时启用）。  

---

### 四、各模块职责与拆分详解

#### 1. `extension.ts` – 入口组装器
**职责**：  
- 调用 `ensureStorageDir` 初始化存储目录。  
- 实例化 `StorageManager` 并传入 `globalStorageUri`。  
- 调用 `registerCommands` 注册所有命令，并将依赖（如 `StorageManager`）注入其中。  
- 将命令的 `Disposable` 对象添加到 `context.subscriptions`。  

**为什么这样拆分**：  
入口文件应该只做“组装”工作，而不是具体实现。这样，当我们需要增加新命令或更换存储方式时，只需修改 `extension.ts` 中对应的几行代码，而不影响其他模块。

#### 2. `commands/` 目录 – 每个命令独立成文件
**职责**：  
- 每个文件导出一个工厂函数，接收所需依赖（如 `storageManager`），返回 `vscode.Disposable`。  
- 命令的具体逻辑（获取选中文本、调用存储、显示消息）放在对应的文件中。  

**示例：`commands/printSelection.ts`**  
```typescript
import * as vscode from 'vscode';
import { StorageManager } from '../storage/storageManager';

export function printSelectionCommand(storage: StorageManager) {
    return vscode.commands.registerCommand('termrepoplugin-vscode.printSelection', async () => {
        const editor = vscode.window.activeTextEditor;
        if (!editor) {
            vscode.window.showWarningMessage('没有活动的编辑器');
            return;
        }
        const selection = editor.selection;
        if (selection.isEmpty) {
            vscode.window.showWarningMessage('没有选中任何文本');
            return;
        }
        const selectedText = editor.document.getText(selection);
        console.log('选中的内容:', selectedText);
        vscode.window.showInformationMessage(`选中内容: ${selectedText}`);
        await storage.addWord(selectedText);
    });
}
```

**为什么这样拆分**：  
- 每个命令独立，便于单独修改和测试。  
- 新增命令时只需添加新文件，并在 `commands/index.ts` 中引入即可，无需修改已有命令代码。  
- 依赖通过参数显式传递，避免全局状态。

#### 3. `storage/` 目录 – 封装数据持久化
**职责**：  
- 管理单词数据的读取、写入、增删查改。  
- 提供 `StorageManager` 类，内部封装文件操作。  
- 对外暴露简洁的方法：`addWord`、`getAllWords`、`deleteWord` 等。  

**示例：`storage/storageManager.ts`**  
```typescript
import * as fs from 'fs/promises';
import * as path from 'path';
import * as vscode from 'vscode';

export class StorageManager {
    private filePath: string;
    private words: string[] = [];

    constructor(storageUri: vscode.Uri) {
        this.filePath = path.join(storageUri.fsPath, 'words.json');
    }

    async initialize() {
        try {
            await fs.mkdir(path.dirname(this.filePath), { recursive: true });
            const data = await fs.readFile(this.filePath, 'utf-8');
            this.words = JSON.parse(data);
        } catch (err) {
            this.words = [];
        }
    }

    async addWord(word: string): Promise<void> {
        if (!this.words.includes(word)) {
            this.words.push(word);
            await this.save();
            vscode.window.showInformationMessage(`已添加单词: ${word}`);
        } else {
            vscode.window.showInformationMessage(`单词 "${word}" 已存在`);
        }
    }

    async getAllWords(): Promise<string[]> {
        return [...this.words];
    }

    private async save(): Promise<void> {
        await fs.writeFile(this.filePath, JSON.stringify(this.words, null, 2), 'utf-8');
    }
}
```

**为什么这样拆分**：  
- 将存储逻辑与命令逻辑分离，未来如果改用 `vscode.Memento` 或数据库，只需修改 `StorageManager`，命令代码无需改动。  
- 集中管理文件路径和错误处理，避免在多个命令中重复编写 `readFile`/`writeFile` 逻辑。  
- 便于单元测试，可以模拟文件系统或注入内存存储。

#### 4. `utils/` 目录 – 通用工具函数
**职责**：  
- 存放与业务无关的辅助函数，如日志格式化、字符串处理、路径拼接等。  
- 目前可先预留，待后续需要拆分算法时再添加 `splitter.ts`。

**为什么这样拆分**：  
- 减少重复代码，提高复用性。  
- 使核心模块更聚焦业务逻辑。

#### 5. `models/` 目录 – 数据模型（预留）
**职责**：  
- 定义单词对象的数据结构（当单词需要存储备注、标签、拆分结果时）。  
- 提供类型定义，便于 TypeScript 类型检查。

**为什么这样拆分**：  
- 随着功能增强，单词对象会变得复杂（包含含义、标签、拆分词等），将数据结构单独定义可保持代码清晰。  
- 方便未来进行数据迁移或版本升级。

---

### 五、拆分后的代码示例（关键部分）

#### 修改后的 `extension.ts`
```typescript
import * as vscode from 'vscode';
import { StorageManager } from './storage/storageManager';
import { registerCommands } from './commands';

export async function activate(context: vscode.ExtensionContext) {
    console.log('扩展已激活');

    // 初始化存储管理器
    const storage = new StorageManager(context.globalStorageUri);
    await storage.initialize();

    // 注册所有命令
    registerCommands(context, storage);

    // 可选的额外初始化（如 CPU 信息展示，但建议也放入命令）
    console.log('扩展初始化完成');
}

export function deactivate() {
    console.log('扩展被停用');
}
```

#### `commands/index.ts`
```typescript
import * as vscode from 'vscode';
import { StorageManager } from '../storage/storageManager';
import { helloWorldCommand } from './helloWorld';
import { showCPUInfoCommand } from './showCPUInfo';
import { showCPUNumCommand } from './showCPUNum';
import { printSelectionCommand } from './printSelection';

export function registerCommands(context: vscode.ExtensionContext, storage: StorageManager) {
    context.subscriptions.push(
        helloWorldCommand(),
        showCPUInfoCommand(),
        showCPUNumCommand(),
        printSelectionCommand(storage)
    );
}
```

#### `commands/helloWorld.ts`
```typescript
import * as vscode from 'vscode';
import * as os from 'os';

export function helloWorldCommand() {
    return vscode.commands.registerCommand('termrepoplugin-vscode.helloworld', () => {
        const cpus = os.cpus();
        console.log('命令 termrepoplugin-vscode.helloWorld 已执行');
        vscode.window.showInformationMessage('Hello World from termrepoplugin-vscode!');
        vscode.window.showInformationMessage('没错，第一个插件跑起来了');
        vscode.window.showInformationMessage(`CPU 型号: ${cpus[0]?.model || '未知'}`);
    });
}
```

---

### 六、拆分后的优势

1. **清晰的项目结构**：任何人都能快速理解每个目录的用途。  
2. **易于维护**：修改某个命令时只需打开对应的文件，不影响其他模块。  
3. **可测试性**：`StorageManager` 可以单独测试（如模拟文件系统），命令也可以模拟依赖进行测试。  
4. **团队协作友好**：不同开发者可以同时开发不同的命令或存储逻辑，减少冲突。  
5. **便于记录开发心路历程**：每个模块的演进可以在对应的文件中通过注释或提交历史清晰记录，为后续贡献者提供指引。  

---

### 七、后续开发建议

1. **逐步迁移**：先拆分存储和命令，再根据功能需求逐步添加新模块（如 `utils/splitter.ts`、`views/`）。  
2. **编写文档**：在 `README.md` 中说明项目结构和开发指南，方便新贡献者快速上手。  
3. **引入单元测试**：使用 `@vscode/test-electron` 和 `mocha` 为 `StorageManager` 和工具函数编写测试。  
4. **版本管理**：利用 Git 进行原子化提交，每个模块的添加或修改对应一次清晰的 commit，便于回溯。  

---

### 八、结语

这次重构不是简单的代码搬家，而是为项目未来的成长铺设稳固的基础。通过模块化拆分，我们不仅解决了当前代码的混乱问题，也为后续实现单词拆分、标签系统、搜索功能等复杂特性扫清了障碍。希望这份报告能帮助你清晰记录开发思路，也为将来想要贡献代码的朋友提供一份友好的指引。

**现在，就从拆分 `extension.ts` 开始吧！** 🚀 



# 日志

以下记录我每天都干了啥

监督下我的进度

## TODO List


- [x] 选中单词右键菜单选项触发，打印 

- [x] 为触发选项添加快捷键，利用快捷键打印

- [x] 被选中的单词储存到本地固定位置

- [ ] 重构 模块化

- [ ] 一键打印所有单词 可以通过弹窗形式

- [ ] 在面板=》底部视图区（放终端那个）开辟集词库 TRP(TermRepoPlugin) 区域 展示本地储存的信息

- [ ] 考虑数据结构

- [ ] 单纯的实现单词的 增删查改

- [ ] 设计单词编辑界面实现以下功能：

    - [ ] 对选中的单词进行拆分 拆分算法可以考虑正则表达式等

    - [ ] 在面版区域展示拆分 并且对拆分的单词添加备注栏

    - [ ] 对组合词添加 自定义翻译 使用场景备注 自定义tag

    - [ ] 拆分单词的中英文自动计入tag

    - [ ] 开始实现数据结构 进行储存

- [ ] 考虑搜索功能 通过模糊词语 关键词 支持中英文搜索

- [ ] 对单条术语记录添加 复制 编辑 按钮

- [ ] 将单条术语记录的删除按钮 增加至编辑界面子界面中

- [ ] 实现词库中记录过的基础单词 在遇见新的组合词被拆分出来时 自动匹配含义 减少重复备注单词




## 2026-3-24

明确了项目目标 将idea转化为可执行的文档 简单介绍下项目 项目名称是TermRepo集词库 理解为术语库 

创建这个项目的初衷是为了解决两个问题

首先是背英语单词的时候 背的总是脱离场景的 用不上的单词没有意义 

其次是开发新项目的时候 可能原来写过的同样功能的方法 变量 类 选择器 装饰器等等 其名字记不住
非常难受的就是给这对玩应起名字 

所以我有了一个想法 把遇到的单词 不同单词组合出来的变量名都记录下来
同时添加好备注 标签 场景 遇到组合词根据大驼峰小驼峰等命名规则进行拆分 然后对单个单词进行备注含义

主要分为三部分 或者说是三个端

插件采集端 网页服务端 移动复习端

先说说插件采集端：目前打算为vscode进行插件的开发 作为插件的采集 后续考虑JB PC等IDE插件 以及浏览器插件

其次是网页服务端：前端管理页面与后端服务程序 带上数据库 同时支持购买云备份云同步服务

最后是移动复习端：设置好端口自动同步 每天进行单词复习通知 制作类似github代码提交墙 鼓励复习单词



## 2026-3-25

我认为应当逐步来

初阶需要实现的功能有

选中单词右键菜单选项触发，打印 

为触发选项添加快捷键，利用快捷键打印

被选中的单词储存到本地固定位置

一键打印所有单词 可以通过弹窗形式

在面板=》底部视图区（放终端那个）开辟集词库 TRP(TermRepoPlugin) 区域 展示本地储存的信息

考虑数据结构

单纯的实现单词的 增删查改

设计单词编辑界面实现以下功能：

​	对选中的单词进行拆分 拆分算法可以考虑正则表达式等

​	在面版区域展示拆分 并且对拆分的单词添加备注栏

​	对组合词添加 自定义翻译 使用场景备注 自定义tag

​	拆分单词的中英文自动计入tag

​	开始实现数据结构 进行储存

考虑搜索功能 通过模糊词语 关键词 支持中英文搜索

对单条术语记录添加 复制 编辑 按钮

将单条术语记录的删除按钮 增加至编辑界面子界面中

实现词库中记录过的基础单词 在遇见新的组合词被拆分出来时 自动匹配含义 减少重复备注单词



呃呃呃 今天整理了下核心功能的开发 但是问题很大的是 所有功能都写在extension中 真的是太恶心了 好吧 确实是屎山了
突然理解了为什么中大型项目肯定是模块化的工程化的 小项目几个小功能就已经恶心到了 要先进行重构了 真是一点一点学习

































