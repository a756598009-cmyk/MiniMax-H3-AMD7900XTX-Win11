# MiniMax H3 图生视频 AMD RX 7900 XTX Windows11 部署成功方案

> 本方案根据实际部署过程整理：Windows11 + AMD RX 7900 XTX + ComfyUI AMD Portable。
> 已验证：首图 I2V、首图+尾图 I2V 均成功生成5秒视频。

## 一、硬件环境

| 项目 | 配置 |
| --- | --- |
| 系统 | Windows 11 |
| 显卡 | AMD Radeon RX 7900 XTX |
| 显存 | 24GB |
| 内存 | 32GB |
| 虚拟内存 | 124GB |

## 二、ComfyUI AMD Portable

使用方式：下载 AMD Portable 版本，解压即可，不需要单独安装 Python。

官方地址：

<https://github.com/Comfy-Org/ComfyUI/releases>

解压目录：

```
C:\H3\ComfyUI_windows_portable_amd
```

## 三、安装自定义节点

进入 ComfyUI目录：

```
cd /d C:\H3\ComfyUI_windows_portable_amd
```

安装 GGUF Loader 节点：

```
git clone https://github.com/ChrisColeTech/ComfyUI-GGUF-Loader ComfyUI/custom_nodes/ComfyUI-GGUF-Loader
```

安装依赖：

```
.\python_embeded\python.exe -s -m pip install -r .\ComfyUI\custom_nodes\ComfyUI-GGUF-Loader\requirements.txt
```

## 四、模型下载

### 1. H3主模型

文件：

```
minimax_h3_fl2va_pruned_int8_convrot.safetensors
```

目录：

```
ComfyUI\models\diffusion_models\
```

[下载](https://huggingface.co/Comfy-Org/MiniMax-H3/tree/main/diffusion_models)

### 【新增】R2V 参考视频模型

**这是 R2V（Reference Video to Video）新增使用的模型，原 I2V 模型不受影响。**

文件：

```
minimax_h3_ref2va_pruned_int8_convrot.safetensors
```

目录：

```
ComfyUI\models\diffusion_models\
```

**【新增】R2V 专用模型下载地址（夸克网盘）**

这是 **R2V 专用模型**，用于 MiniMax H3 的参考视频（R2V）工作流。

[【新增】夸克网盘：R2V 专用模型下载](https://pan.quark.cn/s/ad940a6812e1?pwd=Y87G)

提取码：`Y87G`

[备用：Hugging Face 官方模型地址](https://huggingface.co/Comfy-Org/MiniMax-H3/resolve/main/diffusion_models/minimax_h3_ref2va_pruned_int8_convrot.safetensors)

### 2. Qwen3VL文本编码器

```
qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors
```

```
ComfyUI\models\text_encoders\
```

[下载](https://huggingface.co/Comfy-Org/MiniMax-H3/resolve/main/text_encoders/qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors)

### 3. Video VAE

```
minimax_h3_video_vae_fp16.safetensors
```

```
ComfyUI\models\vae\
```

[下载](https://huggingface.co/Comfy-Org/MiniMax-H3/resolve/main/vae/minimax_h3_video_vae_fp16.safetensors)

### 4. Audio VAE

```
minimax_h3_audio_vae_fp32.safetensors
```

```
ComfyUI\models\vae\
```

[下载](https://huggingface.co/Comfy-Org/MiniMax-H3/resolve/main/vae/minimax_h3_audio_vae_fp32.safetensors)

### 5. Turbo LoRA

```
minimax_h3_fl2v_turbo_8step_v1.0_comfyui_bf16.safetensors
```

```
ComfyUI\models\loras\
```

[下载](https://huggingface.co/Comfy-Org/MiniMax-H3/tree/main/loras)

## 五、模型目录

```
models

├─ diffusion_models
│  ├─ minimax_h3_fl2va_pruned_int8_convrot.safetensors
│  └─ 【新增】minimax_h3_ref2va_pruned_int8_convrot.safetensors
│
├─ text_encoders
│  └─ qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors
│
├─ loras
│  └─ minimax_h3_fl2v_turbo_8step_v1.0_comfyui_bf16.safetensors
│
└─ vae
   ├─ minimax_h3_video_vae_fp16.safetensors
   └─ minimax_h3_audio_vae_fp32.safetensors
```

## 六、启动命令

```
cd /d C:\H3\ComfyUI_windows_portable_amd

.\python_embeded\python.exe -s ComfyUI\main.py --windows-standalone-build
```

## 七、最终最佳参数

### 首图模式

- 分辨率：576×320
- 长度：124 frames
- Steps：6
- Turbo LoRA开启

### 首图+尾图模式（推荐）

- first\_frame：首图
- last\_frame：尾图
- 分辨率：576×320
- 长度：124 frames
- Steps：6

## 八、实际效果

| 模式 | 结果 |
| --- | --- |
| 首图 | 约4分钟生成5秒视频 |
| 首图+尾图 | 约4分钟生成5秒视频 |

## 九、最终推荐

AMD RX 7900 XTX + Windows11 下推荐：

MiniMax H3 + Qwen3VL NVFP4 + Turbo LoRA

576×320 / 124 frames / 6 steps

> 【新增】R2V 模型说明：
> 如果使用 MiniMax H3 官方 R2V 工作流，需要额外下载：
> minimax\_h3\_ref2va\_pruned\_int8\_convrot.safetensors
> 放入：
> ComfyUI\models\diffusion\_models\
> 【新增】R2V 专用模型夸克网盘：
> 点击下载 R2V 专用模型
> 提取码：
> Y87G
> 备用：
> Hugging Face 官方模型地址

## 十、ComfyUI 工作流下载

本教程对应的 MiniMax H3 工作流：

- 首图 I2V 工作流
- 首图 + 尾图 I2V 工作流

下载地址：

[夸克网盘：MiniMax H3 AMD7900XTX 工作流](https://pan.quark.cn/s/0a84218d8ad3?pwd=dvv1)

提取码：`dvv1`

## 十一、完整整合包下载

如果不想单独配置环境，可以直接下载已经整理好的 MiniMax H3 AMD RX 7900 XTX Windows11 整合包：

- 包含 ComfyUI AMD 环境
- 已配置 MiniMax H3 工作流
- 适配 AMD RX 7900 XTX + Windows11

[夸克网盘：MiniMax H3 AMD7900XTX Windows11 整合包下载](https://pan.quark.cn/s/adc31e33b7e9?pwd=PeUa)

提取码：`PeUa`

## 十二、AMD RX 6000 系列显卡运行环境报错解决方法

> 适用显卡：
> AMD Radeon RX 6000 系列（RDNA2，例如 RX 6600 / 6700 XT / 6800 / 6800 XT / 6900 XT 等）。
> ① 出现
> vcruntime.h
> 、
> MSVC
> 、Triton 编译失败等运行环境错误：
> 请先安装 Microsoft Visual C++ Runtime 和 Visual Studio Build Tools，并在 Visual Studio Build Tools 中勾选
> Desktop development with C++（使用 C++ 的桌面开发）
> ，同时安装对应的 Windows SDK。
> 安装完成后重新启动电脑，再重新运行 ComfyUI。
> ② RX 6000 系列使用 INT8 模型出现报错、卡死或原生 INT8 ConvRot 不稳定：
> 优先使用最新版
> ComfyUI-ROCm
> 以及最新版
> ComfyUI-INT8-Fast-ROCM
> 。
> RX 6000 属于 RDNA2，最新版 ComfyUI-ROCm 已针对 RDNA2 及以下显卡加入 INT8 自动接管机制：
> INT8 操作会自动转由 INT8-Fast-ROCM 自带的 INT8 Kernel 处理，而不是继续使用
> Comfy-Kitchen 的原生 INT8 实现。
> 因此，RX 6000 用户不要强制开启不兼容的原生 INT8 ConvRot/Triton 后端；保持默认配置即可。
> ③ 如果安装时出现 GPU 架构检测失败：
> 建议直接重新下载最新版 ComfyUI-ROCm，重新运行
> install.bat
> ；
> 新版已经持续修复 GPU 架构检测，并针对 RDNA1/RDNA2 的检测和安装逻辑进行了改进。
> 最新版 ComfyUI-ROCm：
> https://github.com/patientx-cfz/comfyui-rocm/releases
> 特别说明：
> RX 6000（RDNA2）与 RX 7000（RDNA3）不要完全照搬同一套加速参数。
> RX 6000 优先保证 ROCm/PyTorch、MSVC 编译环境以及 INT8-Fast-ROCM 的兼容性，再进行性能优化。

6000系专用

## AMD RX 6000 系列显卡专用：运行环境与报错解决方法

以下内容独立于前面的 7900 XTX 部署方案，专门用于 RX 6000 系列显卡。

> 本记录只保留最终成功方案。
> 已删除前面排查过程中出现的失败命令、报错命令、无效测试以及后来被替换掉的旧版本方案。
> 本记录以实际成功运行 MiniMax H3 INT8 加速的环境为准。

## 一、软件下载地址

> Python 3.12.7 Include + Libs 开发环境补充包
> 这是 Triton-Windows 官方发布的 Python 开发文件补充包，专门用于
> ComfyUI Portable 的嵌入式 Python 环境。压缩包内包含
> include
> 和
> libs
> 两个目录，用于补齐
> Python.h
> 和
> python312.lib
> 等编译所需文件。
> 官方直链：
> python\_3.12.7\_include\_libs.zip
> 安装位置：
> 将压缩包中的
> include
> 、
> libs
> 文件夹复制到
> ComfyUI\_windows\_portable\_amd\python\_embeded\
> 下。
> 最终应存在：
> python\_embeded\include\Python.h
> python\_embeded\libs\python312.lib
> 对应官方 Release：
> Triton-Windows v3.0.0-windows.post1
> ComfyUI-INT8-Fast-ROCM-ConvRot-1.0.1
> 本教程实际使用的 MiniMax H3 INT8 Fast ROCm 加速组件。
> 安装后放在
> ComfyUI\custom\_nodes\
> 目录下，由 ComfyUI 启动时自动加载。
> 实际安装目录：
> ComfyUI\custom\_nodes\ComfyUI-INT8-Fast-ROCM-ConvRot-1.0.1
> H3 INT8 加速生效日志：
> MiniMax H3 INT8 Fast: using the ROCm Triton backend for int8\_linear only; HIP remains preferred for all other supported operations.
> 项目/下载来源：
> ComfyUI-INT8-Fast-ROCM-ConvRot-1.0.1-ConvRot

> 推荐方式：
> 直接使用 ComfyUI-ROCm 官方整合包。它是部分便携式环境，不要求系统预先安装 Python；
> 安装器会自动准备 Python 3.12 Embeddable、Python development files 以及相关 ROCm/PyTorch 环境。
> 同时整合包已经包含 ComfyUI-INT8-Fast-ROCM-ConvRot-1.0.1，建议优先使用整合包内的最新版 INT8 节点。

## 二、ComfyUI 目录

```
C:\H3\ComfyUI_windows_portable_amd
```

进入目录：

```
cd C:\H3\ComfyUI_windows_portable_amd
```

## 三、安装 Triton

最终使用的是 Triton Windows 3.7 系列。

```
.\python_embeded\python.exe -m pip install -U "triton-windows>=3.7,<3.8"
```

安装完成后验证版本：

```
.\python_embeded\python.exe -c "import triton; print('Triton:',triton.__version__)"
```

> 成功结果：
> Triton: 3.7.1

## 四、Python 编译环境检查

为了支持 Triton / 自定义 Kernel 的编译环境，需要确认 Python 头文件和 import library 已存在。

```
Test-Path .\python_embeded\include\Python.h
```

成功结果：

```
True
```

```
Test-Path .\python_embeded\libs\python312.lib
```

成功结果：

```
True
```

> 注意：
> 最终正确目录名是小写的
> include
> 和
> libs
> 。

## 五、INT8-Fast-ROCM 自定义节点

本次 MiniMax H3 INT8 加速使用的节点目录：

```
C:\H3\ComfyUI_windows_portable_amd\ComfyUI\custom_nodes\ComfyUI-INT8-Fast-ROCM-ConvRot-1.0.1
```

验证 INT8 Triton Kernel 是否能够被 Python 正常导入：

```
.\python_embeded\python.exe -c "import sys; sys.path.insert(0,r'C:\H3\ComfyUI_windows_portable_amd\ComfyUI\custom_nodes\ComfyUI-INT8-Fast-ROCM-ConvRot-1.0.1'); import int8_fused_kernel; print('H3 INT8 Triton kernel: OK')"
```

> 成功结果：
> H3 INT8 Triton kernel: OK

## 六、ComfyUI 启动命令

最终成功启动 MiniMax H3 的命令：

```
cd C:\H3\ComfyUI_windows_portable_amd

.\python_embeded\python.exe -s ComfyUI\main.py --windows-standalone-build
```

## 七、启动后必须看到的 INT8 加速信息

ComfyUI 成功启动并执行 H3 工作流后，关键是检查控制台是否出现以下信息：

```
[INFO] MiniMax H3 INT8 Fast: using the ROCm Triton backend for int8_linear only; HIP remains preferred for all other supported operations.
```

这表示：

- **int8\_linear** 使用 ROCm Triton INT8 后端。
- 其他支持的 INT8 / ConvRot 操作仍优先使用 HIP。
- 不是所有操作都强制使用 Triton。

同时可以看到量化相关信息：

```
[INFO] Found quantization metadata version 1
[INFO] Detected mixed precision quantization
[INFO] Using mixed precision operations
[INFO] Native ops: int8_tensorwise, convrot_w4a4, asym_w4a8_int8
[INFO] model weight dtype torch.bfloat16, manual cast: torch.bfloat16
```

> 判断 INT8 加速是否真正启用：
> 不要只看“ComfyUI 能启动”。
> 真正执行 H3 工作流时，应重点看到
> MiniMax H3 INT8 Fast: using the ROCm Triton backend for int8\_linear only
> 。
> 这才是本次 INT8-Fast-ROCM 加速节点实际接管工作的关键日志。

## 八、MiniMax H3 模型运行确认

成功运行时，我们的控制台还出现了以下关键内容：

```
[INFO] Requested to load MiniMaxH3
[INFO] loaded completely; 21860.18 MB usable, 19996.14 MB loaded, full load: True
```

实际测试完成后，控制台出现：

```
[INFO] Prompt executed in 510.49 seconds
```

## 九、最终 INT8 加速方案结构

| 层级 | 使用方案 |
| --- | --- |
| GPU | AMD Radeon RX 7900 XTX / gfx1100 |
| 计算平台 | ROCm 7.2 |
| PyTorch | 2.9.1+rocm7.2.1 |
| Triton | 3.7.1 |
| INT8 自定义加速 | ComfyUI-INT8-Fast-ROCM-ConvRot-1.0.1 |
| INT8 Linear | ROCm Triton backend |
| 其他支持的 INT8 / ConvRot | HIP 优先 |
| 模型 | MiniMax H3 INT8 ConvRot |

## 十、最终执行顺序

1. 进入 `C:\H3\ComfyUI_windows_portable_amd`。
2. 确认 Python 3.12 的开发文件存在。
3. 安装 Triton Windows 3.7.x。
4. 确认 `triton.__version__` 为 3.7.1。
5. 确认 `int8_fused_kernel` 可以正常导入。
6. 启动 ComfyUI。
7. 加载 MiniMax H3 INT8 模型。
8. 执行工作流。
9. 检查控制台是否出现 `MiniMax H3 INT8 Fast` 的 ROCm Triton INT8 日志。

## 十一、最终结论

> 本次实际成功的核心组合：
> AMD ROCm 环境
> ＋ PyTorch ROCm
> ＋ Triton Windows 3.7.1
> ＋ ComfyUI-INT8-Fast-ROCM-ConvRot-1.0.1
> ＋ MiniMax H3 INT8 ConvRot 模型
> 最终由 INT8-Fast-ROCM 对 H3 的
> int8\_linear
> 使用 ROCm Triton 加速，
> 同时让 HIP 继续负责其他适合的 INT8 / ConvRot 操作。

本页面只保留本次部署最终确认成功的环境、命令、验证方式和成功结果。
失败命令、错误日志、无效测试以及后来被替换的旧配置均未整理进来。
