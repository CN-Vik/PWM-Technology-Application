# PWM-Technology-Application
## PWM技术应用
+ PWM驱动自定义协议的RGB灯(projects\pwm_tx_driven)
+ 基于PWM信号协议数据接收处理(projects\pwm_capture)
+ 互补死区PWM(projects\comp_pwm)

-----------

# Attention: 本仓库用git repo管理！


## 克隆母仓库 + 拉取所有子模块代码
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


## 一键同步所有子仓库到远程最新版本
如果其他端更新了子仓库，本地母仓库想把所有子工程都拉到最新：
```bash
git submodule update --remote
```
