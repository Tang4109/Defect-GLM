# Defect-GLM

## 介绍
最近，建立在通用语料下预训练的多模态大模型，如GPT-4和ViusalGLM，在日常生活领域展现出了优越的问答与图像语义理解能力。这些模型强大的知识储备、图像和语义理解能力和逻辑推理能力，让人们看到了通用人工智能的希望。这使得多模态大模型在很多领域展示出了革命性的应用潜力，并且其有望变革工业设备智能运维领域的应用范式，推动构建智能化和自动化的工业设备运营维护系统，并降低设备监测、运营和维护所需要的人力成本。为实现这一点，基于VisualGLM模型，我们开发了Defect-GLM来探索多模态大模型在工业缺陷检测领域的研究和应用。此外，我们构建了一个大规模的图像-文本的半导体晶圆缺陷数据集，以作为Defect-GLM案例验证数据集。Defect-GLM在半导体晶圆缺陷识别与分析应用上表现出了非凡的潜力，在实验数据集中够达到96%的识别准确率。

## 本项目贡献

<div align=center>
  <img src='./examples/demonstrate_1.jpg'>
</div>

- 本项目探索了多模态大模型在工业缺陷检测领域的研究和应用，实现了基于多模态大模型的半导体晶圆缺陷识别和分析；
- 本项目借助开源晶圆数据集，并结合ChatGPT生成与人工设计的方式，构建了一个半导体晶圆缺陷诊断多模态数据集；
- 本项目使用所构建的晶圆多模态数据集在[VisualGLM-6B](https://github.com/THUDM/VisualGLM-6B)进行微调训练，并展示出了非凡的性能和潜力。
  
## 使用的开源数据集

- [Mixed-type Wafer Defect Datasets](https://www.kaggle.com/datasets/co1d7era/mixedtype-wafer-defect-datasets)是一个开源晶圆缺陷数据集，共包括38种不同缺陷类型的38015张晶圆图片。

- 注意该公开数据集的原始数据类型并不直接适用于多模态模型训练。因此需要对数据集种晶圆图片进行合适的预处理，并结合ChatGPT生成与人工设计等方式获得问答语料，最终结合语料与预处理晶圆数据才得到了可以训练的多模态微调数据集。

## 使用

### 安装依赖
```bash
# 安装依赖
pip install -r requirements.txt
# 也可使用阿里云镜像安装依赖
pip install -i https://mirrors.aliyun.com/pypi/simple/ -r requirements.txt
```
此时默认会安装`deepspeed`库（支持`sat`库训练），此库对于模型推理并非必要，同时部分`Windows`环境安装此库时会遇到问题。 如果想绕过`deepspeed`安装，我们可以将命令改为：
```bash
# 安装依赖
pip install -i https://mirrors.aliyun.com/pypi/simple/ -r requirements_wo_ds.txt
# 安装SwissArmyTransformer
pip install -i https://mirrors.aliyun.com/pypi/simple/ --no-deps "SwissArmyTransformer>=0.3.6"
```
### 权重下载

|模型权重|下载链接|微调方法|
|:-|:-|:-|
|checkpoint-WaferGLM-6000|<a href='https://huggingface.co/YefriL/WaferGLM'><img src='https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Spaces-blue'></a>|LoRA|

### CLI推理

```python
python cli_demo.py --from_pretrained checkpoints/checkpoint_WaferGLM_6000 --prompt_en 'What is the defect in this wafer map?' --english
```

### WebUI运行

```python
python web_demo.py --from_pretrained checkpoints/checkpoint_WaferGLM_6000
```

此时访问`http://127.0.0.1:7860`即可

## 效果展示

<img src='./examples/example_1.png'>
<img src='./examples/example_2.png'>
<img src='./examples/example_3.png'>
<img src='./examples/example_4.png'>


## 未来展望

1. 模型的能力更多来源于数据的支持，`OpenI-zh`作为微调数据集，其数据量足以支持研究，在更广泛的任务和性能表现上，我们认为**在大规模数据集上预训练并在高质量少量数据集上微调是一种可行的方案**；
2. 普遍意义的理解上，视觉多模态模型=视觉模型+语言模型。除了需要关注视觉模型信息与语言模型输出的搭配外，还需要**额外关注到语言模型的加强，在人机的对话中，尤其是医疗语言模型的问答上，除了专业的医疗问题回答，带有人文情怀的有温度的回答更应该是我们追寻的目标**。
3. **高精度的模型永远打不过大参数的模型**，如果在6B模型和13B模型上选择微调，请在资源充足情况下选择13B的大参数模型；

## 项目致谢

1. [VisualGLM-6B](https://github.com/THUDM/VisualGLM-6B)为我们提供了基础的代码参考和实现；
2. [MiniGPT-4](https://github.com/Vision-CAIR/MiniGPT-4)为我们这个项目提供了研发思路；
3. ChatGPT生成了高质量的中文版X光检查报告以支持XrayGLM训练；
4. [gpt_academic](https://github.com/binary-husky/gpt_academic)为文档翻译提供了多线程加速；
5. [MedCLIP](https://github.com/RyanWangZf/MedCLIP) 、[BLIP2](https://huggingface.co/docs/transformers/main/model_doc/blip-2) 、[XrayGPT](https://github.com/mbzuai-oryx/XrayGPT) 等工作也有重大的参考意义；

![](./assets/images/mpu.png)

这项工作由[澳门理工大学应用科学学院](https://www.mpu.edu.mo/esca/zh/index.php)硕士生[王荣胜](https://github.com/WangRongsheng) 、[段耀菲](https://github.com/IsBaSO4) 、[李俊蓉](https://github.com/lijunrong0815)完成，指导老师为檀韬副教授、[彭祥佑](http://www.patrickpang.net/)老师。

*特别鸣谢：[USTC-PhD Yongle Luo](https://github.com/kaixindelele) 提供了有3000美金的OpenAI账号，帮助我们完成大量的X光报告翻译工作

## 免责声明

本项目相关资源仅供学术研究之用，严禁用于商业用途。使用涉及第三方代码的部分时，请严格遵循相应的开源协议。模型生成的内容受模型计算、随机性和量化精度损失等因素影响，本项目无法对其准确性作出保证。即使本项目模型输出符合医学事实，也不能被用作实际医学诊断的依据。对于模型输出的任何内容，本项目不承担任何法律责任，亦不对因使用相关资源和输出结果而可能产生的任何损失承担责任。

## 项目引用

如果你使用了本项目的模型，数据或者代码，请声明引用：

```bash
@misc{wang2023XrayGLM,
      title={XrayGLM: The first Chinese Medical Multimodal Model that Chest Radiographs Summarization}, 
      author={Rongsheng Wang, Yaofei Duan, Junrong Li, Patrick Pang and Tao Tan},
      year={2023},
      publisher = {GitHub},
      journal = {GitHub repository},
      howpublished = {\url{https://github.com/WangRongsheng/XrayGLM}},
}
```

## 使用许可

此存储库遵循[CC BY-NC-SA](https://creativecommons.org/licenses/by-nc-sa/4.0/) ，请参阅许可条款。

