## 完整操作步骤：把3个子仓库加入你的母仓库（Submodule 方式）
先明确你的仓库对应关系：

| 角色 | 仓库地址 | 本地存放路径（建议） | 对应功能 |
|------|----------|----------------------|----------|
| 母仓库 | `https://github.com/CN-Vik/PWM-Technology-Application.git` | 根目录 | 统一管理3个子工程 |
| 子仓库1 | `https://github.com/CN-Vik/PWM-protocol-data-driven.git` | `projects/pwm_tx_driven` | PWM发送数据工程 |
| 子仓库2 | `https://github.com/CN-Vik/Complementary-PWM.git` | `projects/comp_pwm` | 互补PWM工程 |
| 子仓库3 | `https://github.com/CN-Vik/PWM-protocol-data-processing.git` | `projects/pwm_capture` | PWM输入捕获处理工程 |

---

### 步骤1：克隆母仓库到本地
先在本地选一个空文件夹，执行克隆并进入母仓库目录：
```bash
# 克隆你的母仓库
git clone https://github.com/CN-Vik/PWM-Technology-Application.git

# 进入母仓库根目录
cd PWM-Technology-Application
```

### 步骤2：逐个添加3个子模块
在母仓库根目录下，依次执行下面3条命令，把3个子仓库以子模块形式添加进来。
`git submodule add <子仓库地址> <本地存放路径>` 的作用是：自动克隆子仓库到指定路径，同时在母仓库中记录子仓库的地址和版本指针。

```bash
# 添加子仓库1：PWM协议发送驱动
git submodule add https://github.com/CN-Vik/PWM-protocol-data-driven.git projects/pwm_tx_driven

# 添加子仓库2：互补PWM工程
git submodule add https://github.com/CN-Vik/Complementary-PWM.git projects/comp_pwm

# 添加子仓库3：PWM协议捕获处理
git submodule add https://github.com/CN-Vik/PWM-protocol-data-processing.git projects/pwm_capture
```

执行完成后，你会发现两个变化：
1. 根目录自动生成 `.gitmodules` 文件，里面记录了所有子仓库的地址和对应路径；
2. `projects/` 目录下出现三个工程文件夹，里面就是对应子仓库的完整代码。

### 步骤3：提交母仓库的变更并推送到远程
子模块添加完成后，母仓库产生了新的变更（.gitmodules + 三个子模块指针），需要提交并推送到 GitHub 的母仓库：
```bash
# 查看变更，确认有 .gitmodules 和三个 projects 下的目录
git status

# 添加所有变更
git add .gitmodules projects/

# 提交变更
git commit -m "feat: 添加3个PWM子工程作为submodule"

# 推送到远程母仓库
git push origin main
```
> 注意：如果你的默认分支叫 `master`，就把 `main` 换成 `master`。

到这里，**GitHub 上的母仓库就已经关联好 3 个子仓库了**。打开你的母仓库主页，会看到三个带箭头的子模块链接，点击可以直接跳转到对应的独立子仓库。

---

### 步骤4：验证：全新克隆母仓库 + 拉取所有子模块代码
如果换一台电脑、或者别人拿到你的母仓库，直接 `git clone` 母仓库默认是不会拉取子仓库代码的，需要额外执行子模块初始化命令，这也是 submodule 的核心特点：
```bash
# 1. 全新克隆母仓库
git clone https://github.com/CN-Vik/PWM-Technology-Application.git
cd PWM-Technology-Application

# 2. 初始化并拉取所有子仓库的完整代码
git submodule update --init --recursive
```
执行完第二条命令后，三个工程文件夹里才会出现完整的代码文件。

---

## 日常开发常用操作（学习重点）
### 1. 修改某个子工程的代码并提交
子仓库本身是完全独立的 Git 仓库，正常进入目录写代码、提交、推送即可：
```bash
# 进入子工程目录
cd projects/pwm_tx_driven

# 正常修改代码、提交
git add .
git commit -m "fix: 修正PWM周期配置"
git push origin main

# 回到母仓库根目录
cd ../../
```
子仓库 commit 更新后，母仓库里记录的还是旧的版本指针，需要同步更新母仓库：
```bash
# 母仓库根目录执行
git add projects/pwm_tx_driven
git commit -m "update: 同步pwm_tx_driven子工程到最新版本"
git push origin main
```

### 2. 一键同步所有子仓库到远程最新版本
如果其他端更新了子仓库，本地母仓库想把所有子工程都拉到最新：
```bash
git submodule update --remote
```

---

## 关键原理（帮你理解 Submodule）
1. 母仓库**不存储子仓库的源码**，只存储「子仓库地址 + 对应 commit 哈希值」，本质是一个“索引目录”；
2. 三个子仓库完全独立，各自的提交历史、分支、远程地址都互不干扰，也可以单独被克隆使用；
3. `.gitmodules` 是子模块的配置文件，属于母仓库的一部分，跟随母仓库一起版本管理。

---

## 常见避坑点
1. **不要在母仓库里直接修改子工程文件却不提交子仓库**：会导致母仓库指针与子仓库实际代码脱节，其他人拉取后会出现异常；
2. **提交母仓库前，先提交子仓库的变更**：永远先把子仓库的代码 push 到远程，再更新母仓库的版本指针；
3. **忽略文件独立维护**：每个子仓库自己维护自己的 `.gitignore`（屏蔽 Keil 编译产物、缓存等），母仓库的 `.gitignore` 只管好根目录内容。

需要我再补充删除子模块、切换子模块分支这类进阶操作吗？