# AIReader

在 Mac 上阅读 HTML、Markdown 和 PDF，选中文字即可带着上下文向 AI 提问。HTML 的排版、公式和交互会保留，问答与标注保存在文档旁边。

也可以在 Worker 中管理本机终端、连接远程 tmux，查看本机 Claude / Codex 的对话记录。

## 下载与安装

[下载最新版本](https://github.com/amphiscope/aireader/releases/latest) · macOS / Apple Silicon

1. 下载 DMG，打开后将 `AIReader.app` 拖进「应用程序」；也可下载 ZIP 解压安装。
2. 应用使用 ad-hoc 签名，尚未完成 Apple 公证。如果被系统拦截，确认来源后到「系统设置 → 隐私与安全性」选择「仍要打开」。
3. 打开本地文档即可阅读。使用 AI 时，在 Reader 中选择「API 模型 → 配置模型」，填入自己的服务信息；Codex 订阅通道的要求见随包安装说明。

当前提供的是 `0.1.7-rc.7` 候选版，仍在调试。更新内容、已知问题和 SHA256 校验文件见下载页。模型账号及额度需自备。

## 许可

源码暂未公开，本仓库只提供说明和安装包。保留所有权利。可自由下载使用构建版本；未授权再分发、反编译或商用。
