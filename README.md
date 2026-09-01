# AIReader

**本地优先、上下文感知的 HTML 阅读器。**

读技术材料时，把整篇文档投喂给模型既贵又容易串味；只贴一段话，模型又不知道这段在讲什么。
AIReader 的做法是：打开一份 HTML 材料时**完整保留它自带的样式、脚本、公式与图表交互**，
同时只把**当前选区、所在标题路径、当前逻辑节点与必要邻文**编译成一份有限的模型上下文交给模型。

问的不是「这篇文档」，是「我正在读的这一段」。

## 它做什么

- **原样呈现**：HTML 在不带 `allow-same-origin` 的 iframe 沙箱里运行，作者写的 CSS / JS / KaTeX / 交互图表全部保留。
- **按位置取上下文**：划词后出现 `Ask AI`，自动带上选区、标题路径、当前逻辑节点与必要邻文；
  右侧有上下文检查器，可以看到这次到底把什么发给了模型。
- **结构导航**：自动抽取文档结构，识别并折叠 HTML 自带的左侧目录（释放原 grid/flex 占位），工具栏随时恢复。
- **伴生文件即事实源**：每份 HTML 配一个同目录、同基名的 `.aireader.json`，保存标记、完整问答与正文练习状态。
  不在数据库、IndexedDB 或 localStorage 里留隐藏副本——**HTML 和 JSON 一起移动，讨论记录就跟着走**。
- **正文内练习与批改**：识别材料里的编程/批改任务，编辑器、提交版本与批改结果留在正文原位，不挤进右侧问答栏。

## 现状

- macOS 本机应用（Apple Silicon），通过 `localhost` 使用。
- 一次打开一份 HTML，按依赖图读取它实际引用的本地资源，**不扫描同目录其他文档**。
- 远程 URL、完整 Wiki 检索、agent 命令执行尚未进入首版。
- **源码暂未公开**，仓库目前提供说明文档与构建版本。

## 模型配置

密钥由 daemon 自读，**不进入渲染进程、worker 或 CLI 输出**，文件须保持 `0600`：

```bash
mkdir -p ~/.config/aireader && chmod 700 ~/.config/aireader
cat > ~/.config/aireader/aireader.env <<'ENV'
DEEPSEEK_API_KEY=your-key-here
DEEPSEEK_BASE_URL=https://api.deepseek.com
DEEPSEEK_MODEL=deepseek-v4-flash
ENV
chmod 600 ~/.config/aireader/aireader.env
```

同一文件里可另配 `MOONSHOT_API_KEY`、`ZHIPU_API_KEY` 及各自可选的 `*_BASE_URL` / `*_MODEL`。
自建的 OpenAI 兼容端点通过 `VLLM_BASE_URL` + `VLLM_MODEL` 显式启用（`VLLM_API_KEY` 可选，
视觉端点另设 `VLLM_VISION=true`）。

**未配置任何 key 时，应用会明确进入上下文演示模式**——仍可浏览、取上下文、查看检查器，只是不产生模型回答。

## 写给 AIReader 的 HTML

生成阅读材料的工具可以遵循一份创作规范，让阅读器 100% 确定地识别目录、公式与分节上下文：
标题带稳定 id、目录容器盖 `data-aireader-toc="true"`、内嵌一份 `data-aireader-context` 上下文清单、
公式用离线 KaTeX 保留可恢复的 TeX 源。不遵循也能打开，只是部分能力降级。

## 许可

保留所有权利。可自由下载使用构建版本；未授权再分发、反编译或商用。
