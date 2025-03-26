
### TL,DR;

- **`feat`**：新功能。
- **`chore`**：日常维护。
- **`fix`**：修 bug。
- **`docs`**：文档。
- **`style`**：格式。
- **`refactor`**：重构。
- **`test`**：测试。
- **`perf`**：性能。
- **`ci`**：CI/CD。
- **`build`**：构建。
- **`revert`**：回滚。

这些术语在 GitHub 上没有强制要求，但遵循约定式提交的项目会广泛使用。

在 GitHub（以及更广泛的 Git 工作流）中，`feat`、`chore` 等术语通常出现在 commit 消息或 Pull Request (PR) 的描述中，特别是在遵循 **Conventional Commits**（约定式提交）规范的项目中。这些前缀用来描述提交的类型，帮助团队快速理解更改的目的。以下是常见的前缀及其含义：

---

### 常见类型及其含义

1. **`feat`（Feature）**
   - **含义**：表示新增功能或特性。
   - **使用场景**：当你实现了一个新的功能模块、用户可见的改进或新特性时使用。
   - **示例**：

     ```
     feat: add user login functionality
     feat: implement search bar in header
     ```

   - **影响**：通常与版本号的小版本提升相关（例如从 `1.0.0` 到 `1.1.0`）。

2. **`chore`（Chore）**
   - **含义**：表示日常维护任务或杂项工作，不直接影响用户功能。
   - **使用场景**：更新依赖、调整构建脚本、修改配置文件、代码格式化等。
   - **示例**：

     ```
     chore: update npm dependencies to latest versions
     chore: format code with prettier
     ```

   - **影响**：通常不触发版本号变更，除非涉及重大依赖更新。

3. **`fix`（Fix）**
   - **含义**：修复 bug 或问题。
   - **使用场景**：解决代码中的错误、用户报告的问题或意外行为。
   - **示例**：

     ```
     fix: resolve crash on invalid input
     fix: correct typo in login error message
     ```

   - **影响**：通常与补丁版本提升相关（例如 `1.0.0` 到 `1.0.1`）。

4. **`docs`（Documentation）**
   - **含义**：文档相关的更改。
   - **使用场景**：更新 README、API 文档、注释或用户指南。
   - **示例**：

     ```
     docs: add installation instructions to README
     docs: update API endpoint documentation
     ```

5. **`style`（Style）**
   - **含义**：代码风格或格式调整，不影响功能。
   - **使用场景**：调整缩进、删除多余空格、统一命名风格等。
   - **示例**：

     ```
     style: remove trailing whitespace
     style: rename variable to camelCase
     ```

6. **`refactor`（Refactor）**
   - **含义**：代码重构，不新增功能也不修复 bug。
   - **使用场景**：优化代码结构、提高可读性或性能。
   - **示例**：

     ```
     refactor: split large function into smaller ones
     refactor: use async/await instead of promises
     ```

7. **`test`（Test）**
   - **含义**：添加或修改测试代码。
   - **使用场景**：新增单元测试、集成测试或修复测试用例。
   - **示例**：

     ```
     test: add unit tests for login module
     test: fix failing CI test case
     ```

8. **`perf`（Performance）**
   - **含义**：性能优化。
   - **使用场景**：改进代码执行效率或减少资源占用。
   - **示例**：

     ```
     perf: optimize database query for faster response
     perf: cache API results to reduce load time
     ```

9. **`ci`（Continuous Integration）**
   - **含义**：与持续集成/持续部署 (CI/CD) 相关的更改。
   - **使用场景**：修改 GitHub Actions、Jenkins 配置等。
   - **示例**：

     ```
     ci: add linting step to GitHub Actions workflow
     ci: fix build script for Node.js 18
     ```

10. **`build`（Build）**
    - **含义**：构建系统或外部依赖的更改。
    - **使用场景**：修改构建工具、打包配置或版本号。
    - **示例**：

      ```
      build: update webpack config for production
      build: bump version to 1.2.0
      ```

11. **`revert`（Revert）**
    - **含义**：回滚之前的提交。
    - **使用场景**：撤销某个错误的 commit。
    - **示例**：

      ```
      revert: revert "feat: add broken feature" due to bugs
      ```

---

### Conventional Commits 规范
这些前缀来自 **Conventional Commits**（约定式提交），一个轻量级的提交消息规范，格式通常是：

```
<类型>(<范围>): <描述>
```

- **类型**：`feat`、`fix` 等。
- **范围**（可选）：影响的模块或文件，例如 `(api)`、`(ui)`。
- **描述**：简短说明更改内容。

#### 示例

```
feat(api): add endpoint for user authentication
fix(ui): correct button alignment on mobile
chore: update eslint to v8.0.0
```

---

### 在 GitHub 中的应用
1. **Pull Request**：
   - PR 标题或描述可能使用这些前缀，例如：
     - `feat: Implement dark mode toggle`
     - `chore: Update CI pipeline`
   - 帮助 reviewer 快速判断 PR 的性质。

2. **自动化工具**：
   - 项目可能使用工具（如 `semantic-release`）根据提交类型自动生成版本号和 changelog：
     - `feat` → 小版本号提升（minor）。
     - `fix` → 补丁版本提升（patch）。
     - `chore`、`docs` 等通常不影响版本。

3. **团队协作**：
   - 统一前缀提高代码历史的可读性，方便追踪功能开发和问题修复。

---

