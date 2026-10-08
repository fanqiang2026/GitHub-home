# GitHub 个人主页 README 完整指南

> 如何让 GitHub 个人主页直接展示自己的项目、个人介绍以及项目详细内容。

GitHub 支持通过 **Profile README** 自定义个人主页。

配置完成后，访问：

https://github.com/你的用户名
比如我的是https://github.com/fanqiang2026

即可在个人主页顶部看到自己编写的 README 内容。

---

## 📖 一、什么是 GitHub Profile README？

GitHub 个人主页通常会展示：

- 个人简介
- Pinned Repositories（置顶仓库）
- Popular repositories（热门仓库）
- Contributions（贡献记录）
- Activity（活动记录）

除此之外，GitHub 还支持在个人主页顶部显示一个自定义的 README。

这个 README 被称为：

> **Profile README**

它本质上是一个特殊仓库中的 `README.md` 文件。

---

# 🚀 二、创建 Profile README

假设你的 GitHub 用户名是：

zhangsan


那么需要创建一个：

zhangsan/zhangsan


仓库。

也就是说：

GitHub 用户名 │ ↓ 仓库名称必须与用户名完全一致 │ ↓ username/username


例如：

github.com/zhangsan ↓ github.com/zhangsan/zhangsan


---

## 1. 创建仓库

登录 GitHub 后，进入：

New repository


创建一个新仓库。

填写：

Repository name:

zhangsan


如果你的用户名是 `zhangsan`，那么仓库名称就必须是：

zhangsan


最终仓库：

zhangsan/zhangsan


---

## 2. 初始化 README

创建仓库时建议勾选：

☑ Add a README file


然后点击：

Create repository


创建完成后，仓库中会出现：

README.md


这个 README 就是你的 Profile README。

---

# 📝 三、编写个人主页 README

打开：

README.md


点击编辑，然后填写自己的内容。

例如：

Hi, I'm Zhang San 👋
欢迎来到我的 GitHub 主页！

🚀 About Me
我是一名软件开发者，主要关注：

Web Development
Artificial Intelligence
Open Source
Backend Development
🛠️ 技术栈
Python
JavaScript
TypeScript
React
Node.js
Docker
🚀 我的项目
My Project
这是我的主要开源项目。

项目地址：

https://github.com/zhangsan/my-project

📫 联系我
欢迎通过 GitHub 与我交流！


保存后访问：

https://github.com/zhangsan


即可看到：

Hi, I'm Zhang San 👋

About Me ...

技术栈 ...

我的项目 ...

联系方式 ...


---

# 🎯 四、如果想让首页直接显示某个项目的详细内容

这是很多人容易混淆的地方。

假设你已经有一个项目：

zhangsan/my-project


这个项目里面有：

my-project/ ├── README.md ├── src/ ├── docs/ ├── package.json └── ...


其中：

my-project/README.md


已经写好了完整的项目介绍。

你可能希望：

访问：

https://github.com/zhangsan


直接显示：

my-project/README.md


---

## ⚠️ GitHub 的限制

GitHub **不能直接指定某个普通仓库的 README 作为个人主页 README**。

个人主页使用的是：

zhangsan/zhangsan/README.md


而不是：

zhangsan/my-project/README.md


所以：

❌ username/my-project/README.md


不能直接作为个人主页 README。

需要通过下面的方式实现。

---

# ✅ 五、方案一：复制项目 README

这是最简单的方法。

项目：

zhangsan/my-project


其中：

my-project/README.md


将需要展示的内容复制到：

zhangsan/zhangsan/README.md


结构变成：

GitHub │ ├── zhangsan │ └── README.md │ └── my-project └── README.md


个人主页：

https://github.com/zhangsan


显示：

zhangsan/zhangsan/README.md


项目主页：

https://github.com/zhangsan/my-project


显示：

zhangsan/my-project/README.md


---

# 🔄 六、方案二：自动同步 README

如果项目 README 经常修改，手动复制会比较麻烦。

可以使用：

> **GitHub Actions**

实现自动同步。

例如：

my-project │ └── README.md │ │ GitHub Actions ↓ zhangsan/zhangsan │ └── README.md


实现：

修改项目 README ↓ git push ↓ GitHub Actions 自动运行 ↓ 复制 README ↓ 更新 Profile README


这样就不需要每次手动复制。

---

# ⚙️ 七、使用 GitHub Actions 自动同步

假设有两个仓库：

zhangsan/my-project


以及：

zhangsan/zhangsan


希望：

my-project/README.md


自动同步到：

zhangsan/README.md


可以在项目仓库创建：

.github/workflows/sync-profile-readme.yml


示例：

name: Sync Profile README

on: push: branches: - main

jobs: sync: runs-on: ubuntu-latest

steps: - name: Checkout project uses: actions/checkout@v4

- name: Update profile README env: GHTOKEN: 
s
e
c
r
e
t
s
.
P
R
O
F
I
L
E
T
O
K
E
N
r
u
n
:
∣
g
i
t
c
l
o
n
e
h
t
t
p
s
:
/
/
x
−
a
c
c
e
s
s
−
t
o
k
e
n
:
{GHTOKEN}@github.com/zhangsan/zhangsan.git profile

cp README.md profile/README.md

cd profile

git config user.name "github-actions[bot]" git config user.email "41898282+github-actions[bot]@users.noreply.github.com"

git add README.md

if git diff --cached --quiet; then echo "README 没有变化" else git commit -m "chore: sync README" git push fi


需要将：

zhangsan


替换成你自己的 GitHub 用户名。

---

# 🔐 八、GitHub Token 配置

如果 GitHub Actions 需要修改另一个仓库，通常需要配置权限。

可以在 GitHub 中创建一个用于访问目标仓库的 Token。

然后进入：

my-project ↓ Settings ↓ Secrets and variables ↓ Actions


添加：

PROFILE_TOKEN


值填写你的 Token。

之后 Workflow 中通过：

${{ secrets.PROFILE_TOKEN }}


使用。

> ⚠️ Token 属于敏感信息，不要直接写进 README、代码或 Workflow 文件中。

---

# 📌 九、Profile README 和置顶仓库有什么区别？

这两个功能非常容易混淆。

## Pinned Repository

Pinned Repository 是：

> **置顶仓库**

它主要用于突出显示某个项目。

例如：

Popular repositories

┌─────────────────────┐ │ my-project │ │ ⭐ 123 │ │ Python │ └─────────────────────┘


通常适合：

- 展示自己的开源项目
- 展示代表作品
- 展示热门项目
- 展示项目列表

---

## Profile README

Profile README 是：

> **个人主页顶部的自定义内容**

例如：

Hi, I'm Zhang San 👋
About Me
我是一个软件开发者。

🚀 Projects
My Project
这是我的主要项目……

🛠️ Skills
Python · React · Docker


它适合：

- 个人介绍
- 项目介绍
- 技术栈
- 联系方式
- 博客地址
- 项目链接
- GitHub Stats
- 开源项目推广

---

# 🏆 十、推荐的个人主页结构

如果你的主要目的，是通过 GitHub 主页展示自己的项目，可以使用下面这种结构：

👋 Hi, I'm Your Name
Developer / Open Source Contributor / ...
🚀 About Me
简单介绍自己。

例如：

我是一名专注于 Web Development 和 AI 的开发者， 喜欢构建开源项目和研究新技术。

⭐ Featured Project
🚀 My Project
一句话介绍项目。
这是我的主要开源项目，用于解决 XXX 问题。

✨ Features
Feature 1
Feature 2
Feature 3
Feature 4
🛠️ Tech Stack
Python
TypeScript
React
Node.js
Docker
📦 Installation
git clone https://github.com/username/my-project.git

cd my-project

npm install

npm run dev
🔗 Project
https://github.com/username/my-project

📚 Other Projects
Project A
Project B
Project C
🛠️ Skills
Category	Technologies
Languages	Python, JavaScript, TypeScript
Frontend	React, Vue
Backend	Node.js, FastAPI
Database	PostgreSQL, MySQL
DevOps	Docker, GitHub Actions
📫 Contact
GitHub: https://github.com/username
Website: https://example.com
Email: example@example.com

---

# 🖼️ 十一、还可以加入图片

Profile README 支持 Markdown 图片。

例如：

!Profile


也可以使用在线图片：

!Banner


还可以制作一个 GitHub Profile Banner：

┌───────────────────────────────────────────┐ │ │ │ Hi, I'm Zhang San 👋 │ │ │ │ Developer · Open Source · AI │ │ │ └───────────────────────────────────────────┘


这样可以让个人主页更加直观。

---

# 📊 十二、添加 GitHub Stats

也可以在 Profile README 中加入 GitHub 数据统计。

例如：

!GitHub Stats


将：

zhangsan


替换成自己的 GitHub 用户名。

还可以展示：

- Stars
- Contributions
- Commits
- Languages
- Followers
- Repositories

---

# 📂 十三、推荐的 Profile README 结构

一个比较完整的个人主页可以设计成：

Profile README │ ├── 👋 Introduction │ ├── 🚀 Featured Project │ ├── Project Description │ ├── Features │ ├── Screenshot │ ├── Tech Stack │ └── Project Link │ ├── 📚 Other Projects │ ├── 🛠️ Skills │ ├── 📊 GitHub Stats │ ├── 📝 Blog / Website │ └── 📫 Contact


推荐顺序：

个人介绍 ↓ 核心项目 ↓ 项目详细介绍 ↓ 其他项目 ↓ 技术栈 ↓ GitHub 数据 ↓ 联系方式


---

# 🔍 十四、常见问题

## Q1：为什么我的 README 没有显示？

首先检查：

GitHub 用户名 = 仓库名称


例如用户名：

zhangsan


那么仓库必须是：

zhangsan/zhangsan


而不是：

zhangsan/github-profile


---

## Q2：仓库需要设置为 Public 吗？

建议使用：

Public


这样个人主页上的 Profile README 才能正常公开展示。

---

## Q3：必须叫 README.md 吗？

是的。

Profile README 使用：

README.md


作为个人主页内容。

---

## Q4：可以放 HTML 吗？

可以。

GitHub Markdown 支持一定程度的 HTML。

例如：

<h1 align="center"> Hi 👋, I'm Zhang San </h1>

<p align="center"> Developer · Open Source · AI </p>


不过建议不要过度使用 HTML，以保证兼容性和可维护性。

---

## Q5：可以放 GIF 吗？

可以。

例如：

!Coding


也可以使用：

<img src="https://example.com/coding.gif" width="400">


---

# 🎯 十五、最佳实践

如果你的目的是：

> **通过 GitHub 个人主页重点展示一个项目**

推荐采用：

GitHub Profile │ ├── 简短个人介绍 │ ├── ⭐ Featured Project │ │ ├── 项目 Logo │ ├── 项目简介 │ ├── 核心功能 │ ├── 截图 │ ├── 技术栈 │ └── 项目链接 │ ├── 🛠️ 技术栈 │ ├── 📊 GitHub Stats │ └── 📫 联系方式


不要把整个项目 README 原封不动复制过去。

更推荐：

> **项目仓库 README 负责完整的技术文档，Profile README 负责项目宣传和导航。**

例如：

GitHub 个人主页 │ ├── 项目简介 ├── 项目亮点 ├── 项目截图 └── 项目链接 │ ↓ GitHub 项目仓库 │ ├── 完整 README ├── Installation ├── Configuration ├── Usage ├── API └── Documentation


这样结构更加清晰。

---

# ✅ 十六、最终效果

最终你可以拥有：

https://github.com/username


打开之后：

┌───────────────────────────────────────────┐ │ │ │ 👋 Hi, I'm Your Name │ │ │ │ Developer · Open Source · AI │ │ │ ├───────────────────────────────────────────┤ │ │ │ 🚀 Featured Project │ │ │ │ My Project │ │ │ │ 项目介绍…… │ │ │ │ ⭐ Features │ │ • Feature 1 │ │ • Feature 2 │ │ • Feature 3 │ │ │ │ 🛠️ Python · React · Docker │ │ │ │ 🔗 View Project │ │ │ ├───────────────────────────────────────────┤ │ │ │ 🛠️ Skills │ │ │ │ 📊 GitHub Stats │ │ │ │ 📫 Contact │ │ │ └───────────────────────────────────────────┘


---

# 📌 总结

如果想让 GitHub 个人主页直接显示自己的内容，核心步骤只有三个：

### 1. 创建同名仓库

username/username


### 2. 创建 README.md

username/username/README.md


### 3. 访问个人主页

https://github.com/username


GitHub 会自动将：

username/username/README.md


显示在个人主页顶部。

---

## ⭐ 最重要的一点

**GitHub 不能直接把某个普通仓库的 README 设置成个人主页 README。**

如果你的项目是：

username/my-project


而个人主页是：

username


那么个人主页使用的是：

username/username/README.md


而不是：

username/my-project/README.md


如果希望两者内容保持一致，可以：

1. 手动复制 README
2. 使用 GitHub Actions 自动同步
3. 更推荐：Profile README 做项目介绍，项目仓库 README 做完整技术文档

这样既能让 GitHub 首页看起来简洁专业，也能让项目仓库保持完整、独立的文档结构。
