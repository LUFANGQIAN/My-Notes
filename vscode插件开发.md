# VS Code插件开发

首先官方文档跑不了 跟着官方文档跑

https://vscode.js.cn/api/get-started/your-first-extension





## 创建项目

```txt
# ? What type of extension do you want to create? New Extension (TypeScript)
# ? What's the name of your extension? HelloWorld
### Press <Enter> to choose default for all options below ###

# ? What's the identifier of your extension? helloworld
# ? What's the description of your extension? LEAVE BLANK
# ? Initialize a git repository? Y
# ? Which bundler to use? unbundled
# ? Which package manager to use? npm

# ? Do you want to open the new folder with Visual Studio Code? Open with `code`


```

这是执行 `yo code`（VS Code 扩展生成器）时出现的交互提示。`yo code` 会引导你创建一个新的 VS Code 扩展项目。下面逐行解释每个提示和你的选择：

---

**1. `# ? What type of extension do you want to create? New Extension (TypeScript)`**

**含义**：询问你要创建哪种类型的扩展。  
**选项**：包括 `New Extension (TypeScript)`、`New Extension (JavaScript)`、`New Theme`、`New Language Support` 等。  
**你的选择**：`New Extension (TypeScript)`，表示创建一个使用 TypeScript 编写的普通功能扩展。

---

**2. `# ? What's the name of your extension? HelloWorld`**

**含义**：询问扩展的名称。  
**说明**：这个名称会作为扩展的显示名称（`displayName`）出现在扩展市场，也会用于生成项目文件夹名。  
**你的输入**：`HelloWorld`。

---

**3. `### Press <Enter> to choose default for all options below ###`**

**含义**：提示你可以直接按回车，后面的所有选项都将采用默认值，无需逐一选择。  
**作用**：这是一个便利提示，如果你不想手动配置每个细节，可以一键完成。

---

**4. `# ? What's the identifier of your extension? helloworld`**

**含义**：询问扩展的唯一标识符（identifier）。  
**说明**：这是扩展在代码中的内部名称，必须小写，通常用连字符分隔。  
**你的选择**：直接回车接受默认值 `helloworld`（由名称 `HelloWorld` 自动转换而来）。

---

**5. `# ? What's the description of your extension? LEAVE BLANK`**

**含义**：询问扩展的描述信息。  
**说明**：会显示在扩展市场的详情中。  
**你的选择**：留空（直接回车）。

---

**6. `# ? Initialize a git repository? Y`**

**含义**：询问是否在项目目录中初始化一个 Git 仓库。  
**说明**：如果选 `Y`，生成器会自动执行 `git init`。  
**你的选择**：`Y`，表示要初始化 Git。

---

**7. `# ? Which bundler to use? unbundled`**

**含义**：选择打包工具（bundler）。  
**选项**：常见的有 `webpack`、`esbuild`、`unbundled`（不打包）。  
**你的选择**：`unbundled`，表示不进行代码打包，直接使用 TypeScript 编译后的结构，适合简单项目。

---

**8. `# ? Which package manager to use? npm`**

**含义**：选择包管理器。  
**选项**：`npm`、`yarn`、`pnpm` 等。  
**你的选择**：`npm`，后续会使用 `npm install` 安装依赖。

---

**9. `# ? Do you want to open the new folder with Visual Studio Code? Open with 'code'`**

**含义**：询问是否自动用 VS Code 打开刚刚创建的项目文件夹。  
**说明**：如果你选择 `Open with 'code'`，生成器会执行 `code ./HelloWorld` 命令，在新窗口中打开项目。  
**你的选择**：`Open with 'code'`，表示立即用 VS Code 打开项目。

---

**总结**：这些交互步骤完成后，会在当前目录下生成一个名为 `HelloWorld` 的文件夹，其中包含完整的 VS Code 扩展项目模板，并自动用 VS Code 打开，你就可以开始开发了。





## 注册组件

在package.json中将commands数组中单个指令的title更改可以修改执行时的指令 command影响的是在extension.ts文件中注册组件时的组件名，二者名字要相符

```ts
//extension.ts	

const disposable = vscode.commands.registerCommand('helloworld.test', () => {
		// The code you place here will be executed every time your command is executed
		// 你在此处编写的代码将在每次执行命令时运行
		// Display a message box to the user
		// 向用户显示一个消息框

		let massage: string = '这是变量输入的消息';
		vscode.window.showInformationMessage('Hello World from helloworld!');
		vscode.window.showInformationMessage('没错 第一个插件跑起来了');
		vscode.window.showInformationMessage(massage);
	});
```

```ts
//package.json

    "commands": [
      {
        "command": "helloworld.test",
        "title": "Hello World test"
      }
    ]

```



## 自动构建

### 没有保存文件或没有重新编译

- VS Code 扩展在调试前需要先将 TypeScript 编译为 JavaScript（输出到 `out` 或 `dist` 目录）。
- 如果你修改了 `extension.ts` 但没有保存，或没有运行编译任务，调试运行的仍是上一次编译的旧代码。

**解决方法**：

- 按 `Ctrl+S` 保存文件。
- 运行编译：按 `Ctrl+Shift+B`，选择 `tsc: watch` 或 `npm: compile`，确保编译成功。
- 或直接在终端执行 `npm run compile`（具体命令看 `package.json` 中的 `scripts`）。



## package.json配置文件详解

```json
{
  "name": "helloworld",
  "displayName": "helloworld",
  "description": "LEAVE BLANK",
  "version": "0.0.1",
  "engines": {
    "vscode": "^1.110.0"
  },
  "categories": [
    "Other"
  ],
  "activationEvents": [],
  "main": "./out/extension.js",
  "contributes": {
    "commands": [
      {
        "command": "helloworld.helloworld",
        "title": "Hello World test"
      },
      {
        "command": "helloworld.showCPUInfo",
        "title": "显示 CPU 信息",
        "category": "HelloWorld"
      }
    ],
    "menus": {
      "editor/context": [
        {
          "command": "helloworld.showCPUInfo",
          "group": "navigation",
          "when": "editorTextFocus"
        }
      ]
    }
  },
  "scripts": {
    "vscode:prepublish": "npm run compile",
    "compile": "tsc -p ./",
    "watch": "tsc -watch -p ./",
    "pretest": "npm run compile && npm run lint",
    "lint": "eslint src",
    "test": "vscode-test"
  },
  "devDependencies": {
    "@types/vscode": "^1.110.0",
    "@types/mocha": "^10.0.10",
    "@types/node": "22.x",
    "typescript-eslint": "^8.56.1",
    "eslint": "^9.39.3",
    "typescript": "^5.9.3",
    "@vscode/test-cli": "^0.0.12",
    "@vscode/test-electron": "^2.5.2"
  }
}
```

这是 VS Code 扩展的核心清单文件，定义了扩展的基本信息、功能、依赖和构建脚本。

---

### 顶层字段

```json
{
  "name": "helloworld",
```
- **`name`**：扩展的唯一标识符（内部名称），会用于生成文件夹名和发布时作为包名。必须小写，不包含空格。

```json
  "displayName": "helloworld",
```
- **`displayName`**：在 VS Code 扩展市场中显示的名称，用户看到的扩展名。可以包含空格和大小写。

```json
  "description": "LEAVE BLANK",
```
- **`description`**：扩展的简短描述，会显示在扩展详情中。这里留空（实际可以写更友好的说明）。

```json
  "version": "0.0.1",
```
- **`version`**：扩展的版本号，遵循语义化版本规范（SemVer）。每次发布更新时需要递增。

```json
  "engines": {
    "vscode": "^1.110.0"
  },
```
- **`engines`**：指定扩展运行所需的 VS Code 最低版本。`^1.110.0` 表示兼容 1.110.0 及以上版本（包括 1.110.x 和更高版本）。

```json
  "categories": [
    "Other"
  ],
```
- **`categories`**：扩展的分类，用于在市场中分类显示。常用值有 `Programming Languages`、`Snippets`、`Themes`、`Other` 等。这里选择 `Other` 表示“其他”类别。

```json
  "activationEvents": [],
```
- **`activationEvents`**：定义扩展在何时被激活（加载）。空数组 `[]` 表示使用默认的“按需激活”——即用户第一次执行命令或打开特定文件时才会激活扩展。也可以显式声明激活条件，例如 `"onCommand:helloworld.showCPUInfo"`。

```json
  "main": "./out/extension.js",
```
- **`main`**：扩展的入口文件路径。这里指向编译后的 JavaScript 文件（TypeScript 编译输出目录 `out` 中的 `extension.js`）。VS Code 会加载这个文件并调用其中的 `activate` 和 `deactivate` 函数。

```json
  "contributes": {
```
- **`contributes`**：这是扩展贡献点（contribution points）的容器，用来声明扩展提供的功能，如命令、菜单、快捷键、配置等。

```json
    "commands": [
      {
        "command": "helloworld.helloworld",
        "title": "Hello World test"
      },
      {
        "command": "helloworld.showCPUInfo",
        "title": "显示 CPU 信息",
        "category": "HelloWorld"
      }
    ],
```
- **`commands`**：定义扩展提供的命令列表。
  - 第一个命令：`command` 是命令的唯一标识符（在代码中用 `registerCommand` 注册时需要匹配）；`title` 是命令在命令面板中显示的名称。
  - 第二个命令：同样有 `command` 和 `title`，额外增加了 `category`（分类）。如果指定了 `category`，在命令面板中会显示为 `HelloWorld: 显示 CPU 信息`，起到分组作用。

```json
    "menus": {
      "editor/context": [
        {
          "command": "helloworld.showCPUInfo",
          "group": "navigation",
          "when": "editorTextFocus"
        }
      ]
    }
```
- **`menus`**：定义扩展在 VS Code 界面中插入的菜单项。这里只配置了 `editor/context`（编辑器右键菜单）。
  - `command`：关联的命令，必须是上面 `commands` 中已定义的。
  - `group`：控制菜单项在菜单中的位置。`navigation` 是预定义组，通常放在顶部（“导航”组）。
  - `when`：条件表达式，只有满足条件时菜单项才显示。`editorTextFocus` 表示编辑器获得焦点且处于文本编辑状态时显示。

```json
  },
```
- **`contributes` 对象结束。

```json
  "scripts": {
    "vscode:prepublish": "npm run compile",
    "compile": "tsc -p ./",
    "watch": "tsc -watch -p ./",
    "pretest": "npm run compile && npm run lint",
    "lint": "eslint src",
    "test": "vscode-test"
  },
```
- **`scripts`**：npm 脚本，用于开发、构建、测试等。
  - `vscode:prepublish`：在发布扩展前自动执行，这里运行 `compile` 编译 TypeScript。
  - `compile`：运行 TypeScript 编译器（`tsc`），`-p ./` 表示使用当前目录的 `tsconfig.json` 配置。
  - `watch`：以监听模式运行 `tsc`，文件变化时自动重新编译。
  - `pretest`：测试前运行，先编译再执行 lint（代码检查）。
  - `lint`：运行 ESLint 检查 `src` 目录下的代码。
  - `test`：运行 `vscode-test`，用于执行扩展的集成测试。

```json
  "devDependencies": {
    "@types/vscode": "^1.110.0",
    "@types/mocha": "^10.0.10",
    "@types/node": "22.x",
    "typescript-eslint": "^8.56.1",
    "eslint": "^9.39.3",
    "typescript": "^5.9.3",
    "@vscode/test-cli": "^0.0.12",
    "@vscode/test-electron": "^2.5.2"
  }
}
```
- **`devDependencies`**：开发时依赖的 npm 包（仅在开发阶段需要，运行时不需要）。
  - `@types/vscode`：VS Code 扩展 API 的 TypeScript 类型声明，用于代码提示和类型检查。
  - `@types/mocha`：Mocha 测试框架的类型声明（用于测试）。
  - `@types/node`：Node.js 核心模块的类型声明（因为扩展运行在 Node.js 环境中）。
  - `typescript-eslint`：ESLint 的 TypeScript 插件和解析器。
  - `eslint`：代码质量检查工具。
  - `typescript`：TypeScript 编译器本身。
  - `@vscode/test-cli` 和 `@vscode/test-electron`：用于在 CI 或本地运行扩展测试的工具（基于 Electron）。

---





## 选中文字打印场景

```ts
//extension.ts	

//选中文字打印模块
	const printSelectionCmd = vscode.commands.registerCommand('helloworld.printSelection', () => {
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
		// 打印到控制台
		console.log('选中的内容:', selectedText);
		// 可选：弹出消息框显示
		vscode.window.showInformationMessage(`选中内容: ${selectedText}`);
	});
```

```json
//package.json

"contributes": {
  "commands": [
    // 已有命令...
    {
      "command": "helloworld.printSelection",
      "title": "打印选中内容",
      "category": "HelloWorld"
    }
  ],
  "menus": {
    "editor/context": [
      // 已有菜单项...
      {
        "command": "helloworld.printSelection",
        "group": "navigation",
        "when": "editorHasSelection"
      }
    ]
  }
}
```





## 为命令绑定快捷键

```json
//package.json

{
  "contributes": {
    "commands": [
      // 已有的命令定义...
    ],
    "menus": [
      // 已有的菜单配置...
    ],
    "keybindings": [
      {
        "command": "helloworld.printSelection",
        "key": "alt+q",
        "when": "editorTextFocus && editorHasSelection"
      }
    ]
  }
}
```

新增keybindings配置项

```

```

