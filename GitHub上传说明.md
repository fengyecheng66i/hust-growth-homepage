# GitHub 上传操作说明

本地仓库已经建好了，首次提交也做完了。剩下就是把它推到 GitHub。

---

## 第一步：在 GitHub 上建一个空仓库

1. 登录 GitHub，点右上角 `+` → **New repository**
2. 填写：
   - **Repository name**：建议 `hust-growth-homepage`（小写、用连字符，别写中文）
   - **Description**：可选，填 `华中科技大学软件工程 · 个人成长主页网页作业`
   - **Public / Private**：选 **Public**。作业展示、简历放链接都需要公开；如果只想自己看，选 Private 也行，但 Pages 免费版不支持私有仓库
   - **不要勾选** `Add a README file`、`Add .gitignore`、`Choose a license`
     —— 这三个是新手最常踩的坑。一旦远程仓库里有文件，你直接 `push` 会被拒绝。
3. 点 **Create repository**
4. 创建完的页面会显示一段 `…or push an existing repository from the command line`，把其中 `https://github.com/你的用户名/hust-growth-homepage.git` 这个地址复制下来

---

## 第二步：关联远程仓库并推送

打开 Git Bash，进入目录：

```bash
cd "C:/Users/枫叶城ii/Desktop/提交材料"
```

`cd` 这一行如果是手动敲的，最省事的办法是：在文件资源管理器里打开 `提交材料` 文件夹，在地址栏输入 `bash` 回车，会直接开在这个目录。

然后：

```bash
# 把 GitHub 上那个空仓库设为本地的远程源
git remote add origin https://github.com/你的用户名/hust-growth-homepage.git

# 确认分支名是 main
git branch -M main

# 推送，并把 main 设为默认跟踪分支
git push -u origin main
```

**第一次 `push` 会弹出登录窗口。**

- 弹出的是浏览器窗口 → 点授权即可（GitHub 现在推荐这种方式，叫 Git Credential Manager，装 Git for Windows 时自带）
- 弹出的是命令行输入 `Username` / `Password` → **密码那里不能填你的 GitHub 登录密码**，GitHub 早就禁用了密码推送。必须用 **Personal Access Token**：
  1. GitHub 右上角头像 → **Settings** → 左侧拉到底 **Developer settings** → **Personal access tokens** → **Tokens (classic)**
  2. **Generate new token (classic)**
  3. Note 随便填（如 `作业推送`），Expiration 选 30 天
  4. 勾选 **`repo`** 这一个就够了（会自动带上下面的子项）
  5. 生成后**立刻复制**那一串 `ghp_xxxx`，页面刷新后就再也看不到了
  6. 回到命令行，`Username` 填你的 GitHub 用户名，`Password` 处粘贴这串 token（粘贴时屏幕不显示任何字符，是正常的，粘完直接回车）

推送成功后，刷新 GitHub 页面就能看到文件了。

---

## 第三步（可选）：开启 GitHub Pages，把网页变成在线链接

如果想把这份作业变成一个可以发给别人看的网址：

1. 进入仓库页面 → **Settings** → 左侧 **Pages**
2. **Source** 选 `Deploy from a branch`
3. **Branch** 选 `main`，目录选 `/ (root)`，点 **Save**
4. 等 1～3 分钟，页面上方会出现你的网址，形如：
   `https://你的用户名.github.io/hust-growth-homepage/`
5. 但首页文件要叫 `index.html` 才会被自动打开。我们这个仓库根目录没有 `index.html`，有三个办法：
   - **办法 A（最简单）**：Pages 的 Branch 目录里没法定到子文件夹以外的东西，所以改用「多页版」——把 `多页版/` 里的四个文件**复制到仓库根目录**，再推一次
   - **办法 B**：直接访问完整路径
     `https://你的用户名.github.io/hust-growth-homepage/2-网页作业-个人成长主页.html`
     （中文文件名在网址里会被编码成一长串 `%E2%80%A6`，能打开但很难看）
   - **办法 C**：把那个单文件重命名为 `index.html`
   - **我建议办法 C**：`git mv "2-网页作业-个人成长主页.html" index.html`，这样网址最干净，单文件版也能直接在线上跑

---

## 以后每次改完东西，怎么更新

```bash
cd "C:/Users/枫叶城ii/Desktop/提交材料"
git status                          # 先看看改了哪些文件
git add -A                          # 把改动加入暂存区
git commit -m "改了什么，一句话说明"   # 提交
git push                            # 推上去
```

`git status` 这一步别省。它会告诉你到底发生了什么变化，避免 `.gitignore` 没生效时把不该传的文件（比如备份、大 zip）传上去。

---

## 常见报错对照

| 报错 | 原因 | 怎么修 |
|---|---|---|
| `remote: Support for password authentication was removed` | 密码输了 GitHub 登录密码 | 改用上文的 Personal Access Token |
| `Updates were rejected because the remote contains work that you do not have locally` | 建仓库时勾选了 README，远程已有内容 | `git pull --rebase origin main` 后再 `git push`；或删掉远程仓库重建，别勾 README |
| `remote: Permission to xxx denied to yyy` | 本机缓存的旧账号凭据 | Windows 搜索「凭据管理器」→ 找到 `git:https://github.com` → 删除 → 重新 push |
| `fatal: remote origin already exists` | 已经加过远程源 | `git remote set-url origin 新地址` 覆盖，或 `git remote remove origin` 再重加 |
| `fatal: not a git repository` | `cd` 的目录不对 | 确认在 `C:/Users/枫叶城ii/Desktop/提交材料` 里 |

---

## 关于提交身份

本地仓库里我配的是：

- `user.name` = `chenruochen`
- `user.email` = `chenruochen@users.noreply.github.com`

第二条是**占位地址**。GitHub 的 noreply 邮箱格式固定是：

```
数字ID+用户名@users.noreply.github.com
```

去 GitHub → **Settings** → **Emails** → 勾上 `Keep my email addresses private`，页面上会直接显示你专属的那一串。拿到后改一下：

```bash
cd "C:/Users/枫叶城ii/Desktop/提交材料"
git config user.email "你的数字ID+你的用户名@users.noreply.github.com"
git commit --amend --reset-author --no-edit
```

这样提交记录里就不会出现你的真实邮箱。如果这是你自己的私人仓库、不介意暴露，直接把 `user.email` 改成真实邮箱也可以，但 `--amend` 那一步就不需要了。

> 注意：`--amend` 只对**还没推上去**的提交有效。已经 push 过就不要再 amend 了，否则本地和远程历史会分叉，还得 force push。

---

## 隐私检查清单

推之前扫一眼，这个仓库是要公开的：

- [ ] 网页里没有你的真实手机号、QQ 号、身份证号
- [ ] 没有 `.env`、密钥、token 文件（本仓库没有）
- [ ] `3-作业中-完整项目.zip` 里的内容确认过没有无关的私人文件
- [ ] 页面「联系我」里目前是「暂未公开，可经班级群找到我」——这是安全的占位，等你想公开邮箱时再改
