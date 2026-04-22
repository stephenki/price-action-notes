# 代码执行方式对比：直接执行 vs Subagent

## 直接执行（execute_code）

### 执行环境
- 沙盒进程：每次调用都是全新的 Python 进程
- 无状态：变量、导入、安装的包在调用间不保留
- 超时限制：单次最多 5 分钟，stdout 50KB 上限

### 可用能力
- **内置工具**：`from hermes_tools import ...`（terminal, file, web, session_search 等）
- **标准库**：Python stdlib（json, re, os, subprocess 等）
- **外部依赖**：只能使用已预装的包（如 zipfile, bs4 可能不存在）

### 特点
✅ 可控性强（我直接编写所有逻辑）  
✅ 调试直接（看到完整错误）  
✅ 上下文完整（我能看到全部中间结果）  
❌ 无法交互（不能调用需要 stdin 的程序）  
❌ 不能安装新依赖（除非用 terminal 提前装好）  
❌ 不能保留状态（每次重来）

### 适用场景
- 数据处理、文件批量操作
- API 调用组合（web_search → extract → parse）
- 简单脚本（5-10 个工具调用以内）
- 需要我全程掌控逻辑的任务

---

## 使用 Subagent（delegate_task）

### 执行环境
- 独立会话：完全隔离的 Hermes 会话
- 长期进程：可以运行长时间服务（开发服务器、测试套件）
- 状态保留：同一子进程内多次调用共享环境

### 可用能力
- **完整工具集**：继承父级启用的工具（browser, terminal, file, web 等）
- **交互式 CLI**：支持 pty 模式，可运行需要交互的程序
- **进程管理**：background=true, notify_on_complete=True
- **A/ACP 协议**：可调用 Claude Code, OpenCode 等外部 agent

### 特点
✅ 适合复杂/迭代任务（调试、代码审查、研究）  
✅ 可并行（max_concurrent_children，最多 3 个同时跑）  
✅ 长期运行（不阻塞主会话）  
✅ 交互式工具友好（IDE、REPL、测试观察者）  
❌ 成本更高（额外会话开销）  
❌ 控制粒度粗（只能看到最终摘要）  
❌ 上下文隔离（无法直接访问主会话的变量/文件）  
❌ 不能调用我的工具（delegate_task, clarify, memory, execute_code）

### 适用场景
- **代码生成/重构**：交给 Claude Code 或 OpenCode 写完整模块
- **调试/测试**：运行测试套件、观察日志、迭代修复
- **研究/综合**：并行搜索多个主题，合并结果
- **长时间任务**：构建、部署、监控（不阻塞主流程）

---

## 核心差异总结

| 维度 | execute_code（我直接写） | delegate_task（子代理） |
|------|------------------------|----------------------|
| **控制粒度** | 行级控制，能看到每一步 | 任务级控制，只看最终摘要 |
| **状态管理** | 无状态（每次全新进程） | 有状态（子会话内保留） |
| **复杂度上限** | 适合线性、原子任务 | 适合迭代、探索性任务 |
| **工具调用** | 可使用 hermes_tools（含工具调用） | 继承工具集，但不能调用 delegate_task |
| **交互能力** | ❌ 不能 stdin 交互 | ✅ 可 pty 模式交互 |
| **并行性** | 顺序执行 | 可并行（最多 3 个子任务） |
| **成本** | 单次调用，成本低 | 额外会话开销，成本较高 |
| **调试** | 直接看到 stdout/stderr | 需依赖子代理返回的摘要 |
| **适用规模** | 5-15 个工具调用以内 | 50+ 工具调用、多轮推理 |

---

## 决策树：何时用哪种方式？

**用 execute_code（我直接写）：**
- 任务明确，步骤确定（例如：批量重命名 100 个文件）
- 需要精细控制中间结果（例如：逐步过滤数据）
- 依赖少，标准库足够
- 不需要交互（非 stdin 程序）

**用 delegate_task（子代理）：**
- 需要代码生成/重构（交给 Claude Code / OpenCode）
- 任务复杂、路径不确定（研究、调试）
- 需要长时间运行（测试、构建、部署）
- 需要并行处理（同时搜索 3 个主题）
- 需要交互式工具（运行需要输入的命令）

---

## 实际示例对比

### 场景：提取 EPUB 章节并生成笔记

**直接执行（execute_code）**：
```python
# 我直接写：读取 EPUB → 提取 HTML → 清理 → 保存 → 更新 MEMORY
import zipfile, re, os
with zipfile.ZipFile(epub) as zf:
    html = zf.read('OEBPS/9781118172339_epub_c_01.htm')
    text = clean_html(html)
    write_file('book2/chapter-1.md', format_notes(text))
    update_memory('2026-04-20', 'Ch1')
```
→ 适合：一次性的、逻辑线性的提取任务

**子代理（delegate_task）**：
```yaml
goal: "提取 Book 2 Chapter 1-5 的 HTML，生成详细笔记，检查每章字数是否达标"
toolsets: ['terminal', 'file', 'web']
max_iterations: 100  # 子代理可迭代调试
```
→ 适合：章节多、可能需要调整格式、反复检查的探索性任务

---

## 成本与性能

**execute_code**：
- 成本：单次调用 token 消耗
- 性能：即时返回，无额外开销

**delegate_task**：
- 成本：子代理会话的完整上下文 + 工具调用开销
- 性能：需等待子代理完成，但可异步（notify_on_complete）

**建议**：原子任务用 execute_code，复杂/迭代任务用 delegate_task。

---

*最后更新: 2026-04-19*
