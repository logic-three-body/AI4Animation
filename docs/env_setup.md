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

AI4Animation 分散在多个学术会议的项目目录下，**大部分案例依赖 Unity 2022.3.11f1 版本**。

### 2.1 浏览器辅助下载 Unity

如果您本地未安装该确切版本（版本不匹配极易导致脚本和包报错），请按照以下步骤使用浏览器直接唤起安装：

1. 确保 **Unity Hub** 正在运行。
2. 打开浏览器，复制并粘贴以下连接打开（或点击）：
   `unityhub://2022.3.11f1/d00248457e15`
3. 也可以前往 [Unity Download Archive](https://unity3d.com/get-unity/download/archive)，找到 2022.3.11f1，点击 **Unity Hub (Install)**。
4. 安装模块中，如果不打包发布，只需要勾选默认模块即可（无需额外安装 Android, iOS 支持），建议勾选安装最新版本的 Visual Studio 方便脚本调试。

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
| **SIGGRAPH_2021** | 武术运动的神经分层动画 | `AI4Animation/AI4Animation/SIGGRAPH_2021/Unity` |
| **SIGGRAPH_2020** | 篮球及接触感知的多阶段运动 | `AI4Animation/AI4Animation/SIGGRAPH_2020/Unity` |
| **SIGGRAPH_Asia_2019** | 物体交互 (箱子/椅子/门等) | `AI4Animation/AI4Animation/SIGGRAPH_Asia_2019/Unity` |
| **SIGGRAPH_2018** | 四足动物运动模式适应 | `AI4Animation/AI4Animation/SIGGRAPH_2018/Unity` |
| **SIGGRAPH_2017** | 基于相位的实时地形自适应运动 | `AI4Animation/AI4Animation/SIGGRAPH_2017/Unity` |

**启动动作**：
1. 打开 Unity Hub -> Projects -> 点击 `Add` 或 `Open`
2. 逐一导航并选择上方给出的路径。
3. 如果弹出“升级版本”或者“Safe Mode”提示，请确保您安装的 Unity 版本精确匹配该项目目录中 `ProjectSettings/ProjectVersion.txt` 声明的版本（如 `2022.3.11f1`），切勿随意点击“Upgrade”。

---

## 四、Python 训练环境配置（进阶）

如果仅需运行包含的内置 Demo 动画，配置完 Unity 即可完成部署。如果需要使用仓库中自带的数据进行从头训练或者导出特征（深度学习脚本），它们分别存放在对应子级目录如 `AI4Animation/SIGGRAPH_2020/DeepLearning`。

同理，**每个研究项目使用的 Python 环境及库版本差别极大**。

> **我们强烈建议为每个需要的实验单独配置 conda env 隔离。**

### 配置流程示例

以 SIGGRAPH_2020 项目为例进行训练（各子项目环境要求不同，详细请参考对应子项目的 README）：

```powershell
# 1. 建立基于对应项目的特定独立环境
conda create -n ai4anim-2020 python=3.7
conda activate ai4anim-2020

# 2. 从各自独立的 requirements 或 environment 导入
# 注意：各研究项目由于跨度从 2017 到 2024，所依赖的 PyTorch 版本会有极大差异
# 请优先参考各自目录下的环境配置文件如 environment.yml 或 requirements.txt
conda env update -f AI4Animation/AI4Animation/SIGGRAPH_2020/DeepLearning/Models/GenerativeModel/environment.yml

# 3. 运行后需要重启 Unity 环境及检查 ONNX 插件联调支持。
```

> **提示:** 如果想尝试无 Unity 依赖的纯 Python/Pytorch 最新实现方式，请移步参考根项目外层的 `ai4animationpy` 环境。

---
**配置完毕后，如果部分资源加载花屏或粉红色，可能意味着 URP/HDRP 管线在编辑器导入时出现了 Shader 断链，请重新 Reimport All。**
