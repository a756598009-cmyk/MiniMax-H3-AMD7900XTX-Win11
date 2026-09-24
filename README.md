# MiniMax H3 A卡本地部署方案，小白建议用整合包 【支持图生视频/文生视频/首图加尾图生视频/多参考视频音频生视频/文生图/图生图/多图生图/】

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

## 五、安装 MiniMax H3 Image Studio

启动 ComfyUI 便携版，然后开**第二个 CMD**，执行：

```cmd
cd /d C:\H3\ComfyUI_windows_portable_amd\ComfyUI\custom_nodes
```

然后执行：

```cmd
git clone https://github.com/astropuzzo/ComfyUI-MiniMax-H3-Image-Studio.git
```

然后安装它需要的依赖：

```cmd
C:\H3\ComfyUI_windows_portable_amd\python_embeded\python.exe -m pip install -r ComfyUI-MiniMax-H3-Image-Studio\requirements.txt
```

官方 Git 安装方式就是把仓库 clone 到 `ComfyUI/custom_nodes`，然后重启 ComfyUI。

## 六、模型目录

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

## 七、启动命令

```
cd /d C:\H3\ComfyUI_windows_portable_amd

.\python_embeded\python.exe -s ComfyUI\main.py --windows-standalone-build
```

## 八、最终最佳参数

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

## 九、实际效果

| 模式 | 结果 |
| --- | --- |
| 首图 | 约4分钟生成5秒视频 |
| 首图+尾图 | 约4分钟生成5秒视频 |

## 十、最终推荐

AMD RX 7900 XTX + Windows11 下推荐：

MiniMax H3 + Qwen3VL NVFP4 + Turbo LoRA

576×320 / 124 frames / 6 steps

> 【新增】R2V 模型说明：
> 如果使用 MiniMax H3 官方 R2V 工作流，需要额外下载：
> minimax\_h3\_ref2va\_pruned\_int8\_convrot.safetensors
> 放入：
> ComfyUI\models\diffusion\_models\
> 备用：
> Hugging Face 官方模型地址

## 十一、完整整合包下载

如果不想单独配置环境，可以直接下载已经整理好的 MiniMax H3 AMD RX 7900 XTX Windows11 整合包：

- 包含 ComfyUI AMD 环境
- 已配置 MiniMax H3 工作流
- 适配 AMD RX 7900 XTX + Windows11

[夸克网盘：MiniMax H3 AMD7900XTX Windows11 整合包下载](https://pan.quark.cn/s/1de84b8b3a72?pwd=iJQP)

提取码：`iJQP`

## 十三、AI修复视频成4K/补帧

下载链接：

[夸克网盘：AI修复视频成4K/补帧](https://pan.quark.cn/s/21023fdbc6f2?pwd=yASA)

提取码：`yASA`

## 十四、MiniMax H3 九宫格连续电影 Storyboard 视频提示词

### 对应参考图

![MiniMax H3 九宫格连续电影 Storyboard](minimax-h3-9grid-storyboard.jpg)

以下提示词对应上方 3×3 九宫格参考图，用于将九宫格作为整个视频的核心视觉分镜参考：

```text
请将输入的3×3九宫格图片作为整个视频的核心视觉分镜参考。

这张九宫格是一份连续的电影 Storyboard，9个画面代表同一个故事中的9个连续关键时刻，而不是9张彼此独立的图片。

请按照九宫格从左到右、从上到下的顺序理解故事发展，并在9个关键画面之间自动生成自然、连贯的中间动作和镜头运动，将它们最终呈现为一段完整的、连续的、具有电影感的真人实拍风格视频。

不要直接把九宫格图片逐格播放或简单切换。

---

【一、整体故事】

故事发生在盛唐时期的长安城。

深夜，一名年轻漂亮的东方女子悄悄潜入灯火璀璨的长安城。她行动谨慎、身手敏捷，在进入城中后发现自己似乎被人跟踪，于是开始穿过街巷逃避追踪，最终翻上屋顶，在月光下俯瞰整座长安城，并跃过屋顶消失在夜色之中。

整个视频应该像一段真实拍摄完成的高预算古装动作电影片段。

整体节奏：

神秘潜入 → 进入长安 → 察觉危险 → 快速奔跑 → 翻上屋顶 → 躲避追兵 → 俯瞰长安 → 飞跃屋顶 → 消失在夜色中。

---

【二、人物一致性】

整个视频始终保持九宫格中的同一名年轻东方女子作为唯一主要人物。

必须保持人物身份高度一致：

* 同一张脸
* 同样的五官比例
* 同样的脸型
* 同样的黑色长发
* 同样的古代发型
* 同样的发饰
* 同样的红黑色古装
* 同样的身材比例
* 同样的整体人物气质

不同镜头之间绝对不要改变人物外貌。

不要突然换脸。

不要改变发型。

不要改变服装颜色。

不要改变服装款式。

不要让人物突然变老或变年轻。

不要出现多个长相不同的女主角。

人物在运动过程中仍然必须保持脸部和身体比例稳定。

---

【三、第一段：长安城外】

视频从夜晚的长安城外开始。

镜头首先使用一个具有电影感的远景。

明亮的圆月悬挂在夜空中，月光照亮宏伟的长安城门。

远处可以看到高大的唐代城楼、屋檐、灯笼和密集的古代建筑。

女主角站在画面前景，身穿红黑色古装，长发随着夜风轻轻飘动。

她安静地观察远处的城门和周围环境。

镜头缓慢向人物靠近。

人物没有夸张动作，只表现出谨慎、警觉和准备行动的状态。

环境安静而神秘。

---

【四、第二段：潜入城门】

女主角开始向城门靠近。

她利用建筑阴影和夜色隐藏自己的身影。

镜头切换到人物的中近景。

她观察附近的守卫。

守卫位于背景中，人物保持自然运动，不需要成为主要视觉焦点。

女主角确认周围环境后，快速从阴影中穿过。

镜头进行轻微跟拍。

她成功进入长安城。

动作应该自然、克制、符合真实人体运动。

不要出现瞬移。

不要出现突然改变位置。

---

【五、第三段：进入繁华长安街道】

进入城内之后，场景突然变得更加繁华。

长安街道灯火通明。

大量古代行人自然走动。

两侧悬挂着大量暖黄色灯笼。

远处可以看到唐代建筑和楼阁。

女主角穿过人群。

镜头从人物背后进行跟拍。

她的红色衣裙、长发和衣袖随着行走产生自然运动。

镜头逐渐靠近人物。

她突然感觉到身后似乎有人正在跟踪自己。

她的脚步逐渐放慢。

随后快速回头。

---

【六、第四段：发现追踪者】

切换到女主角的近景和面部特写。

她回头观察身后的街道。

她的眼神变得警觉。

背景中的行人仍然自然活动。

远处隐约出现正在接近的追踪者。

不要把追踪者拍得过于清晰。

重点表现女主角发现危险之后的情绪变化。

她意识到自己可能暴露了。

停顿片刻之后，她立即转身。

---

【七、第五段：街巷奔跑】

女主角突然开始快速奔跑。

镜头迅速跟随她进入狭窄的古代街巷。

采用电影级动态跟拍。

可以使用低机位、侧向跟拍和轻微手持摄影感，但整体必须稳定、流畅。

女主角快速穿过狭窄的石板街道。

她的长发、衣袖、裙摆、红色丝带随着奔跑速度产生真实的惯性运动。

身体重心随着奔跑自然变化。

脚步与地面接触必须真实。

衣物不能出现不符合物理规律的飘动。

镜头随着人物运动自然向前推进。

整个过程应该有明显的速度感和紧迫感。

---

【八、第六段：翻上屋顶】

女主角来到古建筑旁。

她迅速借助墙壁和屋檐向上攀爬。

随后抓住屋檐边缘，用力翻上屋顶。

动作必须具有真实的身体重量。

起跳、抓住屋檐、身体翻越、落脚、站稳，这几个动作自然连贯。

不要出现瞬移。

不要出现肢体变形。

不要让人物像飞起来一样。

落到屋顶之后，她短暂停顿，然后继续向前移动。

镜头可以从侧面跟拍。

---

【九、第七段：屋顶逃避追踪】

女主角在月光下快速穿过唐代建筑屋顶。

镜头从侧后方跟随。

周围是密集的古代屋顶、楼阁和灯火。

她突然听到身后的声音。

立即蹲下来隐藏在屋檐后方。

镜头逐渐靠近。

人物只露出部分脸和眼睛。

她屏住呼吸，观察下方。

追踪者从建筑下方经过。

女主角保持安静。

这一段节奏明显放慢，形成前后节奏对比。

---

【十、第八段：俯瞰长安】

危险过去之后，女主角慢慢站起来。

镜头从人物背后开始缓慢向后拉远。

画面逐渐展示整座长安城。

大量古代建筑、街道、楼阁和灯笼延伸到远方。

无数暖黄色灯光形成壮观的城市夜景。

天空中是一轮明亮圆月。

女主角站在屋顶边缘，长发和红色衣裙在夜风中缓慢飘动。

她安静地俯瞰整座长安城。

这一段应该成为整个视频最具有电影感和视觉规模感的镜头。

镜头运动缓慢、稳定、优雅。

---

【十一、第九段：飞跃屋顶并消失】

女主角突然向前奔跑。

镜头迅速跟随。

她从一座屋顶跳向另一座屋顶。

动作必须真实。

起跳时身体产生明显的发力感。

腾空过程中，红色衣裙、长发和丝带向后飞扬。

月光从人物后方照射，形成漂亮的轮廓光。

人物落到另一座屋顶之后继续向前奔跑。

镜头逐渐拉远。

女主角最终消失在古代建筑和夜色之中。

最后留下宏大的长安夜景、灯笼和月光。

以电影式远景结束。

---

【十二、镜头语言】

整体使用高预算真人电影摄影语言。

镜头应该自然地随着故事发展变化。

前段：

远景建立长安城环境。

中景展示人物行动。

近景表现人物警觉。

特写表现人物眼神和情绪。

中段：

使用跟拍表现奔跑。

使用低机位增强动作力量。

使用侧向移动表现追逐。

使用动态摄影表现紧迫感。

后段：

使用稳定的缓慢拉远展示长安城。

最后使用远景完成电影式收尾。

镜头运动必须具有真实摄影机的物理感。

不要出现游戏摄像机式的快速旋转。

不要突然360度旋转。

不要无意义地疯狂推拉。

不要频繁快速切换镜头。

---

【十三、人物动作和物理规律】

所有人物动作必须符合真实人体运动规律。

奔跑：

身体重心自然前倾。

手臂自然摆动。

双腿运动符合真实跑步节奏。

脚步与地面产生真实接触。

头发、衣袖、裙摆和丝带产生真实惯性。

翻越：

先观察落脚位置。

身体发力。

手臂抓住屋檐。

身体向上移动。

翻越屋檐。

双脚自然落地。

落地后身体产生轻微缓冲。

跳跃：

自然助跑。

真实起跳。

短暂腾空。

衣服和头发产生空气阻力。

自然落地。

整个动作必须有重量感。

禁止出现：

瞬移

漂浮

滑行

动作跳帧

肢体拉伸

手脚数量错误

手指畸形

身体突然改变比例

不符合物理规律的飞行

---

【十四、光影】

使用真实电影级夜景光影。

主要光源为：

蓝色月光。

暖黄色灯笼光。

建筑内部暖光。

月光和灯笼形成自然的冷暖色对比。

人物经过灯笼时，脸部和衣服受到温暖的橙黄色光照。

进入阴影时，人物重新受到冷色月光照射。

头发和衣服边缘产生自然轮廓光。

远处建筑具有自然的大气透视。

空气中存在非常轻微的夜间薄雾。

不要过度使用雾气。

不要让画面看起来像游戏CG。

---

【十五、真实材质】

人物皮肤保持真实摄影质感。

皮肤具有自然纹理。

头发具有真实发丝细节。

衣服具有真实布料纹理。

红色丝绸、黑色布料和金属发饰具有不同的材质反射。

古代建筑具有真实木材、砖石和瓦片质感。

灯笼产生真实的光照。

所有物体都应该具有真实的空间关系和光影。

---

【十六、整体视觉风格】

真实真人摄影。

高预算中国古装电影。

电影级摄影。

真实人物。

真实皮肤。

真实头发。

真实服装。

真实布料运动。

真实环境光。

真实建筑。

电影级景深。

自然运动模糊。

高动态范围。

细腻的夜景细节。

真实体积光。

自然大气透视。

高质量电影构图。

整体效果应该像使用专业电影摄影机拍摄的一段唐代古装动作电影。

不要做成动漫。

不要做成插画。

不要做成游戏CG。

不要做成3D人物。

不要出现明显的AI生成感。

---

【十七、最重要的连续性要求】

请将九宫格中的9个画面视为同一个连续故事中的9个关键帧。

不是9张独立图片。

不是9段完全独立的视频。

不是简单的图片切换。

需要在9个关键画面之间生成自然的中间运动。

人物的位置变化必须符合空间逻辑。

人物的动作必须具有前后因果关系。

人物的服装、脸部、发型和身体比例必须始终保持一致。

环境必须具有连续性。

光照必须保持一致。

时间必须保持在同一个深夜。

镜头应该像真正的电影摄影师连续拍摄出来的一样。

九宫格只作为视觉分镜参考，最终视频中绝对不要出现九宫格。

最终视频中不要显示：

任何文字

任何字幕

任何标题

任何编号

任何分镜框线

任何九宫格边框

任何UI元素

任何Logo

任何水印

不要将输入图片中的九宫格结构直接复制到视频中。

最终输出应该是一段完整、连贯、具有真实电影质感的“年轻女子夜闯长安城”故事。
```

## 十五、AMD RX 6000 系列显卡运行环境报错解决方法

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
