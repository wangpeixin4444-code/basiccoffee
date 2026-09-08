# 🚀 部署指南：basiccoffee.cn 上线流程

把 `coffee-demo` 这个咖啡科普小站部署到你自己的域名 `basiccoffee.cn`，全程约 **30-60 分钟**，域名 ¥38/首年起。

---

## 📌 整体流程一览

```
① 注册域名 basiccoffee.cn（你·阿里云）           ⏱ 5–10 分钟（含实名）
② 修改 DNS 准备（你·阿里云 DNS）                  ⏱ 2 分钟  
③ 创建 GitHub 仓库并推送代码（我帮你+你执行）      ⏱ 5 分钟
④ Cloudflare Pages 接入 GitHub 仓库（你）          ⏱ 5 分钟
⑤ 在 Pages 绑定自定义域名（你）                    ⏱ 5 分钟
⑥ 在 阿里云 DNS 添加 CNAME 记录（你）              ⏱ 3 分钟
⑦ 等 Cloudflare 自动验证并签发 SSL               ⏱ 5–30 分钟
⑧ 完成 ✅                                           ⏱ 总计 < 1 小时
```

---

## 步骤 1️⃣ — 注册 basiccoffee.cn（约 5 分钟）

> 💰 预计花费 **¥38 首年**，续费约 ¥40-55/年

1. 打开 https://wanwang.aliyun.com/
2. 搜索框输入 `basiccoffee.cn`
3. 点击 **"加入清单"** → **"立即购买"**
4. 若未登录：用支付宝扫码登录
5. 若未实名：按提示完成**域名持有者实名认证**（身份证 + 人脸，约 3 分钟）
6. 选择注册年限（建议先 1 年试水）
7. **支付宝/余额宝**支付
8. 注册成功后，进入 **"我的云栖" → "域名"** 应该能看到 `basiccoffee.cn`

---

## 步骤 2️⃣ — 在 Cloudflare 准备账号

1. 打开 https://dash.cloudflare.com/sign-up
2. 用邮箱注册（建议用 Gmail）
3. 登录后先不用添加域名，等步骤 5 再做

---

## 步骤 3️⃣ — 推送代码到 GitHub（最关键）

### A. 我会帮你准备好 `git` 命令清单

包括：
- 仓库初始化
- 配置作者信息
- 添加全部文件
- 首次提交

### B. 你需要做的事（在 GitHub 网站上）

1. 注册/登录 GitHub: https://github.com/
2. 右上角 `+` → **New repository**
3. Repository name：`basiccoffee`（必须跟 Pages 项目名一致，下面会用到）
4. 选择 **Public**（公开）
5. **不要勾选** "Add a README file"、"Add .gitignore"、"Choose a license"
6. 点击 **Create repository**
7. 记住 GitHub 给你的一行命令，类似：
   ```
   git remote add origin https://github.com/你的用户名/basiccoffee.git
   git branch -M main
   git push -u origin main
   ```

### C. 在本地推送

打开项目目录（Windows 命令提示符或 PowerShell）：

```bash
cd "C:\Users\WPXSP\WorkBuddy\2026-09-08-10-29-34\coffee-demo"
git init
git config user.name "你的名字"
git config user.email "你的邮箱"
git add .
git commit -m "init: 咖啡科普小站"
git remote add origin https://github.com/你的用户名/basiccoffee.git
git branch -M main
git push -u origin main
```

> 💡 **如果 GitHub 没装**：先安装 [Git for Windows](https://git-scm.com/download/win)

---

## 步骤 4️⃣ — Cloudflare Pages 连接 GitHub

1. 登录 Cloudflare Dashboard: https://dash.cloudflare.com/
2. 左侧菜单 **Workers & Pages** → **Create** → **Pages** → **Connect to Git**
3. 选择 **GitHub** → 授权 Cloudflare 访问你的 GitHub 账号
4. 选 **`basiccoffee`** 仓库 → **Begin setup**
5. 配置 Build：
   - **Project name**：`basiccoffee`（这会决定你的 Pages 默认域名 `basiccoffee.pages.dev`）
   - **Framework preset**：选 **None**
   - **Build command**：**留空**
   - **Build output directory**：填 `/`
6. 点击 **Save and Deploy**
7. 等 1-3 分钟，看到 `Success` 后会拿到一个临时链接 `basiccoffee.pages.dev` 可以先预览

---

## 步骤 5️⃣ — 绑定自定义域名 basiccoffee.cn

1. 在 Cloudflare Pages 项目页，点 **Custom domains** 标签
2. **Set up a custom domain** → 输入 `basiccoffee.cn` → 点 **Continue**
3. Cloudflare 提示你：需要在 DNS 提供商处添加一条 CNAME 记录
4. **复制 Cloudflare 显示的目标地址**，一般是 `basiccoffee.pages.dev`（或类似 `xxx.basiccoffee.pages.dev`）
5. **同样的步骤**再添加 `www.basiccoffee.cn`

---

## 步骤 6️⃣ — 在 阿里云 DNS 添加 CNAME 记录

1. 回到 https://dns.console.aliyun.com/
2. 找到 `basiccoffee.cn` → 点 **解析设置**
3. 添加两条 CNAME 记录：

| 主机记录 | 记录类型 | 记录值 |
|---|---|---|
| `www` | CNAME | `basiccoffee.pages.dev` |
| `@`（根域名） | CNAME | `basiccoffee.pages.dev` |

> ⚠️ **关于根域名（@）**：阿里云 DNS 支持根域 CNAME（隐性 URL 转发），如果不允许，可以用 **URL 转发** 把 `basiccoffee.cn` 301 跳转到 `www.basiccoffee.cn`。
> 
> 如果提示"无法添加 @ 的 CNAME"，改用 **URL 转发记录** 实现跳转，目的地填 `https://www.basiccoffee.cn`

4. 添加完毕后回到 Cloudflare Pages → **Custom domains**，Cloudflare 会**自动验证**（5–30 分钟）
5. 验证通过后，HTTPS 自动签发，可以在浏览器看到 🔒 标志

---

## ✅ 完成验证

打开浏览器，输入 `https://basiccoffee.cn`（或 `https://www.basiccoffee.cn`），应该看到咖啡科普小站首页。

---

## 🔄 后续：怎么改内容？

```bash
# 1. 改 HTML / CSS（任何编辑器）
# 2. 在项目目录下：
git add .
git commit -m "描述你的改动"
git push origin main
# 3. Cloudflare Pages 自动重新部署（30 秒 - 2 分钟）
# 4. 域名自动指向最新版本，无需任何额外操作
```

---

## 🆘 卡在哪一步？

随时把报错截图发给我，我帮你看。最常遇到的问题：
- **Git push 失败**：检查是否用了 SSH key / PAT 登录
- **Cloudflare 验证不到域名**：检查 DNS 记录是否加对了，特别是 `@` 用 CNAME 还是 URL 转发
- **HTTPS 没生效**：通常等 10-15 分钟即可

---

**祝你部署顺利！🚀**
