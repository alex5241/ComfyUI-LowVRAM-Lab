# ComfyUI-LowVRAM-Lab
Experiments and optimizations for running ComfyUI on low-VRAM GPUs.

## 项目简介
本仓库是我在低显存显卡（4060Ti 8GB）上使用 ComfyUI 进行AI创作的笔记记录。

## 项目创建原因
我在使用gpt或者deepseek解决comfyui相关的技术问题时，经常得到比较模糊或者不准确的回答。
我觉得可能是相关技术还比较新，而且各种大模型每月也都在更新，网络上还没有足够多的数据供给AI来抓取。
所以我创建了这个项目来自己记录。
希望我的项目对你有用。

## 我的硬件配置
- GPU：RTX 4060Ti 8GB
- CPU：12600kf
- RAM：32GB
- SSD: 1T
- 系统：Windows 11

## 我的comfyui配置
- 整合包：秋叶整合包 
- 版本号：ComfyUI-aki-v1.7 （大小：4G左右）



## 关于新遮罩模型sec4b，号称碾压sam2。
- 最开始需下载4个模型文件，共20g左右。 https://huggingface.co/OpenIXCLab/SeC-4B/tree/main 
- 后来有了单模型下载，且有fp8支持。https://huggingface.co/VeryAladeen/Sec-4B/tree/main
- 模型需要搭配配套comfyui节点一起使用。 https://github.com/9nate-drake/Comfyui-SecNodes
- 模型放置，节点文档说的是models/sams/sec-4b/，但我本地这样放置不对，我的位置在models\sams。
- 经测试，即便是fp8模型，我的8g显卡也跑不起来。运行工作流 load sec 会直接挂掉。这也符合文档说明，文档建议最低12g显存。
- 模型开发者称： fp8 模型发现有问题 ，comfyui节点已经删除对fp8支持，建议使用 fp16。
- 社区有更省显存和更快速度的节点。 https://github.com/lihaoyun6/ComfyUI-SecNodes_Ultra_Fast。
- 经测试，ComfyUI-SecNodes_Ultra_Fast节点和fp16 在我本地可以完美运行。然后在wan2.2 animate工作流里也正常运行。


## License
MIT License
转载请注明出处

## 最后更新：2025-10-20
## 项目地址：https://github.com/alex5241/ComfyUI-LowVRAM-Lab
