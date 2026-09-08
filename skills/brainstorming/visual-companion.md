# 视觉伴侣指南

用于展示效果稿、线框图、布局比较和示意图。是否启用、何时征求反馈，遵循 [SKILL.md](SKILL.md) 的对话与授权规则；本指南说明具体操作。

服务监听 `screen_dir` 中的 HTML 文件，按修改时间展示最新页面。用户点击选项后，事件写入 `state_dir/events`，供 Agent 结合对话读取。

## 启动与连接

需要 Node.js、Bash 和浏览器。以下命令在本 Skill 目录执行；从其他目录调用时使用脚本的实际路径。

```bash
# 将 /path/to/project 替换为项目绝对路径。
# 用户同意启用后使用 --open，首次新增页面时自动打开浏览器。
bash scripts/start-server.sh --project-dir /path/to/project --open
```

遵守当前环境的浏览器路由规则；使用指定的浏览器工具打开页面时，可省略 `--open`，通过该工具打开返回的完整 URL。

启动输出包含 `url`、`screen_dir`、`state_dir` 等字段，也会保存到 `state_dir/server-info`。记录返回值，后续直接使用，不自行拼接会话目录。

- **连接地址**：首次打开或分享连接时，使用包含 `?key=…` 的完整 `url`。浏览器首次访问后通过 cookie 和会话存储维持页面、资源及 WebSocket 鉴权；不要把地址栏后来显示的无密钥地址当作新的连接入口。无需每轮重复发送 URL。
- **持久化**：传入 `--project-dir` 后，页面保存在项目的 `.superpowers/brainstorm/` 下。该目录含会话密钥与运行状态，应避免提交到 Git；按项目规则加入忽略列表。省略此参数会使用 `/tmp` 临时会话。
- **进程存活**：脚本通常自行后台运行；检测到 `CODEX_CI` 或 Windows 类 shell 时改为前台。会回收后台进程的环境，使用 `--foreground`，并通过当前工具支持的持久进程或后台执行机制运行。不要让等待服务退出的工具调用一直阻塞对话。Windows 使用 Git Bash 或 WSL。
- **远程环境**：默认监听 `127.0.0.1`。浏览器与服务不在同一环境时，按实际网络或端口转发配置连接；确需非回环监听时使用 `--host`，用 `--url-host` 指定浏览器能访问的主机名。修改显示主机名本身不会建立网络连通性。

## 展示与读取反馈

1. 展示前检查 `state_dir/server-info` 和 `state_dir/server-stopped`。存在停止标记、缺少连接信息或浏览器无法连接时，先核实服务是否存活；状态文件本身不保证进程仍在运行。
2. 将页面写入 `screen_dir`。每个新的讨论页面使用新文件名，如 `layout.html`、`layout-v2.html`。新增文件会清空旧事件并触发刷新；覆盖已有文件只刷新，不清空事件。读取需要保留的反馈后再切换页面。
3. 简要说明页面要帮助判断什么。需要用户作出视觉选择时再等待反馈，无需每屏都套用独立审批流程。
4. 读取 `state_dir/events`，结合用户消息理解选择。事件是逐行 JSON；不要把最后一次点击机械地当作最终决定，尤其是多选或文字反馈与点击不一致时。

```jsonl
{"type":"click","choice":"a","text":"方案 A：单栏布局","timestamp":1706000101000}
```

没有事件文件不一定代表用户未操作：新页面会清空事件，连接中断也可能延迟写入。已有用户文字反馈可直接使用，不必为了收集点击再走一遍流程。

转回文字讨论时，如果旧页面容易造成误解，可以新增简短的等待页；无需每次切换都清屏。

## 编写页面

默认写 HTML 片段，服务会补齐页面框架、主题样式和交互脚本。文件以 `<!DOCTYPE` 或 `<html` 开头时按完整文档处理，只注入辅助脚本；完整文档需自行提供布局和样式；辅助脚本仍提供下例使用的 `toggleSelect`，自定义交互则需另行实现。

```html
<h2>哪种布局更适合阅读？</h2>
<p class="subtitle">比较正文宽度和导航位置。</p>
<div class="options">
  <div class="option" data-choice="a" onclick="toggleSelect(this)">
    <div class="letter">A</div>
    <div class="content">
      <h3>单栏</h3>
      <p>正文集中，减少干扰。</p>
    </div>
  </div>
  <div class="option" data-choice="b" onclick="toggleSelect(this)">
    <div class="letter">B</div>
    <div class="content">
      <h3>双栏</h3>
      <p>侧栏导航与正文并排。</p>
    </div>
  </div>
</div>
```

给 `.options` 容器添加 `data-multiselect` 可启用多选。保持 `data-choice` 在当前页面内可区分；`toggleSelect(this)` 负责选中状态，带 `data-choice` 的元素点击由辅助脚本记录并发送。

常用样式速查，具体结构见 [页面模板](scripts/frame-template.html)：

| 用途 | 类名 |
| --- | --- |
| 选项 | `options`、`option`、`letter`、`content` |
| 视觉卡片 | `cards`、`card`、`card-image`、`card-body` |
| 效果稿与并排比较 | `mockup`、`mockup-header`、`mockup-body`、`split` |
| 优缺点 | `pros-cons`、`pros`、`cons` |
| 线框元素 | `mock-nav`、`mock-sidebar`、`mock-content`、`mock-button`、`mock-input`、`placeholder` |
| 文字与分区 | `subtitle`、`section`、`label` |

页面围绕当前决策组织：比较布局时用线框，判断视觉风格时再提高精细度。选项数量以容易比较为准，内容影响判断时使用有代表性的内容。浏览器重连和事件发送实现见 [辅助脚本](scripts/helper.js)。

## 恢复与结束

默认空闲 4 小时后服务退出，可通过 `--idle-timeout-minutes` 调整。启动环境退出也可能触发停止。

恢复时使用相同的 `--project-dir` 重新启动。脚本会尝试复用项目端口和密钥，但会创建新的会话目录；重新记录返回的 `screen_dir` 和 `state_dir`，需要继续展示时，将之前的页面复制到新的 `screen_dir`。端口冲突可能导致换端口，因此检查新 `url`，不要保证旧标签页一定能自动恢复。

结束会话时，以 `state_dir` 的父目录作为会话目录，替换下面的占位路径：

```bash
bash scripts/stop-server.sh /path/to/session
```

脚本通过会话实例标识核验进程后停止服务；正常停止 `/tmp` 下的临时会话时会删除该会话目录，项目内的持久化页面会保留。若返回 `stale_pid` 或 `not_running`，不要根据旧 PID 手动杀进程，也不要假定清理已完成。
