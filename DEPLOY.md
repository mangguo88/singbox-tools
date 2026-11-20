# singbox 节点命令面板 — 部署教程

把这个面板部署成你自己的在线地址，全程免费，不用服务器。

## 这个面板是什么

- 一个纯静态单页 HTML：`index.html`，在仓库根目录。
- 没有后端，不用构建，不用数据库。推到 `main` 分支就自动上线。
- 打开后在线勾选协议、端口、SNI、Argo、WS-CDN 等参数，生成一条 SSH 一键安装命令，复制到服务器上粘贴执行即可。
- 生成的安装命令会从 GitHub 下载安装脚本，地址是 `https://raw.githubusercontent.com/mangguo88/singbox-tools/refs/heads/main/`。

在线体验：https://singbox-tools.pages.dev/

## 准备

- 一个 GitHub 账号。
- 一个 Cloudflare 账号（免费版就够，[dash.cloudflare.com](https://dash.cloudflare.com) 注册）。
- 一个域名（可选）。没有域名也能部署，先用 Cloudflare 给的 `xxx.pages.dev` 地址；有域名再绑上去。

## 第 1 步：Fork 仓库（只针对 Fork 的人）

如果你是把这个仓库 Fork 到自己账号下部署，**必须先改 1 行**，否则面板生成的安装命令下载的还是原作者的脚本。

1. 把仓库 Fork 到你自己的 GitHub 账号。
2. 打开你 Fork 后仓库里的 `index.html`，搜 `mangguo88`（全文件只有 1 处）。
3. 把那 1 处改成你自己的 GitHub 用户名。也就是把：

   `https://raw.githubusercontent.com/mangguo88/singbox-tools/refs/heads/main/`

   改成：

   `https://raw.githubusercontent.com/你的用户名/singbox-tools/refs/heads/main/`

4. 提交这个修改。

> 如果你直接用的就是原仓库（没 Fork），这一步跳过，`index.html` 里的地址已经是对的。

## 第 2 步：Cloudflare Pages 关联 Git 部署

1. 打开 Cloudflare 面板（[dash.cloudflare.com](https://dash.cloudflare.com)），登录。
2. 左侧点「Workers & Pages」，再点「创建应用程序」。
3. 选「Pages」→「连接到 Git」。
4. 选 GitHub，授权后选仓库 `singbox-tools`（如果列表里没有，先授权 Cloudflare 的 GitHub 应用、勾上这个仓库）。
5. 填构建设置：
   - 项目名称：自己起一个，比如 `singbox-panel`（这就是以后 `xxx.pages.dev` 的名字）
   - 生产分支：`main`
   - 框架预设：`无`
   - 构建命令：留空
   - 构建输出目录：`/`（根目录）
6. 点「保存并部署」，等一会儿。部署成功后先拿到一个地址，比如 `https://singbox-panel.pages.dev`。

> 以后每次往 `main` 分支推代码（改面板、改教程），Pages 都会自动重新部署，不用手动操作。

## 第 3 步：绑自己的域名（可选）

1. 进这个 Pages 项目，点「自定义域」→「设置自定义域」。
2. 输入你的域名，比如 `singbox.你的域名`，点「继续」→「激活域」。
3. 域名的 DNS 在你的 Cloudflare 账号里：Cloudflare 会自动加解析记录，等几分钟就生效。
4. 域名的 DNS 不在 Cloudflare：Cloudflare 会给你一个 CNAME 目标，你去域名注册商那里，把这个子域名 CNAME 到 `xxx.pages.dev`。

## 第 4 步：验证

1. 打开你的新地址（`xxx.pages.dev` 或绑好的自定义域名），面板能正常打开。
2. 随便勾一个协议、填个端口，点「复制命令」，看生成的命令里是不是 `raw.githubusercontent.com/你的用户名/singbox-tools`（Fork 的人）或 `raw.githubusercontent.com/mangguo88/singbox-tools`（原仓库）。
3. 找台服务器，把命令粘贴进去跑一遍，确认能正常安装。

## 常见问题

- **打开还是旧版面**：浏览器缓存了旧文件。硬刷新一次（电脑 Ctrl+Shift+R，手机清一下缓存再进），或等几分钟再试。
- **生成的命令下载的不是我的脚本**：`index.html` 里那 1 处 `mangguo88` 没改成你的用户名。按第 1 步改完、推到 `main`，等 Pages 自动重新部署。
- **Pages 部署失败**：检查构建设置——框架预设是「无」、构建命令是空、构建输出目录是 `/`。这三项不对就会失败。
- **自定义域名打不开**：先确认 `xxx.pages.dev` 本身能打开（能打开说明部署没问题，是域名解析的事）；再看 DNS 记录是不是指对了，等解析生效（一般几分钟，最长几小时）。
- **想换个名字 / 换个 pages.dev 地址**：改 Pages 项目的名称即可，不用动仓库。
