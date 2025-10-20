# ComfyUI-LowVRAM-Lab
Experiments and optimizations for running ComfyUI on low-VRAM GPUs.

## 项目简介
本仓库是我在低显存显卡（4060Ti 8GB）上使用 ComfyUI 进行AI创作的笔记记录。

## 项目创建原因
- 我在使用gpt或者deepseek解决comfyui相关的技术问题时，经常得到比较模糊或者不准确的回答。
- 我觉得可能是相关技术还比较新，而且各种大模型每月也都在更新，网络上还没有足够多的数据供给AI来抓取。
- 所以我创建了这个项目来自己记录。
- 本项目所有内容均为真实+原创+手打，希望我的项目对你有帮助。
- 本项目长期更新。

## 我的硬件配置
- GPU：RTX 4060Ti 8GB
- CPU：12600kf
- RAM：32GB
- SSD: 1T
- 系统：Windows 11

## 我的comfyui配置
- 整合包：秋叶整合包 
- 版本号：ComfyUI-aki-v1.7 （大小：4G左右）

## 一些通用的话
- 我的这套配置，在玩视频大模型，比如wan系列的时候，基本上都是无法跑起来的。
- 通常意义上，社区都会有量化或者蒸馏模型，比如gguf或者light2v系列。他们能让低配置的机器跑起来。
- 很多人以为只需要显存大就行，实际上内存也很重要，我32g物理内存+32g虚拟内存，经常占用双100%。建议48g内存起步。
- 大显存比高级别显卡重要。比如5060ti 16g，优于5070 12g.很多模型显存大了才能跑。
- 经常有显卡一样的人跑一套工作流，一个跑不了，一个能跑。可能就是一个内存大，显存通过block swap,用内存顶了一下。
- wan2.2 在技术上完全替代wan2.1，我现在已经删除了wan2.1的所有模型及工作流。
- wan2.5 风声大雨点小，10.1前开放的是收费模式，可能后期不会再开源。目前都在等消息。
- 如果现成的业务需要AI快速出东西，我建议直接在云平台如RH上玩，因为本地搭建环境还挺复杂的，尤其是对于非技术人员。 

## 关于wan2.2
- 我的配置是无法跑wan2.2官方模型的 ，只能去跑gguf，在牺牲一定的精度后换取到了本地的可运行。
- 我本地跑的是gguf q4，我觉得我应该能跑到q5。
- 我本地是不跑KJ的所有工作流的。根据我的不充分测试，KJ的工作流一般比官方流更吃机器配置，且需要更大的内存。

## 关于遮罩
- 自动版方案： unethuman，该方案对局部支持不如 sam2.1+grouded，后者直接可以写单词指定去除的局部，比如face ,dress之类的）  
- 手动版本：自己画遮罩。 低配可以用sam2 ，高配使用 sec4b 。

## 关于新遮罩模型sec4b，号称碾压sam2。
- 最开始需下载4个模型文件，共20g左右。 https://huggingface.co/OpenIXCLab/SeC-4B/tree/main 
- 后来有了单模型下载，且有fp8支持。https://huggingface.co/VeryAladeen/Sec-4B/tree/main
- 模型需要搭配配套comfyui节点一起使用。 https://github.com/9nate-drake/Comfyui-SecNodes
- 模型放置，节点文档说的是models/sams/sec-4b/，但我本地这样放置不对，我的位置在models\sams。
- 经测试，即便是fp8模型，我的8g显卡也跑不起来。运行工作流 load sec 会直接挂掉。这也符合文档说明，文档建议最低12g显存。
- 模型开发者称： fp8 模型发现有问题 ，comfyui节点已经删除对fp8支持，建议使用 fp16。
- 社区有更省显存和更快速度的节点。 https://github.com/lihaoyun6/ComfyUI-SecNodes_Ultra_Fast。
- 经测试，ComfyUI-SecNodes_Ultra_Fast节点搭配fp16模型在我本地可以完美运行。然后在wan2.2 animate工作流里也正常运行。


## License
MIT License
转载请注明出处

## 最后更新：2025-10-20
## 项目地址：https://github.com/alex5241/ComfyUI-LowVRAM-Lab
