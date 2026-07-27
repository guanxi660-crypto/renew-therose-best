# 🌹 TheRose Cloud Auto Renew & Maintain

这是一个基于 Python (`SeleniumBase`) 和 GitHub Actions 构建的自动化脚本项目。专门用于 **TheRose Cloud** 面板服务器的无人值守自动续期与运行状态维护。

## ✨ 核心功能

*   **🛡️ 自动登录与人机验证**: 利用 SeleniumBase 的 `uc` (Undetected ChromeDriver) 模式，自动处理登录页面的 Cloudflare Turnstile 人机验证。
*   **🛒 智能自动续期**: 定时监控服务器到期时间，在允许续期的窗口期内自动寻找并点击 `Extend` 和 `Order now` 完成续期。
*   **⚙️ 智能状态维护 (中英文兼容)**: 
    *   **关机自动启动**: 若检测到服务器处于离线状态，自动点击“启动 (Start)”。
    *   **开机自动重启**: 若检测到服务器正在运行，自动点击“重启 (Restart)”。
    *   *注：底层逻辑已做深度优化，即使因 Linux 无中文字体导致面板按钮文字显示为乱码（方块），依然能精准识别并执行点击。*
*   **📨 Telegram 运行汇报**: 无论成功或失败，脚本都会将关键步骤的截图及结果推送至指定的 Telegram 聊天中。
*   **🤖 GitHub Actions 托管**: 无需本地服务器挂机，利用 GitHub 提供的免费算力，默认每 55 分钟自动执行一次全套流程。

---

## 🚀 部署指南

### 1. Fork 本仓库
点击页面右上角的 `Fork` 按钮，将本仓库克隆到你自己的 GitHub 账号下。

### 2. 配置环境变量 (Secrets)
脚本的运行高度依赖环境变量。请前往你 Fork 后的仓库，依次点击 **Settings** -> **Secrets and variables** -> **Actions** -> **New repository secret**，添加以下必要变量：

| Secret 名称 | 必须 | 说明 | 示例 |
| :--- | :---: | :--- | :--- |
| `EMAIL` | ✅ | TheRose Cloud 的登录邮箱 | `user@example.com` |
| `PASSWORD` | ✅ | TheRose Cloud 的登录密码 | `YourPassword123` |
| `SERVER_URL` | ✅ | 目标服务器控制台的直达链接 | `https://panel.therose.cloud/server/1ce3ddfb` |
| `TG_BOT_TOKEN` | ✅ | Telegram 机器人的 Token (通过 @BotFather 获取) | `123456789:ABCdefGhIJKlmNoPQRsTUVwxyZ` |
| `TG_CHAT_ID` | ✅ | 接收通知的 Telegram 账号或群组 ID | `987654321` |
| `NODE_LINK` | ✅ | 代理节点链接 (用于 Sing-box 代理，突破区域限制) | `vless://...` 或 `vmess://...` |

### 3. 启用 GitHub Actions
1. 进入仓库的 **Actions** 选项卡。
2. 点击 **"I understand my workflows, go ahead and enable them"**（如果你看到了这个提示）。
3. 在左侧列表中选中 **Auto Renew therose** 工作流。
4. 点击右侧的 **Run workflow** 手动触发一次运行，检查配置是否正确。

---

## 📁 文件结构说明

*   `therose.py`: 核心自动化脚本，包含所有的 Selenium 网页交互逻辑、DOM 元素定位、降级 JS 注入点击以及消息推送功能。
*   `.github/workflows/therose.yml`: GitHub Actions 工作流配置文件。定义了运行环境 (Ubuntu)、Python 版本、依赖安装、虚拟显示器 (`xvfb`) 挂载、定时任务配置 (Cron) 以及历史记录清理逻辑。
*   `logo.png` *(可选)*: 如果根目录下存在此图片，Telegram 发送通知时会带上该 Logo 增强排版美观度。

---

## ⚠️ 常见排错与注意事项

1. **面板出现新验证机制**: 如果官方更新了面板的防爬虫机制，可能导致 `login_failed.png` 截图报错，需要同步更新 SeleniumBase 或调整定位器。
2. **代理失效**: 如果 `NODE_LINK` 指定的节点失效，将导致网络连接超时。请确保提供的节点长期稳定。
3. **查看运行截图**: 每次运行结束后（无论成功失败），在 Actions 运行记录的底部的 `Artifacts` 中会生成 `run-screenshots` 压缩包，下载即可查看执行时的网页截图，方便定位故障。

> **免责声明**: 本脚本仅供学习交流与自动化技术研究使用，请合理设置请求频率，遵守服务商的 TOS（服务条款）。
