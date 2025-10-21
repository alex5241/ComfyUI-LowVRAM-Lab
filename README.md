# ComfyUI-LowVRAM-Lab
Experiments and optimizations for running ComfyUI on low-VRAM GPUs.

## 项目简介
本仓库是我在低显存显卡（4060Ti 8GB）上使用 ComfyUI 进行AI创作的笔记记录。

- 我在使用gpt或者deepseek解决comfyui相关的技术问题时，经常得到比较模糊或者不准确的回答。
- 我觉得可能是相关技术还比较新，而且各种大模型每月也都在更新，网络上还没有足够多的数据供给AI来抓取。
- 所以我创建了这个项目来自己记录。
- 本项目所有内容均为真实+原创+手打，希望我的项目对你有帮助。
- 本项目内容多为总结性质经验内容，并非新手教学方案。
- 本项目长期更新。但由于测试过程比较慢，更新不定期。

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
- 我的这套配置，在玩视频大模型，比如wan系列的时候，原模上都是无法跑起来的。
- 通常意义上，社区都会有量化或者蒸馏模型，比如gguf或者lightx2v系列。他们能让低配置的机器跑起来。
- 很多人以为只需要显存大就行，实际上内存也很重要，我32g物理内存+32g虚拟内存，经常占用双100%。长期跑AI，尤其是视频，建议48g内存起步。
- 大显存比高级别显卡重要。比如5060ti 16g，优于5070 12g.很多模型显存大了才能跑。
- 经常有显卡一样的人跑一套工作流，一个跑不了，一个能跑。可能就是一个内存大，显存通过block swap,用内存顶了一下。
- 我的配置也能玩，但是大多数情况下都是在降低分辨率或者花费更多等待时间，出来的效果也很一般。如果你想玩的爽，我不推荐我的配置。
- 我个人推荐的入门硬件配置是：intel cpu + 显卡16g + 内存 48g + 固态 2T 。这只是入门，如果没到这个配置也能玩，就像我一样。
- wan2.2 在技术上完全替代wan2.1，我现在已经删除了wan2.1的所有模型及工作流。
- wan2.5 风声大雨点小，10.1前开放的是收费模式，可能后期不会再开源。目前都在等消息。
- 如果现成的业务需要AI快速出东西，我建议直接在云平台如RH上玩，因为本地搭建环境还挺复杂的，尤其是对于非技术人员。
- 云平台的好处是不用搭建环境，直接就能跑。坏处是不能nsfw；需要付费；需要排队等待。本机跑的好坏反之。
- 社区里有很多做融合，加速等模型的，基本上每周都会出新模型，迭代速度非常快。comfyui也基本每天都会有新的内容更新。

## 关于环境：
- 整合包有自己的一套环境，都在整合包里。与电脑安装的python，torch等环境无关。
- 秋叶整合包目前最稳定的版本是v1.7。内置的是 torch 2.7.0+cu128 ,  python 3.11.
- 秋叶v2版本过高，部分python库或者节点库等没跟上适配，会有莫名其妙的问题，比如卡采样器，就是加速失效导致。
- 其他的整合包也大差不差，有的很大很全，内置了所有的节点和模型库，文件就会很大，20g的整合包也经常见。
- 整个加速依赖关系为  cuda->python->pytroch->xformers或sage（要依赖triton）或flashattention，各依赖之间必须版本匹配一致。
- 自行安装或者升级环境时，循序上面的依赖关系，并严格保持版本匹配进行下载安装,下方为一个比较稳定的版本示例：
  - pip list ,  python 3.11   
  - 先下python官网非exe的集成包，其内部没有pip，再加上pip，然后可用pip命令下载其他包。
  - pytorch  必须下带有cuda加速包 ,   一般整合包自带   2.7.0+cu128
  - xformers  命令行安装   0.0.30+4cf69f09.d20250606
  - triton 命令行安装  pip install -U "triton-windows<3.4"    triton-windows-3.3.1.post19 ，实际下载的是 triton_windows-3.3.1.post19-cp311-cp311-win_amd64.whl
  - sageattention2,  下载对应版本，本地安装。  2.1.1+cu128torch2.7.0
- 下载多版本选择时，windows/linux是平台，cp是python版本,tor是torch缩写，cu是cuda缩写，x86,arm是cpu类型,32 64是cpu位数，现在基本都是64位，都是x86_64,有些是amd64。
- 工作流层面的常用加速：
  - 双节棍  需要安装python库 https://github.com/nunchaku-tech/nunchaku/releases 和节点 https://github.com/nunchaku-tech/ComfyUI-nunchaku 同时需配合自己的大模型,所以每有一个大模型，双节棍就得基于官方出自己的模型库；
  - lightx2v: 蒸馏模型，需配合它自己的蒸馏模型或者lora ；
  - teacache 不用；
  - 其他自带优化加速的模型。
- 单独安装python包，需要在启动器内部打开命令行， python命令改为python.exe -m + pip命令 。  比如： pythone.exe -m pip install
- 基本上所有东西安装就三种，都是往model里扔东西，会了一个就都全会了：
  - 直接拷贝内容到对应目录即可。
  - 内容需要对应节点一起用，安装对应节点。内容可能不是一个，是多个； 或者内容需要py脚本等一堆，直接全下载拷进去。
  - 内容需要底层python库支持，先安，再下节点，再下内容。比如双节棍。

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

## 关于注意力加速
- 我一直没完全弄懂注意力加速到底该怎么开，但是我经常出问题的时候，就切一下就正常了。
- 图片处理开成xformers； 视频处理改成sageattention。
- 在模型都正常的情况下，如果采样器全黑，优先考虑切换到xformers试下。
- 很多视频工作流里是有切换到sageattention的内容的，运行时，会自动切换，不用在启动器里改动也可以。
- flash_attention_2： 只有linux版本,windows下无需关心。
- Flash Attention 仅在 FP16 / BF16 精度下可用，对于 FP32 / FP8 模型，README 明确写：“Automatically disabled for FP32/FP8 precision 。所以低配置电脑因为跑不起fp16的话，无需考虑。

## License
MIT License
转载请注明出处

## 最后更新：2025-10-20
## 项目地址：https://github.com/alex5241/ComfyUI-LowVRAM-Lab
