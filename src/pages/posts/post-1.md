### 从零搭建个人博客

### 核心思路：静态网站生成器 + Git 平台托管

这是目前搭建个人博客最主流、最经济实惠的技术方案，**完全免费**（如果你不使用自定义域名的话）。

*   **静态网站生成器 (SSG)**：你用 `Markdown` 写文章，它帮你编译生成一整套完整的 `HTML`, `CSS`, `JS` 静态网站文件。优点是无数据库、速度快、安全性高。
*   **Git 平台托管**：将生成的静态网站文件推送（Git）到托管平台（如 GitHub, GitLab, Gitee），平台会提供免费的服务器和域名（如 `yourname.github.io`）来展示你的网站。

---

### 第一步：选择你的技术栈（核心决策）

根据你的技术偏好，选择一个静态网站生成器。以下是前端程序员最流行的几个选择：

| 生成器 | 语言 | 特点 | 适合人群 |
| :--- | :--- | :--- | :--- |
| **Hexo** | Node.js | 生态丰富，主题插件多，上手快，文档成熟 | 初学者，喜欢众多主题选择的人 |
| **Hugo** | Go | **速度极快**，单二进制文件无需复杂环境，性能好 | 追求效率和简洁的开发者 |
| **VuePress** | Vue.js | 由 Vue 驱动，**技术栈统一**，适合写技术文档 | Vue 开发者，希望深度自定义 |
| **VitePress** | Vue.js | VuePress 的轻量级兄弟，**更快更简单** | 追求极致速度和现代感的开发者 |
| **Astro** | 多框架 | 可以混合使用 React, Vue 等，主打 **“岛屿架构”**，性能优异 | 喜欢折腾新技术，希望站点交互性可控 |
| **Gatsby** | React/GraphQL | 功能强大，插件生态极其丰富，但学习曲线稍陡 | React 重度开发者 |

**给你的建议：**
*   **如果求快、省心**：选 **Hexo** 或 **Hugo**。5分钟就能搭起来。
*   **如果你是 Vue 开发者**：强烈推荐 **VitePress**，和你技术栈完美契合，自定义起来得心应手。
*   **如果你想学点新东西**：试试 **Astro**，理念很前沿。

---

### 第二步：详细搭建步骤（以 Hexo + GitHub Pages 为例）

这是最经典的组合，流程其他工具也大同小异。

#### 1. 准备环境
*   安装 [Node.js](https://nodejs.org/)
*   安装 [Git](https://git-scm.com/)
*   注册一个 [GitHub](https://github.com/) 账号

#### 2. 安装和初始化 Hexo
```bash
# 安装 Hexo 命令行工具
npm install -g hexo-cli

# 初始化一个博客项目，`my-blog` 是你的文件夹名
hexo init my-blog

# 进入项目目录
cd my-blog

# 安装依赖
npm install

# 在本地启动预览服务器
hexo server
```
现在打开 `http://localhost:4000` 就能看到你的博客雏形了！

#### 3. 写你的第一篇文章
在 `source/_posts` 目录下新建一个 `.md` 文件，比如 `hello-world.md`。

```markdown
---
title: 我的第一篇文章
date: 2024-05-20
tags:
  - 随笔
---

## 大家好！

这是我的博客第一篇文章，用 **Markdown** 写作真方便！
```
保存后，刷新本地预览页面，文章就出现了。

#### 4. 部署到 GitHub Pages
*   在 GitHub 上创建一个名为 **`你的用户名.github.io`** 的仓库（必须严格遵循此命名）。
*   在博客根目录安装部署插件：
    ```bash
    npm install hexo-deployer-git --save
    ```
*   修改根目录下的 `_config.yml` 文件，找到 `deploy` 部分并配置：

    ```yaml
    # Deployment
    ## Docs: https://hexo.io/docs/one-command-deployment
    deploy:
      type: git
      repo: https://github.com/你的用户名/你的用户名.github.io.git # 你的仓库地址
      branch: main
    ```

*   执行部署命令：
    ```bash
    hexo clean && hexo deploy
    ```
    完成后，访问 `https://你的用户名.github.io`，你的博客就上线了！

---

### 第三步：进阶优化与个性化（低成本）

1.  **更换主题**
    *   Hexo: 在 [官方主题库](https://hexo.io/themes/) 找喜欢的主题，按照主题文档安装（通常是 git clone 到 `themes` 目录下并修改配置）。
    *   VitePress：默认主题就很棒，也可以自己用 Vue 组件完全自定义。

2.  **购买自定义域名（可选，非必需）**
    *   在 Namesilo, Namecheap 或国内腾讯云、阿里云购买一个喜欢的域名（年费通常几十元）。
    *   在你的博客源文件 `source` 目录下，创建一个名为 `CNAME` 的文件（无后缀），里面写上你的域名（如 `blog.yourname.com`）。
    *   在域名注册商的控制面板中，为域名添加 **CNAME 记录**，指向你的 GitHub Pages 地址（`你的用户名.github.io`）。
    *   重新部署 `hexo deploy`。等待 DNS 生效后，就可以用自定义域名访问了。

3.  **自动化部署（加分项）**
    现在的流程是你写完文章要手动执行 `hexo deploy`。你可以使用 **GitHub Actions** 实现自动化：
    *   将整个博客**源码**推送到另一个仓库（如 `my-blog-source`）。
    *   编写一个 `.github/workflows/deploy.yml` 工作流文件，每当 `main` 分支有推送时，自动触发构建和部署到 `你的用户名.github.io` 仓库。
    *   这样你只需要 `git push` 文章源码，网站就会自动更新。

4.  **评论系统**
    静态博客没有后端，评论需要借助第三方服务：
    *   **Giscus** (**推荐**)：利用 GitHub Discussions，评论就是 GitHub 讨论，程序员首选。
    *   **Utterances**：类似，利用 GitHub Issues。
    *   **Waline**：一个简洁的自托管评论系统，如果你有服务器可以考虑。

---

### 方案对比与总结

| 方案 | 成本 | 优点 | 缺点 | 适合谁 |
| :--- | :--- | :--- | :--- | :--- |
| **Hexo/Hugo + GitHub Pages** | **几乎为0** | 极低成本，速度快，完全控制，适合学习 | 需要本地环境和命令行操作 | **前端开发者，初学者及以上** |
| **WordPress + 虚拟主机** | 域名费 + 主机费（年付几百元） | 后台强大，插件生态无敌，所见即所得 | 需要维护，可能 slower，有安全风险 | 非技术用户，喜欢传统后台 |
| **第三方平台（语雀/掘金/CSDN）** | **0** | 无需维护，自带流量 | 定制性差，数据不在自己手里 | 纯写作，不介意平台束缚 |

### 给你的最终建议

作为一名前端程序员，**我强烈建议你选择“静态生成器 + GitHub Pages”的方案**。

1.  **技术匹配**：整个过程你会用到 Node, Git, Markdown 等，都是你的本职工作技能，无缝衔接。
2.  **极致成本**：除了域名，其他几乎免费，并且享受的是顶级的 CDN 和托管服务。
3.  **完美实践**：这是你学习自动化部署、CI/CD（GitHub Actions）、版本管理的最佳练手项目。
4.  **完全掌控**：所有数据（文章是 Markdown 文件）都在你自己手里，不会受任何平台限制。

**行动起来：**
1.  从 **Hexo** 或 **VitePress** 中选一个。
2.  花 30 分钟跟着教程走一遍。
3.  今晚之前，你的博客就可以上线了！

祝你搭建顺利，享受写作和分享的乐趣！