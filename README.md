# Starbase

Star 系列工程的 GitHub 合集，使用 Git 子模块收录独立仓库并固定源码版本。

| 项目 | 名称 | 仓库 | 说明 |
|---|---|---|---|
| 昴宿底盘 | **StarPleiades** | [star-pleiades](https://github.com/xcjk-tofuture/star-pleiades) | STM32 / TM4C 板载固件与 ROS2 配套 |
| 星璇无人机 | **StarFlight** | [star-flight](https://github.com/xcjk-tofuture/star-flight) | 传感器、姿态解算与飞行控制 |
| 北河 AR 眼镜 | **Pollux AR** | [pollux-ar](https://github.com/xcjk-tofuture/pollux-ar) | Hi3861 AR 历史工程存档 |

名称预留：**StarStudio**（统一上位机，`star-studio`）、**StarBus**（公共串口协议，`star-bus`）。两者尚未建立独立仓库；现有工具和协议继续位于固件工程内。

## 获取全部工程

```sh
git clone --recurse-submodules https://github.com/xcjk-tofuture/starbase.git
cd starbase
git submodule status
```

已克隆但尚未获取子模块时：

```sh
git submodule update --init --recursive
```

```text
starbase/
├─ star-pleiades/   StarPleiades STM32 master 基线；包含 ros/ws_starbot
├─ star-flight/     StarFlight master 基线
└─ pollux-ar/       Pollux AR master 存档
```

合集提交固定每个子模块的 SHA。`git submodule update` 恢复这些固定版本；它不会自动跟随固件 `dev` 功能。子模块默认处于 detached HEAD，开发前应切换对应分支或创建独立 worktree。

## 分支用途

| 仓库 | 分支 | 用途 |
|---|---|---|
| StarPleiades | `master` | STM32 基线 |
| StarPleiades | `dev` | STM32 功能开发，验收后合回 master |
| StarPleiades | `board/tm4c` | TM4C 基础适配，不跟随后续功能 |
| StarFlight | `master` / `dev` | 无人机基线 / 功能开发 |
| Pollux AR | `master` | 存档；保留原海思 SDK、GN/hb 环境 |

单独读取 TM4C，可从合集目录运行：

```sh
git -C star-pleiades fetch origin board/tm4c:refs/remotes/origin/board/tm4c
git -C star-pleiades worktree add ../star-pleiades-tm4c origin/board/tm4c
```

构建、接口和待办分别见子仓库的 README 与 docs。StarPleiades / StarFlight 使用 CMake、ARM GCC、OpenOCD；STM32 为 ST-Link，TM4C 为板载 ICDI。Pollux AR 保留原构建环境。

## 更新合集版本

先在子仓库完成相应分支的提交和验证，再在合集里检出选定提交、检查子模块状态，提交更新后的子模块 SHA。合集不直接修改子仓库源码，也不替代子项目的发布和硬件验收。
