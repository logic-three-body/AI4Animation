# AI4Animation (Unity) 环境配置指南

> 本文档涵盖 AI4Animation 主仓库各独立案例（SIGGRAPH 2017-2024 等）在 Unity 和 Python 端的完整环境隔离配置方案。

---

## 一、前置环境检测

在开始配置 AI4Animation 的 Unity 运行环境前，请检查您的基础环境：

### 1.1 系统及硬件
- **操作系统**: Windows 10/11 64位（包含桌面 GUI 支持）。
- **显卡需求**: NVIDIA RTX 系列显卡，显存推荐 >= 8GB（用以支持实时推理和高帧率渲染）。
- **驱动程序**: NVIDIA 驱动版本 >= 525 且需配置好可用的 CUDA（如果你需要重训部分案例使用 GPU）。

### 1.2 必备软件
- **Git LFS**: 本仓库包含大量的源动捕文件、材质和模型资源。确保安装了 [Git LFS](https://git-lfs.com/)。
- **Conda**: (可选，但推荐) 若需运行案例附带的 Python 深度学习模块，必须具备较新的 conda 工具（Anaconda/Miniconda）。
- **Unity Hub**: 必须安装。请在 [Unity官网](https://unity.com/download) 下载并安装 Unity Hub。

---

## 二、Unity 环境配置及授权登录

AI4Animation 分散在多个学术会议的项目目录下，当前子模块内项目实际分布在以下 4 个 Unity 版本：

| Unity 版本 | 对应项目 |
|---|---|
| `2018.3.0f2` | `SIGGRAPH_2017`、`SIGGRAPH_2018`、`SIGGRAPH_Asia_2019` |
| `2019.3.0f3` | `SIGGRAPH_2020` |
| `2021.3.8f1` | `SIGGRAPH_2022` |
| `2022.3.11f1` | `SIGGRAPH_2024` |

### 2.1 浏览器辅助下载 Unity

如果您本地未安装所需的精确版本（版本不匹配极易导致脚本和包报错），请按版本逐个安装：

1. 确保 **Unity Hub** 正在运行。
2. 前往 [Unity Download Archive](https://unity3d.com/get-unity/download/archive)，依次安装：
   - `2018.3.0f2`
   - `2019.3.0f3`
   - `2021.3.8f1`
   - `2022.3.11f1`
3. 若仅先验证最新项目，可先安装 `2022.3.11f1`（`SIGGRAPH_2024`）。
4. 安装模块中，如果不打包发布，只需要勾选默认模块即可（无需额外安装 Android, iOS 支持），建议勾选 Visual Studio 方便脚本调试。

### 2.2 扫描二维码或授权登录 Unity ID

如果您是首次使用 Unity Hub，需要有效的 Unity 账号。
- 如果本地网络/授权不便，**可以由协作方/管理员提供二维码扫码登录协助**（请联系协作人或通过手机版 Unity Connect 扫码辅助认证）。

---

## 三、各案例项目 (Unity Env) 隔离启动

为了避免资源包互相冲突、避免全局覆盖配置带来麻烦，每个包含在不同文件夹下的 SIGGRAPH 案例都是**独立隔离的 Unity 项目**。您需要将它们作为单独的工程添加进 Unity Hub。

### 隔离工程列表和启动方式：

| 案例 | 功能介绍 | Unity 启动路径 (通过 Hub "Add project" 加入) |
|---|---|---|
| **SIGGRAPH_2024** | 具身控制器和特征匹配 | `AI4Animation/AI4Animation/SIGGRAPH_2024/Unity` |
| **SIGGRAPH_2022** | 周期性自动编码器和动作查询 | `AI4Animation/AI4Animation/SIGGRAPH_2022/Unity` |
| **SIGGRAPH_2020** | 篮球及接触感知的多阶段运动 | `AI4Animation/AI4Animation/SIGGRAPH_2020/Unity` |
| **SIGGRAPH_Asia_2019** | 物体交互 (箱子/椅子/门等) | `AI4Animation/AI4Animation/SIGGRAPH_Asia_2019/Unity` |
| **SIGGRAPH_2018** | 四足动物运动模式适应 | `AI4Animation/AI4Animation/SIGGRAPH_2018/Unity` |
| **SIGGRAPH_2017** | 基于相位的实时地形自适应运动 | `AI4Animation/AI4Animation/SIGGRAPH_2017/Unity` |

### 对应版本矩阵

| 项目 | 需要的 Unity 版本 |
|---|---|
| `SIGGRAPH_2017` | `2018.3.0f2` |
| `SIGGRAPH_2018` | `2018.3.0f2` |
| `SIGGRAPH_Asia_2019` | `2018.3.0f2` |
| `SIGGRAPH_2020` | `2019.3.0f3` |
| `SIGGRAPH_2022` | `2021.3.8f1` |
| `SIGGRAPH_2024` | `2022.3.11f1` |

**启动动作**：
1. 打开 Unity Hub -> Projects -> 点击 `Add` 或 `Open`
2. 逐一导航并选择上方给出的路径。
3. 如果弹出“升级版本”或者“Safe Mode”提示，请确保您安装的 Unity 版本精确匹配该项目目录中 `ProjectSettings/ProjectVersion.txt` 声明的版本，切勿随意点击“Upgrade”。

---

## 四、Python 训练环境配置（进阶）

如果仅需运行包含的内置 Demo 动画，配置完 Unity 即可完成部署。如果需要使用仓库中自带的数据进行从头训练或者导出特征（深度学习脚本），它们分别存放在对应子级目录如 `AI4Animation/SIGGRAPH_2020/DeepLearning`。

同理，**每个研究项目使用的 Python 环境及库版本差别极大**。

> **我们强烈建议为每个需要的实验单独配置 conda env 隔离。**

### 配置流程示例

以 SIGGRAPH_2020 项目为例进行训练（各子项目环境要求不同，详细请参考对应子项目的 README）：

```powershell
# 在 ai4animationpy 根目录执行，按路径创建独立环境
powershell -NoProfile -ExecutionPolicy Bypass -File .\scripts\create_ai4animation_2020_env.ps1 -EnvBase D:\AnimationTech\.envs -Verify
```

> **提示:** 如果想尝试无 Unity 依赖的纯 Python/Pytorch 最新实现方式，请移步参考根项目外层的 `ai4animationpy` 环境。

### FBX Python Bindings（可选）

如果需要运行 `ai4animationpy` 的 `FBXLoading` demo 或 Python 侧 `Motion.LoadFromFBX(...)`，还需要在对应环境中额外安装 Autodesk FBX SDK Python bindings。该依赖不属于 Unity 项目的默认安装内容。

固定安装前提：
- Autodesk FBX SDK
- Autodesk FBX SDK Python Bindings
- `sip==6.6.2`
- `FBXSDK_ROOT`
- `FBXSDK_COMPILER=vs2022`

推荐在 `D:\AnimationTech\.envs\ai4anim-fbxloading` 中执行安装和验证：

```powershell
conda activate D:\AnimationTech\.envs\ai4anim-fbxloading
python -m pip install sip==6.6.2
python -m pip install .
python -c "import fbx, ai4animation"
```

### SIGGRAPH_2020 训练资产说明

`AI4Animation/AI4Animation/SIGGRAPH_2020/DeepLearning/Weights` 下的 `.bin` 文件属于 Unity 运行时权重；Python 训练/推理脚本仍然需要单独的 `dataset/` 与 checkpoint 目录。

因此，即使 legacy 环境中的 `torch / tensorflow / tensorboardX` 已安装成功，只要没有先从 Unity 导出数据，也不应把该链路视为端到端训练完成。

---
**配置完毕后，如果部分资源加载花屏或粉红色，可能意味着 URP/HDRP 管线在编辑器导入时出现了 Shader 断链，请重新 Reimport All。**
